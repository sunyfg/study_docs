## webpack项目迁移到 vite 的坑

1. **首先在本地创建一个空的 vite 项目**

```bash
npm create vite@latest

# 选择模板：
# 	1.填写项目名称
# 	2.选择 react 框架
# 	3.选择 typescript 语言
```

2. **把原项目中的源代码 copy 到 vite 项目中**

   清空 vite 模板项目 src 中的内容，然后把原项目 `src` 文件夹中的所有源代码 copy 到 vite 项目的 `src` 中；

3. **迁移 package.json 中的依赖配置**

   主要把原 package.json 中的 `devDependencis` 和 `dependencis`  配置，以增量的形式追加到 vite 项目package.json对应的配置中。

   注：vite 模板项目安装的 `react` 和 `react-dom` 版本是否和原项目的一样。如果不一样，尽量先使用原项目中的版本。减少迁移过程中可能会遇到的其他坑。

4.  **less & scss**

   一般项目中都使用了 css 预处理器，迁移到 vite 后，需要手动安装下依赖，只安装对应的处理器即可。

   ```bash
   # 安装 less 处理器
   npm install less -D
   # 安装 sass 处理器
   npm install sass -D
   ```

   遇到的问题：

   这里要注意下安装的处理器版本：我在安装 sass 处理器后，运行 `npm run dev` 后，遇到一个报错提示不让使用 `$height / 2` 这样的语法， 需要用 `calc()` 包裹。

   解决：查看原webpack项目 sass-loader 版本，在 github 对应的版本里找到它依赖的 sass 版本是多少，在 vite 项目中安装对应版本的 sass 包。

5. **把 tsconfig.json 中的配置迁移到vite项目中**

   需要打开 `isolatedModules: ture` ，这是因为 `esbuild` 只执行没有类型信息的转译，它并不支持某些特性，如 `const enum` 和隐式类型导入。这样的话可能会造成一些问题。

   **问题1**：`Re-exporting a type when 'isolatedmodules' is enabled requires using 'export type'` 

   在第二次 export 出类型的时候，需要至添加 `type` 关键字。

   举个例子：

   ```ts
   // 文件 ./type.ts
   export interface MyType {
       name: string
       age: number
   }
   
   // 文件 ./index.ts
   // 这样写会报错
   export { MyType } from './type.ts';
   
   // 这样写就OK了
   export type { MyType  } from './type.ts' 
   ```

   问题2： vite 不支持 `const enum` 声明枚举变量。

   这个就没办法了，目前看 vite 官网上也是明确表达了不支持 `const enum`  和隐式类型导入。这一下子就把我给劝退了，因为webpack项目中写了大量的枚举变量。（这块不清楚能否通过自定义 plugin 来支持）



因为 `const enum ` 的问题，导致没法继续搬迁到 vite，因此本次先记录下来，作为一个经验。