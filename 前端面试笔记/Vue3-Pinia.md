## pinia是什么？

在`Vue3`中，可以使用传统的`Vuex`来实现状态管理，也可以使用最新的`pinia`来实现状态管理，我们来看看官网如何解释`pinia`的：`Pinia` 是 `Vue` 的存储库，它允许您跨组件/页面共享状态。 实际上，`pinia`就是`Vuex`的升级版，官网也说过，为了尊重原作者，所以取名`pinia`，而没有取名`Vuex`，所以大家可以直接将`pinia`比作为`Vue3`的`Vuex`

## 为什么要使用pinia？

* `Vue2`和`Vue3`都支持，这让我们同时使用`Vue2`和`Vue3`的小伙伴都能很快上手
* `pinia`中只有`state`、`getter`、`action`，抛弃了`Vuex`中的`Mutation`，`Vuex`中`mutation`一直都不太受小伙伴们的待见，`pinia`直接抛弃它了，这无疑减少了我们工作量
* `pinia` 中 `action` 支持同步和异步，`Vuex` 不支持
* 良好的`Typescript`支持，毕竟我们`Vue3`都推荐使用`TS`来编写，这个时候使用`pinia`就非常合适了
* 无需再创建各个模块嵌套了，`Vuex`中如果数据过多，我们通常分模块来进行管理，稍显麻烦，而`pinia`中每个`store`都是独立的，互相不影响
* 体积非常小，只有`1KB`左右
* `pinia`支持插件来扩展自身功能
* 支持服务端渲染

## pinna使用

* 我们这里搭建一个最新的`Vue3 + TS + Vite`项目

```sh
npm create vite@latest my-vite-app --template vue-ts
```

* `pinia`基础使用

```sh
yarn add pinia
```

```ts
// main.ts
import { createApp } from "vue";
import App from "./App.vue";
import { createPinia } from "pinia";
const pinia = createPinia();

const app = createApp(App);
app.use(pinia);
app.mount("#app");
```

* 创建`store`

```ts
// sbinsrc/store/user.ts
import { defineStore } from 'pinia'

// 第一个参数是应用程序中 store 的唯一 id
export const useUsersStore = defineStore('users', {
  // 其它配置项
})
```

创建`store`很简单，调用`pinia`中的`defineStore`函数即可，该函数接收两个参数：

* name：一个字符串，必传项，该`store`的唯一`id
* `options`：一个对象，`store`的配置项，比如配置`store`内的数据，修改数据的方法等等。

我们可以定义任意数量的`store`，因为我们其实一个`store`就是一个函数，这也是`pinia`的好处之一，让我们的代码扁平化了，这和`Vue3`的实现思想是一样的

* 使用`store`

```vue
<!-- src/App.vue -->
<script setup lang="ts">
import { useUsersStore } from "../src/store/user";
const store = useUsersStore();
console.log(store);
</script>
```

* 添加`state`

```ts
export const useUsersStore = defineStore("users", {
  state: () => {
    return {
      name: "test",
      age: 20,
      sex: "男",
    };
  },
});
```

* 读取`state`数据

```vue
<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <p>姓名：{{ name }}</p>
  <p>年龄：{{ age }}</p>
  <p>性别：{{ sex }}</p>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { useUsersStore } from "../src/store/user";
const store = useUsersStore();
const name = ref<string>(store.name);
const age = ref<number>(store.age);
const sex = ref<string>(store.sex);
</script>
```

上段代码中我们直接通过`store.age`等方式获取到了`store`存储的值，但是大家有没有发现，这样比较繁琐，我们其实可以用解构的方式来获取值，使得代码更简洁一点：

```ts
import { useUsersStore, storeToRefs } from "../src/store/user";
const store = useUsersStore();
const { name, age, sex } = storeToRefs(store); // storeToRefs获取的值是响应式的
```

* 修改`state`数据

```vue
<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <p>姓名：{{ name }}</p>
  <p>年龄：{{ age }}</p>
  <p>性别：{{ sex }}</p>
  <button @click="changeName">更改姓名</button>
