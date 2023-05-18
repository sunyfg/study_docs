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

## loader

由于webpack 本身只能打包commonjs规范的js文件，所以，针对css，图片等格式的文件没法打包，就需要引入第三方的模块进行打包。

loader虽然是扩展了 webpack ，但是它只专注于转化文件（transform）这一个领域，完成压缩，打包，语言翻译。

仅仅只是为了打包、仅仅只是为了打包、仅仅只是为了打包！

file-loader 可以指定要复制和放置资源文件的位置，以及如何使用版本哈希命名以获得更好的缓存

url-loader 允许你有条件地将文件转换为内联的 base-64 URL (当文件小于给定的阈值)，这会减少小文件的 HTTP 请求数。如果文件大于该阈值，会自动的交给 file-loader 处理。

## plugin

目的在于解决loader无法实现的其他事，从打包优化和压缩，到重新定义环境变量，功能强大到可以用来处理各种各样的任务。

webpack提供了很多开箱即用的插件：CommonChunkPlugin主要用于提取第三方库和公共模块，避免首屏加载的bundle文件，或者按需加载的bundle文件体积过大，导致加载时间过长，是一把优化的利器。而在多页面应用中，更是能够为每个页面间的应用程序共享代码创建bundle。

## 从运行时机的角度区分

* loader 运行在打包文件之前
* plugin 在整个编译周期都起作用
