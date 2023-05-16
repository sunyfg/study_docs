# React 高级特性

* 函数组件
* 非受控组件
* Portals
* context
* 异步组件
* 性能优化
* 高阶组件HOC
* Render Props

## 函数组件

* 纯函数，输入 props，输出 JSX
* 没有实例，没有生命周期
* 不能扩展其它方法

## 非受控组件

* ref
* defaultValue defaultChecked
* 手动操作 DOM 元素

### 场景

* 必须手动操作 DOM 元素，setState 实现不了
* 文件上传 `<input type="file" />` 
* 某些富文本编辑器，需要传入 DOM 元素

### 受控组件 vs 非受控组件

* 优先使用受控组件，符合 React 设计原则
* 必须操作 DOM 时，再使用非受控组件

## Portals

* 组件默认会按照既定层级嵌套渲染
* 如何让组件渲染到父组件以外？

```js
ReactDom.createPortal(Dom, target);
```

### Portals 使用场景

* overflow: hidden
* 父组件 z-index 值太小
* fixed 需要放在 body 第一层

## context

* 公共信息（语言，主题）如何传递给每个组件？
* 用 props 太繁琐
* 用 redux 小题大做
* 父组件：ThemeContext.Provider value
  * 类组件：静态属性 contextType = ThemeContext
  * 函数组件：ThemeContext.Consumer

## 异步组件

* import()
* React.lazy
* React.Suspense

```react
const Demo = React.lazy(() => import('./Demo'));

// 使用
<React.Suspense fallback={<div>loading...</div>} />
	<Demo />
</React.Suspense>
```

## 性能优化

* shouldComponentUpdate（SCU）
* PureComponent 和 React.memo
* 不可变值 immutable.js

### 不可变值

### SCU

默认返回 `true` 

React 默认：父组件有更新，子组件则无条件更新

必须配合 “不可变值” 使用

SCU 每次都要用吗？—— 需要时才优化

建议浅层比较，不建议深度比较

lodash：`_.isEqual(obj1, obj2)` 深度比较

### PureComponent 和 memo

* PureComponent，SCU 中实现了浅比较
* memo，函数组件中的 PureComponent
* 浅比较已适用大部分情况

### React.memo 使用

```react
function MyComponent(props) {
  /* 使用 props 渲染 */
}
function areEqual(prevProps, nextProps) {
  /*
  	如果把 nextProps 传入 render 方法的返回结果与prevProps 传入 render 方法的返回结果一致，则返回 true,
  	否则返回 false
  */
}
export default React.memo(MyComponent, areEqual)
```

### immutable.js

* 彻底拥抱「不可变值」
* 基于共享数据（不是深拷贝），速度好
* 有一定的学习成本和迁移成本，按需使用

## 组件公共逻辑的抽离

* mixin，已被 React 弃用
* 高阶组件（HOC）
* Render Props
* hooks

### 高阶组件（HOC）

```react
// 高阶组件不是一种功能，而是一种模式
const HOCFactory = (Component) => {
  class HOC extends React.Component {
    // 在此定义多个组件的公共逻辑
    render() {
      return <Component {...this.props} />
    }
  }
}
const Component1 = HOCFactory(WarppedComponent);
```

### Render Props

```react
// Render Props 的核心思想
// 通过一个函数将 class 组件的 state 作为 props 传递给纯函数组件
class Factory extends React.Component {
  constructor() {
    this.state = {
      a: 0,
      b: 0
    }
  }
  render() {
    return <div>{this.props.render(this.state)}</div>
  }
}

const App = () => {
  <Factory render={
      /* render 是一个纯函数 */
      (props) => <p>{props.a} {props.b}</p>
    }
  />
}
```

### HOC vs Render Props

* HOC：模式简单，但增加组件层级
* Render Props：代码简洁
