## 提高构建速度

### 给 loader 减轻负担

用 include 或 exclude 来帮我们避免不必要的转译：

```js
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
  ]
}
```

开启缓存将转译结果缓存至文件系统：

```js
loader: 'babel-loader?cacheDirectory=true'
```

### 使用 Happypack 将 loader 由单进程转为多进程

webpack 的缺点是单线程的，我们可以使用 Happypack 把任务分解给多个子进程去并发执行，大大提升打包效率。配置的方法是把 loader 的配置转移到 HappyPack 中去。

```js
const HappyPack = require('happypack')
// 手动创建进程池
const happyThreadPool =  HappyPack.ThreadPool({ size: os.cpus().length })

module.exports = {
  module: {
    rules: [
      ...
      {
        test: /\.js$/,
        // 问号后面的查询参数指定了处理这类文件的HappyPack实例的名字
        loader: 'happypack/loader?id=happyBabel',
        ...
      },
    ],
  },
  plugins: [
    ...
    new HappyPack({
      // 这个HappyPack的“名字”就叫做happyBabel，和楼上的查询参数遥相呼应
      id: 'happyBabel',
      // 指定进程池
      threadPool: happyThreadPool,
      loaders: ['babel-loader?cacheDirectory']
    })
  ],
}
```

### DllPlugin 提取公用库

开发过程中，我们经常需要引入大量第三方库，这些库并不需要随时修改或调试，我们可以使用DllPlugin和DllReferencePlugin单独构建它们。具体使用如下：

1.配置webpack.dll.config.js：

```js
// build/webpack.dll.config.js
var path = require("path");
var webpack = require("webpack");
module.exports = {
 // 要打包的模块的数组
 entry: {
    vue: ['vue/dist/vue.js', 'vue', 'vue-router', 'vuex'],
    comment: ['jquery', 'lodash', 'jquery/dist/jquery.js']
 },
 output: {
  path: path.join(__dirname, '../static/dll'), // 打包后文件输出的位置
  filename: '[name].dll.js',// vendor.dll.js中暴露出的全局变量名。
  library: '[name]_library' // 与webpack.DllPlugin中的`name: '[name]_library',`保持一致。
 },
 plugins: [
  new webpack.DllPlugin({
   path: path.join(__dirname, '.', '[name]-manifest.json'),
   name: '[name]_library', 
   context: __dirname
  }),
 ]
};
```

2.在 package.json 的 scripts 里加上：`"dll": "webpack --config build/webpack.dll.config.js"`

3.运行npm run dll 在 static/js 下生成 `vendor-manifest.json`

4.在 build/webpack.base.conf.js 里加上:

```js
 // 添加DllReferencePlugin插件
 plugins: [
  new webpack.DllReferencePlugin({
   context: __dirname,
   manifest: require('./vendor-manifest.json')
  })
 ],
```

5.最后在 index.html 中引入 vendor.dll.js

```html
<div id="app"></div>
<script src="./static/dll/vue.dll.js"></script>
<script src="./static/dll/comment.dll.js"></script>
```

### externals 选项

也可以使用 externals 让 webpack 不打包某部分，然后在其他地方引入 cdn 上的 js 文件，利用缓存下载 cdn 文件达到减少打包时间的目的。配置 externals 选项：

```js
// webpack.prod.config.js
...
module.exports = {
    externals: {
        'vue': 'window.Vue',
        'vuex': 'window.Vuex',
        'vue-router': 'window.VueRouter'
        ...
    }
}
```

配置 externals 之后，webpack 不会把配置项中的代码打包进去，别忘了需要在外部引入 cdn 上的 js 文件

```html
<body>
  <script src="XXX/cdn/vue.min.js"></script>
  ......
</body>
```

### 代码分割 splitChunks

- 业务代码和第三方库分离打包，实现代码分割；
- 业务代码中的公共业务模块提取打包到一个模块；
- 第三方库最好也不要全部打包到一个文件中，因为第三方库加起来通常会很大，我会把一些特别大的库分别独立打包，剩下的加起来如果还很大，就把它按照一定大小切割成若干模块。

### 删除冗余代码 tree-shaking

一个比较典型的应用，就是Tree-Shaking。

### UI 库 按需加载

以Element-ui为例，UI框架通过借助babel-plugin-component，我们可以只引入需要的组件，以达到减小项目体积的目的。

### 懒加载

结合 Vue/React 的异步组件和 Webpack 的代码分割功能，轻松实现路由组件的懒加载。

```js
const Foo = () => import("./Foo.vue");
```

