# 考点

* 基本配置
* 高级配置
* 优化打包效率
* 优化产出代码
* 构建流程概述
* babel

# 面试题

* 前端代码为何要进行构建和打包？
* module chunk bundle 分别什么意思，有何区别？
* loader 和 plugin 的区别？
* webpack 如何实现懒加载？
* webpack 常见性能优化？
* babel-runtime 和 babel-polyfill 的区别

# webpack5

调整

* dev-server 命令修改：`dev: webpack serve --config build/webpack.dev.js`
* 升级新版本：`const {merge} = require('webpack-merge')`
* 升级新版本：`const { CleanWebpackPlugin } = require('clean-webpack-plugin')`
* `module.rules` 中 `leader: ['xxx-loader']` 换成 `use: ['xxx-loader']`
* `filename: 'bundle.[contenthash:8].js'` 其中 `h` 小写，不能大写

# webpack 性能优化

* 优化打包构建速度 - 开发体验和效率
* 优化产出代码 - 产品性能

## 构建速度

* 优化 babel-loader
  * ?cacheDirectory 开启缓存
  * include exclude 确定范围
* IgnorePlugin 避免引入无用的代码
  * `new webpack.IgnorePlugin()`
  * 直接不引入，代码中没有
  * 减少产出体积
* noParse 避免重复打包
  * `module.noParse`
  * 引入，但不打包
* happyPack 多进程打包
  * JS 单线程，开启多进程打包
  * 提高构建速度
  * 第三方包：happypack
* parallelUglifyPlugin
* 自动刷新
* 热更新
* DllPlugin - 针对比较大的第三方库，提前打包好，然后引用

# production

`mode: production`

* 自动开启代码压缩
* Vue React 会自动删掉调试代码，如开发环境的 warning
* 自动启动 Tree-Shaking

# ES6 Module 和 Commonjs 区别

* ES6 Module 静态引入，编译时引入
* Commonjs 动态引入，执行时引入
* 只有 ES6 Module 才能静态分析，实现 Tree-Shaking

# module chunk bundle 分别什么意思，有何区别？

* module - 各个源码文件，webpack 中一切皆模块
* chunk - 多模块合并成的，如 entry import() splitChunk
* bundle - 最终的输出文件

# loader 和 plugin 区别

