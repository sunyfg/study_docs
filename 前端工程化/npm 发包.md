# npm 发包

### 发包流程 

1. 首先要有 npm 账号，到 npmjs.com 上注册。
2. 在电脑上首次发包，需要执行 `npm adduser` 添加一个可发包的 npm 账号。非首次，可以使用 `npm login` 进行登录。
3. 执行 `npm whoami` 可以查看当前登录的用户是谁。
4. 在 package.json 中做好相关配置。参考博客：https://juejin.cn/post/7093873173128544270#heading-0
5. npm publish



### 注意事项

package.json 中，name 和 version 是必须的。

#### name

name必须唯一，可以通过 `npm view <name>` 快速检测你的包名是否已存在。

#### version

版本号必须按照 [node-semver](https://github.com/npm/node-semver) 规范书写。

#### bin

定义包中，可执行的文件。

- 如果全局安装了此包，`say-hello` 命令会添加到 `bin` 目录。
- 如果是在项目中安装了此包，可以通过 `npm exec say-hello`  或 `npx say-hello` 运行此文件；或者也可以在 `"scripts"` 里面直接使用 `say-hello` 命令。

```json
{
  "bin": {
    "say-hello": "sayHello.js"
	}
}
```

如何包名 name 和 命令相同，则可以简写：(后续就可以通过 npm exec say-hello 执行文件)

```json
{
  "name": "say-hello",
  "bin": "sayHello.js"
}
```

> Tips: 执行文件首行要加上 Shebang 指定脚本文件的解释程序：

```js	
#!/usr/bin/env node

console.log('hello');
```

可以查看 create-react-app 的可执行文件：https://github.com/facebook/create-react-app/blob/main/packages/create-react-app/index.js