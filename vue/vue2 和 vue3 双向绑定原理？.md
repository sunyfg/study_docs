## 为什么要取代 Object.defineProperty？



## Vue2 中使用 Object.defineProperty() 的缺陷

* Object.defineProperty 递归遍历所有对象的所有属性，当数据层级较深时，会造成性能影响
* Object.defineProperty 只能作用在对象上，不能作用在数组上
* Object.defineProperty 只能监听定义时的属性，不能监听新增属性
* 由于 Object.defineProperty 不能作用于数组，vue2 选择通过重写数组方法原型的方式对数组数据进行监听，但是仍然无法监听数组索引的变化和长度的变更