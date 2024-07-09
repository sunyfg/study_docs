# 一面

## 面试题

* react

  * state 存在了哪里？考 fiber
  * 怎么调用子组件的方法? forwardRef & useImperactiveHandler
  * 讲下 useCallback

* webpack

  * 有没有写过 webpack plugin, 实现自动化把 js 静态资源发布到 cdn.

* 服务端渲染，SSR 有没有了解。现代化服务端渲染，基于node做。原理是什么？

* 微前端

* monorepo

* Node都使用哪些？SSR ， 集群怎么搞？

* lighthouse 都看哪些指标？做过哪方面的优化？

* 如何自己实现对接口请求时长的统计和上报？可以借用 AXIOS

* 怎么理解前端工程化？ 组件化、模块化、自动化、规范化。

* npm 发包。

  * 怎么发包？
  * package.json 中都有哪些常用的字段？
  * 怎么控制版本？

* TypeScript

  * interface 和 type的使用场景，怎么实现继承？
  * 怎么获取函数返回值的类型？
  * 怎么声明函数式组件接受的 props 类型？ 有几种方式？分别是什么？
  * 如何获取 class 的参数类型？

* 组件库

  * sensd 组件库是怎么打包的？webpack?rollup?
  * 打包出的格式是什么样的？
  * 按需加载原理，怎么做？

* 如果到下家公司，你会用的技术做哪些贡献？你期望什么样的公司？

* 如果ok，什么时间能入职下家公司？

  

## 答案

#### react 的 state 存在了哪里，为何函数组件再次执行后，还可以拿到 state 的值？（考察对React fiber 架构的理解）

> 参考：https://vue3js.cn/interview/React/Fiber.html#%E4%BA%8C%E3%80%81%E6%98%AF%E4%BB%80%E4%B9%88

Fiber 是 Fiber 树结构的节点单位，也就是 React16 新架构下的虚拟 Dom.

一个 Fiber 就是一个 JavaScript 对象。

*state 是存在 Fiber 对象的 memoizedState 中。*

**下面是 Fiber 的结构描述。**

```ts
type Fiber = {
  // 用于标记fiber的WorkTag类型，主要表示当前fiber代表的组件类型如FunctionComponent、ClassComponent等
  tag: WorkTag,
  // ReactElement里面的key
  key: null | string,
  // ReactElement.type，调用`createElement`的第一个参数
  elementType: any,
  // The resolved function/class/ associated with this fiber.
  // 表示当前代表的节点类型
  type: any,
  // 表示当前FiberNode对应的element组件实例
  stateNode: any,

  // 指向他在Fiber节点树中的`parent`，用来在处理完这个节点之后向上返回
  return: Fiber | null,
  // 指向自己的第一个子节点
  child: Fiber | null,
  // 指向自己的兄弟结构，兄弟节点的return指向同一个父节点
  sibling: Fiber | null,
  index: number,

  ref: null | (((handle: mixed) => void) & { _stringRef: ?string }) | RefObject,

  // 当前处理过程中的组件props对象
  pendingProps: any,
  // 上一次渲染完成之后的props
  memoizedProps: any,

  // 该Fiber对应的组件产生的Update会存放在这个队列里面
  updateQueue: UpdateQueue<any> | null,

  // 上一次渲染的时候的state
  memoizedState: any,

  // 一个列表，存放这个Fiber依赖的context
  firstContextDependency: ContextDependency<mixed> | null,

  mode: TypeOfMode,

  // Effect
  // 用来记录Side Effect
  effectTag: SideEffectTag,

  // 单链表用来快速查找下一个side effect
  nextEffect: Fiber | null,

  // 子树中第一个side effect
  firstEffect: Fiber | null,
  // 子树中最后一个side effect
  lastEffect: Fiber | null,

  // 代表任务在未来的哪个时间点应该被完成，之后版本改名为 lanes
  expirationTime: ExpirationTime,

  // 快速确定子树中是否有不在等待的变化
  childExpirationTime: ExpirationTime,

  // fiber的版本池，即记录fiber更新过程，便于恢复
  alternate: Fiber | null,
}
```

#### 怎么调用子组件的方法？

一般使用 React.forwardRef 配合 useImperactiveHandle 一起使用。

```tsx
// child.tsx
const Child = React.forwardRef((props, ref) => {
  const someMethods = () => {
    console.log('call the someMethods function.')
  }
  
  useImperactiveHandle(ref, () => {
		return {
      someMethods,
		}
  })
  
  return <div>child node</div>
})

// father.tsx
const Father = () => {
  const childRef = useRef(null);	
  
  const handleClick = () => {
		if (childRef.current) {
      	
		}
  }
  
  return (
  	<div>
      <Button onClick={handleClick}>button</Button>
      <Child ref={childRef}></Child>
    </div>
  )
}


```





#### Typescript 如何获取class构造函数的参数类型？

使用 `ConstructorParameters`  实用方法。

```typesc
class MyClass {
	name = ''
	age;
	constructor(name: string, age: number) {
		this.name = name;
		this.age = age;
	}
}


type MyClassParamsType = ConstructorParameters<typeof MyClass>;

// 返回一个元组类型，包含了每个参数以及类型。
// MyClassParamsType = [name: string, age: number]

```

引用：

> 1. 官方文档：https://www.typescriptlang.org/docs/handbook/utility-types.html#constructorparameterstype
>
> 2. 他人博客：https://www.jiyik.com/tm/xwzj/prolan_2137.html
>
> 3. [自行到 playground 亲眼看下](https://www.typescriptlang.org/play?#code/PTAEHUFMBsGMHsC2lQBd5oBYoCoE8AHSAZVgCcBLA1UABWgEM8BzM+AVwDsATAGiwoBnUENANQAd0gAjQRVSQAUCEmYKsTKGYUAbpGF4OY0BoadYKdJMoL+gzAzIoz3UNEiPOofEVKVqAHSKymAAmkYI7NCuqGqcANag8ABmIjQUXrFOKBJMggBcISGgoAC0oACCbvCwDKgU8JkY7p7ehCTkVDQS2E6gnPCxGcwmZqDSTgzxxWWVoASMFmgYkAAeRJTInN3ymj4d-jSCeNsMq-wuoPaOltigAKoASgAywhK7SbGQZIIz5VWCFzSeCrZagNYbChbHaxUDcCjJZLfSDbExIAgUdxkUBIursJzCFJtXydajBBCcQQ0MwAUVWDEQC0gADVHBQGNJ3KAALygABEAAkYNAMOB4GRonzFBTBPB3AERcwABS0+mM9ysygc9wASmCKhwzQ8ZC8iHFzmB7BoXzcZmY7AYzEg-Fg0HUiQ58D0Ii8fLpDKZgj5SWxfPADlQAHJhAA5SASPlBFQAeS+ZHegmdWkgR1QjgUrmkeFATjNOmGWH0KAQiGhwkuNok4uiIgMHGxCyYrA4PCC0sYgmEAFk8ABhAfCADeihKJU4DJQvMjkZns8dSlnaMpqDI7Fg6DISvnyHyVx3wwuTtPnHYiGk3x1oGnm83Q0EAWPi-6C4A3KuXwI77rjyYhOn+m4AL6KFBiioO0oAjuODCDvsIGjo0VK7vu4q0I4C4KD8AA8cFEESiETgAfMEQA)