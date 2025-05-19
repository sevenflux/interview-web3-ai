# Vue

[TOC]

## 1\. VueJS 的生命周期钩子有哪些？

VueJS 实例在其生命周期中会经历一系列阶段，这些阶段允许我们在特定时机执行自定义逻辑。主要的生命周期钩子包括：

* **创建阶段 (Initialization)**：
  * `beforeCreate`：实例初始化之后，数据观测 (data observation) 和 event/watcher 设置之前被调用。
  * `created`：实例创建完成后被立即调用。在这一步，实例已完成数据观测，属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。
* **挂载阶段 (DOM Insertion)**：
  * `beforeMount`：在挂载开始之前被调用：相关的 `render` 函数首次被调用。
  * `mounted`：el 被新创建的 `vm.$el` 替换，并挂载到实例上去之后调用该钩子。
* **更新阶段 (Diff & Re-render)**：
  * `beforeUpdate`：数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。
  * `updated`：由于数据更改导致的虚拟 DOM 重新渲染和打补丁之后调用。
* **销毁阶段 (Teardown)**：
  * `beforeDestroy`：实例销毁之前调用。在这一步，实例仍然完全可用。
  * `destroyed`：实例销毁后调用。该钩子被调用后，对应 Vue 实例的所有指令都被解绑，所有事件监听器被移除，所有子实例也都被销毁。

## 2\. `v-show` 和 `v-if` 指令有什么区别？

`v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。`v-if` 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
相比之下，`v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 的 `display` 属性进行切换。
总的来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。 `v-show` 不支持 `<template>` 元素，也不支持 `v-else`。

## 3\. 为什么在 `v-for` 指令中需要使用 `key` 属性？

为了跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为 `v-for` 迭代中的每一项提供一个唯一的 `key` 属性。 `key` 的理想值是每一项的唯一 ID。 建议尽可能在使用 `v-for` 时提供 `key`，除非迭代的 DOM 内容很简单，或者你刻意依赖默认行为以获取性能上的提升。 不应使用对象或数组等非原始值作为 `v-for` 的 `key`，应使用字符串或数字类型的值。

## 4\. VueJS 中数组变化的侦测有哪些注意事项？

由于 JavaScript 的限制，Vue 不能检测以下数组的变动：

1. 当你利用索引直接设置一个数组项时，例如：`vm.todos[indexOfTodo] = newTodo`。
2. 当你修改数组的长度时，例如：`vm.todos.length = todosLength`。

要解决第一种情况，可以使用 `Vue.set(vm.todos, indexOfTodo, newTodoValue)` 或 `vm.todos.splice(indexOfTodo, 1, newTodoValue)`。
要解决第二种情况，可以使用 `vm.todos.splice(todosLength)`。

## 5\. VueJS 中对象变化的侦测有哪些注意事项？

Vue 不能检测对象属性的添加或删除。 例如，`vm.user.email = 'john@email.com'` 不会使 `vm.user.email` 变为响应式的。 可以使用 `Vue.set(object, key, value)` 方法或 `Object.assign()` 来解决这个问题。

## 6\. VueJS 中如何实现双向数据绑定？

你可以使用 `v-model` 指令在表单输入、文本区域和选择元素上创建双向数据绑定。 它会根据控件类型自动选取正确的方法来更新元素。 `v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值，而总是将 Vue 实例的数据作为数据来源。

## 7\. 什么是 props (属性)？

Props 是你可以在组件上注册的自定义 attribute。当一个值传递给一个 prop attribute 的时候，它就变成了那个组件实例的一个 property。 组件的 `props` 选项用于声明组件期望从父组件接收的数据。

## 8\. 如何在自定义输入组件上实现 `v-model`？

自定义事件也可以用于创建支持 `v-model` 的自定义输入组件。要让组件的 `v-model` 生效，它必须：

1. 将其 `value` attribute 绑定到一个 `value` prop。
2. 在其 `input` 事件触发时，将新的值通过自定义的 `input` 事件抛出。

例如：

```html
<input
  v-bind:value="value"
  v-on:input="$emit('input', $event.target.value)"
>
```

然后可以在父组件中使用 `v-model`：`<custom-input v-model="searchInput"></custom-input>`。

## 9\. 什么是插槽 (Slots)？

Vue 实现了一个内容分发 API，参照了当前 Web Components 规范草案，使用特殊的 `<slot>` 元素作为原始内容的插槽。 这允许你像这样撰写组件：`<alert><slot></slot></alert>`，父组件可以向这个插槽中插入动态内容：`<alert>出错了。</alert>`。

## 10\. Props 遵循怎样的数据流？

所有的 prop 都使得其父子 prop 之间形成了一个单向下行绑定：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。 这会防止从子组件意外变更父级组件的状态，从而导致你的应用的数据流向难以理解。 子组件不应该修改 prop，否则 Vue 会在控制台发出警告。

## 11\. 什么是混入 (Mixins)？

混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。 当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项。

## 12\. 什么是自定义指令 (Custom Directives)？

自定义指令是你可以附加到 DOM 元素上的微小命令。它们以 `v-` 为前缀，以告知库你正在使用特殊的标记，并保持语法一致。 当你需要对普通 DOM 元素进行底层操作时，自定义指令非常有用。

## 13\. 相比模板 (Templates)，使用渲染函数 (Render Functions) 有什么好处？

Vue 通过模板建立你的应用。但在某些特殊情况下，你真的需要 JavaScript 的完全编程能力，这时渲染函数就派上了用场。 模板实际上在构建时会被编译成渲染函数。 渲染函数提供了对组件树更细致的控制。

## 14\. `<keep-alive>` 标签的用途是什么？

`<keep-alive>` 是一个抽象组件，用于保存组件状态或避免重新渲染。 当你用 `<keep-alive>` 包裹一个动态组件时，它会缓存非活动状态的组件实例，而不是销毁它们。 当存在多个条件子元素时，`<keep-alive>` 要求同时只有一个子元素被渲染。

## 15\. 什么是 Vuex？

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式 + 库。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

## 16\. Vuex 中的 Mutations 和 Actions 有什么区别？

Mutations 和 Actions 的主要区别在于：

1. **Mutations (变更)**：更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的事件类型 (type) 和一个回调函数 (handler)。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数。Mutation 必须是同步函数。
2. **Actions (动作)**：Action 类似于 mutation，不同在于：Action 提交的是 mutation，而不是直接变更状态。Action 可以包含任意异步操作。

## 17\. 方法 (Methods) 和计算属性 (Computed Properties) 的主要区别是什么？

计算属性是基于它们的响应式依赖进行缓存的。只在相关响应式依赖发生改变时它们才会重新求值。 这也意味着只要依赖项还没有发生改变，多次访问计算属性会立即返回之前的计算结果，而不必再次执行函数。 相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。

## 18\. 为什么组件的 `data` 必须是一个函数？

组件的 `data` 必须是一个函数，而不是直接提供一个对象。这是因为每个实例需要维护一份被返回对象的独立拷贝。 如果 `data` 是一个纯对象，所有实例将共享同一个数据对象引用，一个实例的状态改变会影响所有其他实例。

## 19\. 什么是组合式 API (Composition API)？

组合式 API (Composition API) 是一系列附加的、基于函数的 API，允许灵活地组合组件逻辑。 它旨在解决大规模应用中 Options API 的局限性，提供更好的逻辑复用和代码组织。

## 20\. 强制重新渲染一个组件的最佳方式是什么？

强制 Vue 重新渲染一个组件的最佳方式是在该组件上设置一个 `:key`。当需要重新渲染组件时，只需更改 `key` 的值，Vue 就会重新渲染该组件。