</template>

<script setup lang="ts">
import child from './child.vue';
import { useUsersStore, storeToRefs } from "../src/store/user";
const store = useUsersStore();
const { name, age, sex } = storeToRefs(store);
  
const changeName = () => {
  store.name = "张三";
  console.log(store);
};
</script>
```

* 重置`state`

有时候我们修改了`state`数据，想要将它还原，这个时候该怎么做呢？就比如用户填写了一部分表单，突然想重置为最初始的状态。

此时，我们直接调用`store`的`$reset()`方法即可，继续使用我们的例子，添加一个重置按钮

```ts
<button @click="reset">重置store</button>

// 重置store
const reset = () => {
  store.$reset();
};
```

当我们点击重置按钮时，`store`中的数据会变为初始状态，页面也会更新

* 批量更改`state`数据

如果我们一次性需要修改很多条数据的话，有更加简便的方法，使用`store`的`$patch`方法，修改`app.vue`代码，添加一个批量更改数据的方法

```vue
<button @click="patchStore">批量修改数据</button>

<script setup>
// 批量修改数据
const patchStore = () => {
  store.$patch({
    name: "张三",
    age: 100,
    sex: "女",
  });
};
</script>
```

有经验的小伙伴可能发现了，我们采用这种批量更改的方式似乎代价有一点大，假如我们`state`中有些字段无需更改，但是按照上段代码的写法，我们必须要将state中的所有字段例举出了。

为了解决该问题，`pinia`提供的`$patch`方法还可以接收一个回调函数，它的用法有点像我们的数组循环回调函数了。

```ts
store.$patch((state) => {
  state.items.push({ name: 'shoes', quantity: 1 })
  state.hasChanged = true
})
```

* 直接替换整个`state`

`pinia`提供了方法让我们直接替换整个`state`对象，使用`store`的`$state`方法

```ts
store.$state = { counter: 666, name: '张三' }
```

上段代码会将我们提前声明的`state`替换为新的对象，可能这种场景用得比较少

* `getters`属性

`getters`是`defineStore`参数配置项里面的另一个属性。

可以把`getter`想象成`Vue`中的计算属性，它的作用就是返回一个新的结果，既然它和`Vue`中的计算属性类似，那么它肯定也是会被缓存的，就和`computed`一样

* 添加`getter`

```ts
export const useUsersStore = defineStore("users", {
  state: () => {
    return {
      name: "test",
      age: 10,
      sex: "男",
    };
  },
  getters: {
    getAddAge: (state) => {
      return state.age + 100;
    },
  },
})
```

上段代码中我们在配置项参数中添加了`getter`属性，该属性对象中定义了一个`getAddAge`方法，该方法会默认接收一个`state`参数，也就是`state`对象，然后该方法返回的是一个新的数据。

* 使用`getter`

```vue
<template>
  <p>新年龄：{{ store.getAddAge }}</p>
  <button @click="patchStore">批量修改数据</button>
</template>

