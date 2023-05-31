# link 和 @import 的区别

* `link` 属于 `xhtml` 标签，而 `@import` 完全是 `css` 提供的一种方式
* `link` 链接的内容是和 `html` 页面同步加载的，`@import` 需要等到页面全部加载完成之后才能加载
* `@import` 是 CSS2.1 提出的，所以老的浏览器不支持，只有在 IE5 以上的才能识别，而 `link` 标签无兼容性问题
* `link` 能加载 `css`、`javascript`，`@import` 只能加载 `css` 

总结：

* `link` 的权重高于 `@import` 
* `link` 属于 HTML 标签所以不受兼容性的影响，`@import` 属于 `css` 标签，只能加载 `css` 样式。
* `link` 除了加载 `css` 外，还能用于定义 RSS、定义 rel 连接属性等作用；而 `@import` 是 `css` 提供的，只能用于加载 `css`