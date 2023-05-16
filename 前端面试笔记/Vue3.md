# 面试题

* Vue3 比 Vue2 有什么优势？
* 描述 Vue3 生命周期
* 如何看待 Composition API 和 Options API?
* 如何理解 ref toRef 和 toRefs？
* Vue3 升级了哪些重要的功能？
* Composition API 如何实现代码逻辑复用？
* Vue3 如何实现响应式？
* watch 和 watchEffect 的区别是什么？
* setup 中如何获取组件实例？
* Vue3 为何比 Vue2 快？
* Vite 是什么？
* Composition API 和 React Hooks 的对比？

## Vue3 比 Vue2 有什么优势？

* 性能更好
* 体积更小
* 更好的 ts 支持（Vue3 ts开发）
* 更好的代码组织
* 更好的逻辑抽离
* 更多新功能

## Vue3 生命周期

变动

* beforeDestroy 改为 beforeUnmount
* destroyed 改为 unmounted
* 其它沿用 Vue2 生命周期

生命周期函数

* setup 代替 beforeCrate created，在之前执行
* onBeforeMount
* onMounted
* onBeforeUpdate
* onUpdated
* onBeforeUnmount
* onUnmounted

## Composition API 对比 Options API

### Composition API 带来了什么？

* 更好的代码组织
* 更好的逻辑复用
* 更好的类型推导