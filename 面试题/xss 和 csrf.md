https://juejin.cn/post/6844903781704925191#heading-13

https://juejin.cn/post/6844903685122703367

# 前端安全系列（一）：如何防止XSS攻击？

Cross-Site Scripting（跨站脚本攻击）简称 XSS，是一种**代码注入攻击**。

https://juejin.cn/post/6844903685122703367#heading-15

## XSS 类型

1. **存储型**： 用户输入内容 =》 提交到服务器 =》 其他用户访问内容 =》 浏览器执行恶意代码 =》 被攻击。
2. **反射型**： 用户访问拼接恶意代码的URL =》 服务端把URL参数组装到HTML中 =》 返回带有恶意代码的HTML =》 被攻击。
3. **DOM型**： 用户访问拼接恶意代码的URL =》  前端JavaScript取出恶意参数并执行 =》 被攻击



## 检查网站是否有XSS攻击风险

1. 手动

2. 通过借用第三方扫描工具



## XSS 防范

1. **输入过滤**： 缺点是攻击者可以绕过前端直接攻击接口。
2. **预防存储型和反射型XSS**
   1. 改成纯前端渲染，把代码和数据拆分开。（反过来想，所以如果是react的服务端渲染，则会出现存储型和反射型XSS攻击）
   2. 对 HTML 做充分的转义： 使用成熟的转义库
3. **预防DOM型XSS**
   1. 用 `.textContent` `.setAttribute` 代替 `.innerHTML` `.outerHTML` `document.write()` 
4. **其他方法**
   1. **开启 CSP (Content Security Policy)**
      1. 禁止加载外域代码，防止复杂的攻击逻辑。
      2. 禁止外域提交，网站被攻击后，用户的数据不会泄露到外域。
      3. 禁止内联脚本执行（规则较严格，目前发现 GitHub 使用）。
      4. 禁止未授权的脚本执行（新特性，Google Map 移动版在使用）。
      5. 合理使用上报可以及时发现 XSS，利于尽快修复问题。
   2. **控制输入内容的长度**
   3. **http-only cookie**
   4. **验证码： 防止脚本代替用户自动提交危险操作。**





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

