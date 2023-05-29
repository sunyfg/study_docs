# 浏览器 tab 之间如何通信？

### 场景1：两个需要交互的tab页面具有依赖关系 – `postMessage`

如 A页面中通过JavaScript的 `window.open` 打开B页面，或者B页面通过 `iframe` 嵌入至A页面，此种情形最简单，可以通过HTML5的 `window.postMessage` API完成通信，由于 `postMessage` 函数是绑定在 `window` 全局对象下，因此通信的页面中必须有一个页面（如A页面）可以获取另一个页面（如B页面）的 `window` 对象，这样才可以完成单向通信；B页面无需获取A页面的 `window` 对象，如果需要B页面对A页面的通信，只需要在B页面侦听`message` 事件，获取事件中传递的 `source` 对象，该对象即为A页面 `window` 对象的引用。

### 场景2：两个打开的页面属于同源范畴 – storage

若要实现两个互不相关的同源 `tab` 页面通信，可以使用一种比较巧妙的方式：`localstorage`。`localStorage` 的存储遵循**同源策略**，因此同源的两个 `tab` 页面可以通过这种共享 `localStorage` 的方式实现通信，通过约定`localStorage` 的某一个 `itemName`，基于该 `key` 值的内容作为“共享硬盘”方式通信。

HTML5提供了 `storage` 事件，通过 `window` 对象侦听 `storage` 事件，会侦听 `localStorage` 对象的变化事件（包括item的添加、修改和删除）。因此，通过事件可以完成高效的通信机制。

```js
window.addEventListener('storage', (event) => {
  console.log(event);
  const {
    domain,   // 存储变化对应的域
    key,      // 被设置或删除的键
    oldValue, // 键变化之前的值
    newValue, // 键被设置的新值，若键被删除则为 null
  } = event;
})
```

