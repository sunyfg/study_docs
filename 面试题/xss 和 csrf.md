https://juejin.cn/post/6844903781704925191#heading-13

https://juejin.cn/post/6844903685122703367

# 前端安全系列（一）：如何防止XSS攻击？

Cross-Site Scripting（跨站脚本攻击）简称 XSS，是一种**代码注入攻击**。

https://juejin.cn/post/6844903685122703367#heading-15

# 前端安全系列之二：如何防止CSRF攻击？

跨站请求伪造（英语：Cross-site request forgery）

https://juejin.cn/post/6844903689702866952#heading-20

### 特点

* CSRF（通常）发生在第三方域名
* CSRF攻击者不能获取到 Cookie 等信息，只是使用

针对这两点，我们可以专门制定防护策略，如下：

* 阻止不明外域的访问
  * 同源检测
  * Samesite Cookie
* 提交时要求附加本域才能获取的信息
  * CSRF Token
  * 双重Cookie验证

### 防护策略

