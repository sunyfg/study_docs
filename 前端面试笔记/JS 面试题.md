# JS 面试题

## for-in 和 for-of 有什么区别？

* for-in 遍历得到 key
* for-of 遍历得到 value

### 适用于不同的数据类型

* 遍历对象：for...in 可以，for...of 不可以
* 遍历 Map Set：for...of 可以，for...in 不可以
* 遍历 generator：for...of 可以，for...in 不可以

### 可枚举 vs 可迭代

* for...in 用于可枚举数据
  * 对象、数组、字符串
  * 可到 key
* for...of 用于可迭代数据
  * 数组、字符串、Map、Set
  * 可到 value
  * obj[Symbol.iterator] 属性存在

## for await...of 有什么作用？

* for await...of 用于遍历多个 Promise

```js
const promiseList = [p1, p2, p3];

for await (let res of promiseList) {
  console.log(res)
}
```