<script setup lang="ts">
import { useUsersStore } from "../src/store/user";
const store = useUsersStore();
// 批量修改数据
const patchStore = () => {
  store.$patch({
    name: "张三",
    age: 100,
    sex: "女",
  });
};
</script>
```

上段代码中我们直接在标签上使用了store.gettAddAge方法，这样可以保证响应式，其实我们state中的name等属性也可以以此种方式直接在标签上使用，也可以保持响应式。

当我们点击批量修改数据按钮时，页面上的新年龄字段也会跟着变化。

* getter中调用其它getter

前面我们的getAddAge方法只是简单的使用了state方法，但是有时候我们需要在这一个getter方法中调用其它getter方法，这个时候如何调用呢？

其实很简单，我们可以直接在getter方法中调用this，this指向的便是store实例，所以理所当然的能够调用到其它getter。

```ts
export const useUsersStore = defineStore("users", {
  state: () => {
    return {
      name: "小猪课堂",
      age: 25,
      sex: "男",
    };
  },
  getters: {
    getAddAge: (state) => {
      return state.age + 100;
    },
    getNameAndAge(): string {
      return this.name + this.getAddAge; // 调用其它getter
    },
  },
});
```

上段代码中我们又定义了一个名为getNameAndAge的getter函数，在函数内部直接使用了this来获取state数据以及调用其它getter函数。

细心的小伙伴可能会发现我们这里没有使用箭头函数的形式，这是因为我们在函数内部使用了this，箭头函数的this指向问题相信大家都知道吧！所以这里我们没有采用箭头函数的形式。

在组件中调用的形式没什么变化，代码如下：

```vue
<p>调用其它getter：{{ store.getNameAndAge }}</p>
```

* getter 传参

既然getter函数做了一些计算或者处理，那么我们很可能会需要传递参数给getter函数，但是我们前面说getter函数就相当于store的计算属性，和vue的计算属性差不多，那么我们都知道Vue中计算属性是不能直接传递参数的，所以我们这里的getter函数如果要接受参数的话，也是需要做处理的。

```ts
export const useUsersStore = defineStore("users", {
  state: () => {
    return {
      name: "小猪课堂",
      age: 25,
      sex: "男",
    };
  },
  getters: {
    getAddAge: (state) => {
      return (num: number) => state.age + num;
    },
    getNameAndAge(): string {
      return this.name + this.getAddAge; // 调用其它getter
    },
  },
});
```

上段代码中我们getter函数getAddAge接收了一个参数num，这种写法其实有点闭包的概念在里面了，相当于我们整体返回了一个新的函数，并且将state传入了新的函数。

接下来我们在组件中使用，方式很简单，代码如下：

```vue
 <p>新年龄：{{ store.getAddAge(1100) }}</p>
```

* actions 属性

前面我们提到的state和getters属性都主要是数据层面的，并没有具体的业务逻辑代码，它们两个就和我们组件代码中的data数据和computed计算属性一样。

那么，如果我们有业务代码的话，最好就是写在actions属性里面，该属性就和我们组件代码中的methods相似，用来放置一些处理业务逻辑的方法。

actions 属性值同样是一个对象，该对象里面也是存储的各种各样的方法，包括同步方法和异步方法。

* 添加 actions

我们可以尝试着添加一个actions方法，修改user.ts:

```ts
export const useUsersStore = defineStore("users", {
  state: () => {
    return {
      name: "小猪课堂",
      age: 25,
      sex: "男",
    };
  },
  getters: {
    getAddAge: (state) => {
      return (num: number) => state.age + num;
    },
    getNameAndAge(): string {
      return this.name + this.getAddAge; // 调用其它getter
    },
  },
  actions: {
    saveName(name: string) {
      this.name = name;
    },
  },
});
```

上段代码中我们定义了一个非常简单的actions方法，在实际场景中，该方法可以是任何逻辑，比如发送请求、存储token等等。大家把actions方法当作一个普通的方法即可，特殊之处在于该方法内部的this指向的是当前store。

* 使用 actions

使用actions中的方法也非常简单，比如我们在App.vue中想要调用该方法:

```js
const saveName = () => {
  store.saveName("我是小猪");
};
```

我们点击按钮，直接调用store中的actions方法即可

## 总结

pinia 的知识点很少，如果你有Vuex基础，那么学起来更是易如反掌。其实我们更应该关注的是它的函数思想，大家有没有发现我们在 Vue3 中的所有东西似乎都可以用一个函数来表示，pinia 也是延续了这种思想。

所以，大家理解这种组合式编程的思想更重要，pinia无非就是以下3个大点：

* state
* getters
* actions

如果还有兴趣学习pinia的其它特点，比如插件、订阅等，可以去官网深研：pinia官网 （ https://pinia.web3doc.top/ ）。
