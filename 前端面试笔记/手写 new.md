# 手写new

首先需要知道 new 都做了什么事情，才有可能自己实现一遍。



## new做了以下几件事情：

1. 创建了一个新的对象；
2. 新对象继承父类原型上的方法；
3. 执行父类的构造函数，并把this指向新的对象（添加父类的属性到新对象上并初始化），保存方法执行的结果；
4. 判断执行结果是对象则返回对象，否则返回新对象。



## 自己实现

```js
function myNew(father, ...rest) {
	// 创建一个新对象A，并且其原型__proto__ 指向 father.prototype
  const newObj = Object.create(father.prototype);
  
  // 执行父类构造函数，并把 this 指向新对象。新对象继承父类的属性；
  // 把执行结构存起来。
  const result = father.apply(newObj, rest);
  
  // 判断父类构造函数执行结果是否为对象，对象则返回，否则返回newObj。
  return typeof result === 'object' ? result : newObj;
}

```

