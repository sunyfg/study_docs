# React Hooks

**React Hooks是React 16.8版本中引入的新特性**

## Hooks 出现本质上原因

* 让函数组件也能做类组件的事，有自己的状态，可以处理一些副作用，能获取 ref ，也能做数据缓存
* 解决逻辑复用难的问题
* 放弃面向对象编程，拥抱函数式编程

## **为什么要使用自定义 Hooks**?

自定义 hooks 是在 React Hooks 基础上的一个拓展，可以根据业务需求制定满足业务需要的组合 hooks ，更注重的是逻辑单元。通过业务场景不同，到底需要React Hooks 做什么，怎么样把一段逻辑封装起来，做到复用，这是自定义 hooks 产生的初衷。

## 分类

**数据更新驱动**，**状态获取与传递，执行副作用，状态派生与保存，和工具类型**

### 数据更新驱动

* useState
* useReducer

  * 订阅状态，创建 reducer 更新视图
  * 类似redux的功能 api
* useSyncExternalStore
  * React v18 

* useTransition
  * React v18 

* useDeferredValue
  * React v18 


### 执行副作用

* useEffect
  * 异步调用，视图更新后，执行副作用
  * 弥补函数组件没有生命周期的缺陷
* useLayoutEffect
  * 同步执行，视图更新前，执行副作用
  * useLayoutEffect callback 中代码执行会阻塞浏览器绘制
* useInetionEffect
  * React v18 新添加
  * useInsertionEffect 执行 -> useLayoutEffect 执行 -> useEffect 执行
  * 本质上 useInsertionEffect 主要是解决 CSS-in-JS 在渲染中注入样式的性能问题


### 状态获取和传递

* useContext
  * 订阅并获取 react context 上下文，用于跨层级状态传递
* useRef
  * 获取元素和组件实例
* useImperativeHandle
  * 用于函数组件能够被 ref 获取
  * 配合 forwardRef 自定义暴露给父组件的实例值

### 状态派生与保存

* useMemo
  * 派生并缓存新的状态，性能优化使用
  * 需要在函数组件中进行大量的逻辑计算，那么我们不期望每一次函数组件渲染都执行这些复杂的计算逻辑，所以就需要在 useMemo 的回调函数中执行这些逻辑，
* useCallback
  * 缓存状态，常用于缓存提供给子代组件的 callback 回调函数
  * useCallback 返回的是函数，这个回调函数是经过处理后的也就是说父组件传递一个函数给子组件的时候，由于是无状态组件每一次都会重新生成新的 props 函数，这样就使得每一次传递给子组件的函数都发生了变化，这时候就会触发子组件的更新，这些更新是没有必要的，此时我们就可以通过 usecallback 来处理此函数，然后作为 props 传递给子组件。

### 工具 hooks

* useID
  * React v18 
  * 服务端渲染
  * 它可以在 client 和 server 生成唯一的 id , 解决了在服务器渲染中，服务端和客户端产生 id 不一致的问题，更重要的是保障了 React v18 中 **streaming renderer （流式渲染）** 中 id 的稳定性。
  
* useDebugValue
  * devtool debug
  * 可用于在 React 开发者工具中显示自定义 hook 的标签。这个hooks目的就是检查自定义hooks。


