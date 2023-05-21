## 跳过不必要的组件更新

这类优化是在组件状态发生变更后，通过减少不必要的组件更新来实现。

### PureComponent、React.memo

在 React 工作流中，如果只有父组件发生状态更新，即使父组件传给子组件的所有 Props 都没有修改，也会引起子组件的 Render 过程。从 React 的声明式设计理念来看，如果子组件的 Props 和 State 都没有改变，那么其生成的 DOM 结构和副作用也不应该发生改变。当子组件符合声明式设计理念时，就可以忽略子组件本次的 Render 过程。PureComponent 和 React.memo 就是应对这种场景的，PureComponent 是对类组件的 Props 和 State 进行浅比较，React.memo 是对函数组件的 Props 进行浅比较。

### shouldComponentUpdate

使用 shouldComponentUpdate 来深比较 props，只在 props 有修改才执行组件的 Render 过程。

问题：props 中某个「子组件未使用的属性」发生了更新，子组件也会触发 Render 过程。

通过实现子组件的 shouldComponentUpdate 方法，仅在「子组件使用的属性」发生改变时才返回 `true`，便能避免子组件重新 Render。

有两个弊端：

* 如果存在很多子孙组件，「找出所有子孙组件使用的属性」就会有很多工作量，也容易因为漏测导致 bug。
* 存在潜在的工程隐患

### useMemo、useCallback 实现稳定的 Props 值

如果传给子组件的派生状态或函数，每次都是新的引用，那么 PureComponent 和 React.memo 优化就会失效。所以需要使用 useMemo 和 useCallback 来生成稳定值，并结合 PureComponent 或 React.memo 避免子组件重新 Render。

### 发布者订阅者跳过中间组件 Render 过程

React 推荐将公共数据放在所有「需要该状态的组件」的公共祖先上，但将状态放在公共祖先上后，该状态就需要层层向下传递，直到传递给使用该状态的组件为止。

每次状态的更新都会涉及中间组件的 Render 过程，但中间组件并不关心该状态，它的 Render 过程只负责将该状态再传给子组件。

只要是发布者订阅者模式的库，都可以进行该优化。比如：redux、use-global-state、React.createContext 等。

### 列表项使用 key 属性

React diff 算法 会根据 tag 和 key 判断组件是否要删除重建。

### useMemo 返回虚拟 DOM

利用 useMemo 可以缓存计算结果的特点，如果 useMemo 返回的是组件的虚拟 DOM，则将在 useMemo 依赖不变时，跳过组件的 Render 阶段。该方式与 React.memo 类似，但与 React.memo 相比有以下优势：

* 更方便。React.memo 需要对组件进行一次包装，生成新的组件。而 useMemo 只需在存在性能瓶颈的地方使用，不用修改组件。

* 更灵活。useMemo 不用考虑组件的所有 Props，而只需考虑当前场景中用到的值，也可使用 [useDeepCompareMemo](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fsandiiarov%2Fuse-deep-compare%23usedeepcomparememo) 对用到的值进行深比较。

## 通用优化

### 组件按需挂载

#### 懒加载

懒加载的实现是通过 Webpack 的动态导入和 `React.lazy` 方法。

#### 懒渲染

懒渲染指当组件进入或即将进入可视区域时才渲染组件。常见的组件 Modal/Drawer 等，当 visible 属性为 true 时才渲染组件内容，也可以认为是懒渲染的一种实现。

懒渲染的使用场景有：

1. 页面中出现多次的组件，且组件渲染费时、或者组件中含有接口请求。如果渲染多个带有请求的组件，由于浏览器限制了同域名下并发请求的数量，就可能会阻塞可见区域内的其他组件中的请求，导致可见区域的内容被延迟展示。
2. 需用户操作后才展示的组件。这点和懒加载一样，但懒渲染不用动态加载模块，不用考虑加载态和加载失败的兜底处理，实现上更简单。

懒渲染的实现中判断组件是否出现在可视区域内是通过 [react-visibility-observer](https://link.juejin.cn/?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Freact-visibility-observer) 进行监听。

```js
import { useState, useEffect } from "react"
import VisibilityObserver, {
  useVisibilityObserver,
} from "react-visibility-observer"

const VisibilityObserverChildren = ({ callback, children }) => {
  const { isVisible } = useVisibilityObserver()
  useEffect(() => {
    callback(isVisible)
  }, [callback, isVisible])

  return <>{children}</>
}

export const LazyRender = () => {
  const [isRendered, setIsRendered] = useState(false)

  if (!isRendered) {
    return (
      <VisibilityObserver rootMargin={"0px 0px 0px 0px"}>
        <VisibilityObserverChildren
          callback={isVisible => {
            if (isVisible) {
              setIsRendered(true)
            }
          }}
        >
          <span />
        </VisibilityObserverChildren>
      </VisibilityObserver>
    )
  }

  console.log("滚动到可视区域才渲染")
  return <div>我是 LazyRender 组件</div>
}

export default LazyRender
```

#### 虚拟列表

虚拟列表是懒渲染的一种特殊场景。虚拟列表的组件有  [react-window](https://link.juejin.cn?target=https%3A%2F%2Freact-window.now.sh%2F%23%2Fexamples%2Flist%2Ffixed-size) 和 react-virtualized，它们都是同一个作者开发的。react-window 是 react-virtualized 的轻量版本，其 API 和文档更加友好。所以新项目中推荐使用 react-window，而不是使用 Star 更多的 react-virtualized。

### 批量更新

React 17 及 之前版本：

* 在 React 管理的事件回调和生命周期中，setState 是异步的，而其他时候 setState 都是同步的。这个问题根本原因就是 React 在自己管理的事件回调和生命周期中，对于 setState 是批量更新的，而在其他时候是立即更新的。
* 批量更新 setState 时，多次执行 setState 只会触发一次 Render 过程。相反在立即更新 setState 时，每次 setState 都会触发一次 Render 过程，就存在性能影响。

React 18：

* React 管理的事件回调和生命周期：异步更新，合并 state
* DOM 事件，setTimeout：异步更新，合并 state
* 优化 Automatic Batching 自动批处理

### 缓存优化

缓存优化往往是最简单有效的优化方式，在 React 组件中常用 useMemo 缓存上次计算的结果。当 useMemo 的依赖未发生改变时，就不会触发重新计算。一般用在「计算派生状态的代码」非常耗时的场景中，如：遍历大列表做统计信息。

### debounce、throttle 优化频繁触发的回调



