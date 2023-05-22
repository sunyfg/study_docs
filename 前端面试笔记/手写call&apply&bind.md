# 手写call、apply、bind

> 引用：https://juejin.cn/post/6962037968013885448

实现思想：

call/apply/bind 都是用来改变函数体内的 this 指向的。我们都知道`谁调用函数，this 就指向谁`。 

那大体思路就是，把函数`fn`设置成某一个对象`obj`的属性，然后执行 `obj.fn()`  不就把 `this` 指向到 `obj` 了么。



## call

call 的语法：`function.call(thisArg, arg1, arg2, ...)`

```js
Function.prototype.myCall = function(thisArg, ...rest) {
  const context = thisArg ?? window;
  context.fn = this;
	const result = context.fn(...rest);
  delete context.fn;
  return result;
}
```



## apply

apply 语法： `function.apply(thisArg)` `function.apply(thisArg, argsArray)`

```js
Function.prototype.myApply = function(thisArg, argsArray = []) {
	const context = thisArg ?? window;
  context.fn = this;
  if (!Array.isArray(argsArray)) {
    	throw new Error('第二个参数请传入一个数组类型')
	}
  const result = context.fn(...argsArray)
  delete context.fn;
  return result;
}
```



## bind

**`bind()`** 方法创建一个新的函数，在 `bind()` 被调用时，这个新函数的 `this` 被指定为 `bind()` 的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

bind 语法： `function.bind(thisArg[, arg1[, arg2[, ...]]])`

```js
Function.prototype.myBind = function(thisArg, ...args) {
	const context = thisArg ?? window;
  context.fn = this;
  
  return function(...args2) {
    const args = [...args, ...args2];
    const result = context.fn(...args);
    delete context.fn;
    return result;
	}
}

```

