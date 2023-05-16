* http 常见状态码
* 常见的 header
* restful api
* http 缓存机制（重要）

# http 状态码

## 分类

* 1xx 服务器收到请求
* 2xx 请求成功
* 3xx 重定向
* 4xx 客户端错误
* 5xx 服务端错误

## 常见状态码

* 200 成功
* 301 永久重定向，配合location，浏览器自动处理
* 302 临时重定向，配合location，浏览器自动处理
* 304 资源未被修改
* 404 资源未找到
* 403 没有权限
* 500 服务器错误
* 504 网关超时

## 关于协议和规范

* 就是一个约定
* 要求大家都跟着执行
* 不要违反规范

# http methods

## 传统 methods

* get 获取服务器数据
* post 向服务器提交数据

## 现在 methods

* get 获取数据
* post 新建数据
* patch/put 更新数据
* delete 删除数据

## Restful API

* 一种新的 API 设计方法
* 传统 API 设计：把每个 url 当做一个功能
* Restful API 设计：把每个 url 当做一个唯一的资源

## 如何设计成一个资源？

* 尽量不用 url 参数
* 用 method 表示操作类型

# http headers

## Request Headers

* Accept 浏览器可接收的数据格式
* Accept-Encoding 浏览器可接收的压缩算法，如 gzip
* Accept-Languange 浏览器可接收的语言，如 zh-CN
* Connection: keep-alive 一次 TCP 链接重复使用
* Cookie
* Host
* User-Agent（UA）浏览器信息
* Content-Type 发送数据的格式，如 application/json

## Response Headers

* Content-type 返回数据的格式，如 application/json
* Content-length 返回数据的大小，多少字节
* Content-Encoding 返回数据的压缩算法，如 gzip
* Set-Cookie

## 自定义 header

## 缓存相关的 headers

* Cache-Control
* Expires
* Last-Modified
* If-Modified-Since
* Etag
* If-None-Match

# http 缓存

## 关于缓存

* 什么是缓存？
* 为什么需要缓存？
* 哪些资源可以被缓存？- 静态资源（js css img）

## 强制缓存

![image-20230513233023832](/Users/sunyanfeng/Library/Application Support/typora-user-images/image-20230513233023832.png)

### Cache-Control

* Response Headers 中
* 控制强制缓存的逻辑
* 例如 Cache-Control: max-age=31536000（单位秒）

### Cache-Control 的值

* max-age 缓存过期时间
* no-cache 不用本地缓存
* no-store 不用本地缓存，也不用服务端缓存措施
* private 只允许最终用户做缓存
* public 允许中间的路由、代理等做缓存

### Expires

* 同在 Response Headers 中
* 同为控制缓存过期
* 已被 Cache-Control 代替

## 协商缓存

* 服务器端缓存策略
* 服务器判断客户端资源，是否和服务端资源一样
* 一致则返回 304，否则返回 200 和最新的资源

![image-20230513233833670](/Users/sunyanfeng/Library/Application Support/typora-user-images/image-20230513233833670.png)

### 资源标识

* 在 Response Headers 中，有两种
* Last-Modified 资源的最后修改时间
* Etag 资源的唯一标识（一个字符串，类似人类的指纹）
* 如果两者同时存在，优先使用 Etag
* Last-Modified 只能精确到秒级
* 如果资源被重复生成，而内容不变，则 Etag 更精准

![image-20230513234156921](./img/image-20230513234156921.png)

![image-20230513234417061](./img/image-20230513234417061.png)

## 完整流程图

![image-20230513234828519](./img/image-20230513234828519.png)

## 三种刷新操作

* 正常操作：地址栏输入地址
  * 强制缓存有效，协商缓存有效
* 手动刷新
  * 强制缓存失效，协商缓存有效
* 强制刷新：ctrl + f5
  * 强制缓存失效，协商缓存失效

# https

* http 明文传输，敏感信息容易被拦截
* https = http + 加密

## 加密方式

* 对称加密：一个 key 同时负责加密和解密
* 非对称加密：一个 key 加密，一个 key 解密
* https 同时用到了这两种加密方式

## https 证书

* 中间人攻击 - 调包
* 使用第三方证书 - 避免中间人攻击
* 浏览器校验证书

![image-20230514000547335](./img/image-20230514000547335.png)

