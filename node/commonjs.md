## exports 是 module.exports 的引用。

重新给 exports 赋值，会导致 exports 将不再是 module.exports 的引用。

> exports 的有效用法：

```js
// 示例1： ✅
exports.fn = function() {}

// 示例2： ❎
// exports 不再是 module.exports 的引用。无法真正导出任何内容。
exports = {
  fn: function() {}
}

// ✅
// 如果想用示例2，给 exports 赋值后，需要再把exports 赋值给 module.exports
module.exports = exports = {
  fn: function() {}
}
```

**推荐以下写法：**

```js
// 写法1
exports.fn1 = function() {}
exports.fn2 = function(){}
exports.obj = {}
//  这个时候就不能 module.exports = something; 因为这样会覆盖 exports。导致只导出了 something, 但是可以 module.exports = exports;


// 写法2
module.exports = function() {};
module.exports.otherOjb = {};
module.exports.otherFn = function() {};

// 写法3
module.exports = exports = {
  fn: function() {}
  obj: {}
	//...
}
```





## 被 module.exports 导出后，如何使用

./child.js

```js
module.exports = function() {
  return 'hello world';
}

module.exports.subFn = function() {
	return '我是 subFn';
}
```

./parent.js

```js
// require 进来的就是 module.exports 导出的。
const fn = reuqire('./child.js')

fn(); // 'hello world'

fn.subFn(); // '我是 subFn'
```



总之，exports 不是很安全的，尽量使用 module.exports 导出模块内容。

```js
// balabala ....

module.exports = {
  // balabala...
}
```



