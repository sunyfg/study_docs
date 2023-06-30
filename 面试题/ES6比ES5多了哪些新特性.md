* 新增 let 和 const 命令
* 解构赋值
* Set 和 Map
* 模板字符串
* 展开运算符
* 箭头函数
* Promise
* async await
  * ES2017
* Module 语法 模块化
  * export 
  * import
* Proxy
* Class 类

## Proxy

```js
var obj = new Proxy({}, {
  get: function (target, propKey, receiver) {
    console.log(`getting ${propKey}!`);
    return Reflect.get(target, propKey, receiver);
  },
  set: function (target, propKey, value, receiver) {
    console.log(`setting ${propKey}!`);
    return Reflect.set(target, propKey, value, receiver);
  }
});
```

