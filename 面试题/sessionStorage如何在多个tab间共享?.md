# sessionStorage 如何在多个tab间共享?

* 同源（协议、域、端口）
* 假设在A页面中有 sessionStorage
* B页面必须是在A页面中使用`window.open(url)`方法打开，才能在B页面中使用A页面的sessionStorage