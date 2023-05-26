# http网络缓存，Max-Age是http哪个版本的?

http1.1中我们有了expires的完全替代首部 cache-control: max-age

### Expires（HTTP/1.0）

Expires描述的是一个绝对时间，由服务器返回，用GMT格式的字符串表示。

由于expires是一个绝对时间，如果人为的更改时间，会对缓存的有效期造成影响，使缓存有效期的设置失去意义。因此在http1.1中我们有了expires的完全替代首部cache-control：max-age

### Cache-Control(HTTP/1.1)

max-age值是一个相对时间，它定义了文档的最大使用期——从第一次生成文档到文档不再新鲜、无法使用为止，最大的合法生存时间(以秒为单位)。

#### 过程说明

- 当我们首次请求资源时，服务器在返回资源的同时，会在Response Headers中写入expires首部或cache-control，标识缓存的过期时间，缓存副本会将该部分信息保存起来。
- 当再次请求该资源的时候，缓存会对date(Date 是一个通用首部，表示原始服务器消息发出的时间。即表示的是首次请求某个资源的时间。)和expires/cache-control的时间进行对比，从而判断缓存副本是否足够新鲜。