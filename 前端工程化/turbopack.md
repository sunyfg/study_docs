# Turbopack

Turbopack 是一个使用 Rust 语言开发的打包器，Webpack 的作者来到新公司后进行开发的。



## 为什么开发 Turbopack ，而不是使用 vite ？

vite 是一个基于 esbuild 的打包工具，并依赖于浏览器的原生 `ES Module` 系统。这种速度很快，但是对于许多模块组成的大型应用程序，vite 可能会遇到扩展问题。浏览器中大量的级联网络请求可能会导致启动时间相对较慢。

**Turbopack 的开发者认为，有两种方法可以加快这个过程：`do less work`  and `do work in parallel`.**  这两件事情都要抓住去做。turbo引擎会缓存它调度的所有函数的结果，这意味着它永远不需要做两次相同的工作。简单来说，它以最大的速度做最小的工作。

Esbuild 是一个非常快速的打包工具，但是 Tubopack 不使用它的原因有三点：

1. Esbuild 本身没有 **HMR**，这在 dev server 上是比较难以接受的。
2. Esbuild 虽然打包很快，但是它没有做太多**缓存**。
3. Esbuild 不支持“**懒惰打包**”



## Lazy bundling(懒惰打包)

“懒打包” 是一把让本地服务变的更快的钥匙。esbuild 没有“懒打包”这一概念，不是打包所有就是一个也不打包，除非你专门针对某些入口点。

#### 懒惰打包是什么？

就比如：你在本地服务访问 `localhost:3000` 页面，那打包器只打包 /src/index.jsx 文件以及它依赖的模块。

懒惰打包这个方法可以让 Turbopack `do less work` .



## 总结

Turbopack 的致力于实现：

1. 作为一个打包器，当工作在一个大型应用程序中时，可以胜过原生 ESM 方式。
2. 使用增量计算。Turbo 引擎给 Turbopack 架构的核心带来了：最大化速度，最小化工作量。
3. 优化本地开发服务的启动时间。我们构建了一个"懒惰的资源图"（a lazy asset graph）只计算所请求的资源。



## 参考

https://turbo.build/pack/docs/why-turbopack



