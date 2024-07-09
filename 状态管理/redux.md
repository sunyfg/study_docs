## Redux 是什么？

redux 是一个用于管理和更新APP内状态的模式和库。

## 为什么用Redux

Redux 帮我们管理全局状态。它提供了一些列工具和模式可以让我们容易理解你APP中的这些状态在什么时间、在哪里、为什么和怎么样发生了变化。并且知道当这些发生变化的时候，你的程序逻辑将会如何表现。

## Redux 库和工具

Redux 是一个独立的库，但是通常它会配合其他包一起使用。

### React-Redux

redux 可以和任何 UI 框架配合使用，在React中是使用频率最高的。 React-Redux 也是我们官方的包，它可以让你的React 组件通过读取一部分 state 和 分发 actions 来更新 store , 进而与 Redux store 发生交互。

### Redux Toolkit

这个工具是我们推荐的书写 redux 逻辑的方法，它包含了我们认为构建Redux应用程序所必需的包和函数。Redux工具箱构建在我们建议的最佳实践中，简化了大多数Redux任务，防止了常见错误，并使编写Redux应用程序变得更容易。

### Redux DevTools Extension

Redux DevTools扩展显示了Redux存储中状态随时间变化的历史记录。这允许您有效地调试应用程序，包括使用强大的技术，如“时间旅行调试”。



## Redux 概念

### Immutability 不可变性

在 js 中可以不修改 object 和 array 的引用，即可修改其内容。比如：

```js
const obj = {a: 1};
obj.b = 2;

const arr = [];
arr.push('a');
arr[1] = 'b';
```

为了保证不可变的更新值，需要 copy 对象或数组，然后修改副本。比如

```js
const obj = {
    a:{
        c:3
    },
    b: 2
}

const copyObj = {
    ...obj, // copy obj
    a:{ // overwrite a
        ...obj.a, // copy obj.a
        c: 4 // overwrite c
	}
}
```

Redux 期望所有状态的更新都是基于 `immutably` 完成的。



## Redux 专有术语

### Actions

一个 `action` 是一个包含 `type` 字段的普通JavaScript对象。你可以认为一个 `action` 是一个描述了发生在程序中一些事情的事件。

```js
const addTodoAction = {
    type: 'todos/todoAdded',
    payload: 'buy milk'
}
```

### Action Creator

一个 `action creator`  是一个函数，它返回一个 `action` 对象。通常用这种方法避免每次都书写 `action` 对象。

```js
const addTodo = text =>{
	return {
		type: 'todos/todoAdded',
        payload: text
    }
}
```

### Reducers

一个 `reducer` 是一个函数，它接收当前 `state` 和一个 `action` 对象，如果有必要，那么它会决定如何更新这个状态，并且返回一个新的状态：`(state, action) => newState`. 你可以把 `reducer` 看做成一个事件监听器，它基于接受到的 `action type` 来处理事件。

#### 如何书写reducer

大概要遵守以下几个步骤：

* 检查是否要处理当前 `action`
  * 是，拷贝`state` ，并用新的值更新副本，最后返回更新后的副本
* 否则返回已存在的未变更的`state`

```js
const initialState = { value: 0 }

function counterReducer(state = initialState, action) {
  // Check to see if the reducer cares about this action
  if (action.type === 'counter/increment') {
    // If so, make a copy of `state`
    return {
      ...state,
      // and update the copy with the new value
      value: state.value + 1
    }
  }
  // otherwise return the existing state unchanged
  return state
}
```

当然，你也可以使用 `if/else`,`switch` 等逻辑判断。

#### 为何取名 Reducers

我们知道`Array.reduce()` 方法允许你一次处理数组中的每一项，并返回单个最终结果。你可以把它想象成“将数组减少到一个值”

> **A Redux reducer function is exactly the same idea as this "reduce callback" function!** 

### Store

当前Redux应用程序状态驻留在一个称为store的对象中。

可以通过传入一个 `reducer` 来创建 `store` ， 它有一个方法 `getState` ，用于返回当前 `state` 的值。

```js
import { configureStore } from '@reduxjs/toolkit'

const store = configureStore({ reducer: counterReducer })

console.log(store.getState())
// {value: 0}
```

### Dispatch

Redux store 有一个名叫`dispatch` 的方法。它是唯一可以更新 `state` 的方法，你可以通过调用 `store.dispatch()` 并且传入一个 `action` 对象。在内部，`store` 会执行它的 `reducer` 函数，并且保存新的`state vlaue` , 我们可以通过调用 `getState()`  获取到更新后的值。

```js
store.dispatch({
    type:'counter/increment'
})
console.log(store.getState())
// {value: 1}
```

你可以认为 `dispatching actinos` 是 “触发一个事件”。整个流程就是： 在某些情况下，我们需要拿到 `store` 来并分析它。 `Reducers` 行为就像事件监听器，当它们监听到感兴趣的操作时，就会更新状态作为响应。

### Selectors

`Selectors` 是一些列函数，它知道如何从一个 `store state` 中提取指定的一部分







