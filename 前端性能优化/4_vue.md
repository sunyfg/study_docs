# Vue 性能优化

* 体积优化
* 传输优化
* 感知性能优化
* 源码优化
* 开发、打包编译优化

## 体积优化
* 图片、js、css 压缩

  * webpack `image-webpack-loader ` 解析器，压缩图片
  * webpack `css-loader` 解析器，压缩css
  * webpack `uglifyjs-webpack-plugin` 插件，压缩 js，同时打包中可以去除代码中 console.log、debugger语句
* 采用 `Tree Shaking` 技术，对没有用到的插件、api不打包到最终打包后的文件中去。

  * 基于ES6中（import与exports）模板语法
  * 通常用于移除 JavaScript 上下文中，已经export，但是没有地方import的代码
* `Code Splitting` ，将代码按 `路由维度`或者`组件` 分块(chunk)，这样做到按需加载，同时可以充分利⽤浏览器缓存

## 传输优化

* 路由懒加载

  * 当打开页面时，才去加载对应文件
  * `component: () => import(/\* webpackChunkName: "user" \*/ '@/views/user/register')`
* 开启 HTTP2
  * 通常浏览器在传输时，**并发请求数是有限制的**，超过限制的请求需要排队。使用HTTP2协议后，其可以在一个TCP连接中，处理多个请求（多路复用），从而不受此限制。如果网站支持HTTPS，一定要开启HTTP2，对于请求多的页面提升很大，尤其是在网速不佳时。


**Nginx开启HTTP2**

```nginx
// nginx.conf
listen 443 http2;
```

* 开启 `Gzip压缩`，缩小 TCP 传输时文件的体积

* 预加载，link标签的rel属性的两个可选值：

  * Prefetch：预请求，是为了提示浏览器，`用户未来的页面有可能需要加载目标资源`，所以浏览器有可能通过事先获取和缓存对应资源，优化用户体验。
  * Preload：预加载，表示`用户十分有可能需要在当前浏览中加载目标资源`，所以浏览器必须预先获取和缓存对应资源。
  * 对比：
    * preload 和 prefetch 都没有同域名的限制；
    * preload 主要用于预加载当前页面需要的资源；而 prefetch 主要用于加载将来页面可能需要的资源；
    * 不论资源是否可以缓存，prefecth会存储在net-stack cache中至少5分钟；
    * preload 需要使用as属性指定特定的资源类型以便浏览器为其分配一定的优先级，并能够正确加载资源；

* 托管至OSS + CDN加速

## 感知性能优化

* 白屏时的loading动画
  * 减轻等待焦虑

* 首屏先显示骨架，再加载真实内容。
* 针对非首屏
  * 非首屏的图片懒加载
  * 加载占位图 ---- 先全局通用loading图，或者用CSS填充色块，图片加载完成后替换为原图
  * 组件懒加载
  * 路由跳转 Loading 动画


## 源码优化

* for 循环设置 key 值
* 使用 `keep-alive`，对组件进行缓存，从而节省性能 

## 开发、打包编译优化

查看 [5_webpack.md](./5_webpack.md) 文件

