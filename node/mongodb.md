# 链接 mongodb 数据库

```js
const Koa = require("koa");
const Router = require("koa-router");
const koaBody = require("koa-body");

const mongoose = require("mongoose");
const { connectionStr } = require("./vue.config.js");
mongoose.connect(connectionStr, (err) => {
 if (err) console.log("mongonDB连接失败了");
  console.log("mongonDB连接成功了");
});

//koa实例化
const app = new Koa();
const router = new Router();

// 总路由添加前缀/api,总地址变为http://localhost:3000/api
router.prefix("/api");
router.get("/", async (ctx) => {
  ctx.body = "hello World";
});

app.use(koaBody());
app.use(router.routes()).use(router.allowedMethods());

app.listen(3000, () => {
  console.log("服务启动了");
});
```

