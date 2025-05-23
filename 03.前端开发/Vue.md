# Vue

[TOC]

## 1. Vue.js 是什么？

Vue.js 是一个开源的 JavaScript 框架，用于构建用户界面和动态单页 Web 应用程序（Single-Page Applications, SPA）。

## 2. Vue.js 是否只在少数浏览器中支持？

错误。Vue.js 能够兼容主流浏览器，具有广泛的浏览器支持范围。

## 3. 列举 Vue.js 的核心特性？

Vue.js 的特性包括模板系统、事件处理、路由功能、数据绑定机制、轻量化设计以及与现有项目快速集成等。

> [术语说明] 模板指用声明式语法将 DOM 绑定到 Vue 实例数据；数据绑定中的双向绑定通过 v-model 实现，而轻量化指的是框架文件体积小且运行效率高。

## 4. 什么是 Vue.js 中的过滤器 (filters)？

过滤器本质上是 JavaScript 函数，用于格式化模板中显示的文本内容。它们通过管道符(|)调用，例如：`{{ message | capitalize }}`。

```vue
// 示例过滤器定义
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  return value.toString().toUpperCase()
})
```

## 5. v-for 指令的主要作用是什么？

用于循环渲染数组或对象中的元素。其语法为 `v-for="item in items"`，可以配合 `:key` 属性提升渲染性能。当遍历对象时，还可获取键名和索引：`v-for="(value, name, index) in object"`。

## 6. 在 Vue.js 中如何创建 Vue 实例？

通过实例化 Vue 构造函数完成，通常需要传入包含数据、方法等选项的配置对象：

```javascript
const vm = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

## 7. 能否在 Vue.js 中创建自定义过滤器？

是的，可以通过全局方法 `Vue.filter()` 注册自定义过滤器，或在组件选项中通过 `filters` 属性定义局部过滤器。

## 8. Vue.js 提供了哪些常用的内置指令？

包括但不限于：

- 循环渲染：`v-for`
- 条件渲染：`v-if`/`v-else`（需注意元素必须连续）
- 显示切换：`v-show`（通过 CSS 控制）
- 数据绑定：`v-bind`（简写为 `:`）
- 双向绑定：`v-model`
- 事件监听：`v-on`（简写为 `@`）

## 9. const 关键字在 Vue.js 中的用途？

用于声明不可变的常量。例如声明配置对象：

```javascript
const appConfig = {
  apiUrl: 'https://api.example.com'
}
```

## 10. 什么是虚拟 DOM (Virtual DOM)？

虚拟 DOM 是对真实 DOM 的轻量级 JavaScript 对象抽象。Vue 通过比对虚拟 DOM 的变化来最小化真实 DOM 操作，从而提升渲染效率。

> [原理补充] 当数据变更时，Vue 会生成新的虚拟 DOM 树，通过 Diff 算法比较新旧树的差异，最终仅更新必要的 DOM 节点。

## 11. Vue.js 是否支持双向数据绑定？

支持。通过 `v-model` 指令实现表单元素与数据的双向绑定。该指令本质上是语法糖，自动处理用户的输入事件和数值更新。

## 12. 哪种指令用于单向数据绑定？

`v-bind` 指令用于单向数据绑定，将数据从组件实例传递到 DOM 属性。示例：`<div v-bind:class="dynamicClass"></div>` 或简写为 `<div :class="dynamicClass">`。

## 13. Vue.js 中实现双向绑定的指令是？

`v-model` 指令专用于表单元素（如 input、select 等），实现数据的双向绑定。其底层对应不同表单元素的 value 属性和 input 事件。

## 14. 什么是混入 (Mixin)？

混入是代码复用机制，允许将组件选项（如方法、生命周期钩子）合并到多个组件中。例如：

```javascript
const myMixin = {
  created() {
    this.logMessage()
  },
  methods: {
    logMessage() {
      console.log('Mixin activated')
    }
  }
}

Vue.component('example-comp', {
  mixins: [myMixin],
  // ...
})
```

## 15. Vue 3 中是否仍可使用过滤器？

不可用。Vue 3 已移除过滤器语法，推荐使用计算属性 (computed properties) 或方法 (methods) 替代原有过滤器功能。

## 16. 过滤器在 Vue 中是否可复用？

是的。通过全局注册的过滤器可在任意组件中使用，局部过滤器则在其注册组件内有效。

## 17. Vue.js 中的条件渲染指令有哪些？

主要包括 `v-if`、`v-else-if`、`v-else`。这些指令按条件决定是否渲染元素，当条件频繁变化时建议改用 `v-show`。

## 18. 是否能在 Vue.js 中调用 REST API？

可以，常用 Axios 库进行 HTTP 请求。示例集成方式：

```javascript
import axios from 'axios'

export default {
  data() {
    return {
      posts: []
    }
  },
  async created() {
    const response = await axios.get('/api/posts')
    this.posts = response.data
  }
}
```

## 19. Vue.js 如何检测数组变更？

Vue 对部分数组方法进行封装以触发视图更新，包括 `push()`、`pop()`、`shift()`、`unshift()`、`splice()`、`sort()`、`reverse()`。直接通过索引修改元素或修改数组长度时需使用 `Vue.set()` 或用新数组替换原数组。

## 20. 列举 Vue 的按键修饰符

`.enter`、`.tab`、`.delete`、`.esc`、`.space` 等用于监听特定键盘事件。示例：`@keyup.enter="submitForm"` 在回车键松开时触发方法。

## 21. Vue 的事件修饰符有哪些？

包括阻止事件冒泡的 `.stop`、使用捕获模式的 `.capture`、仅当事件在元素本身触发时生效的 `.self`、只触发一次的 `.once`，以及提升滚动性能的 `.passive`。

## 22. v-show 和 v-if 的区别？

`v-if` 是真正的条件渲染，元素会在条件切换时被销毁/重建；`v-show` 通过 CSS 的 `display` 属性控制显示，初始渲染成本较高但切换开销小。频繁切换时建议使用 `v-show`。

## 23. Vue.js 的鼠标事件修饰符有哪些？

`.left`（左键）、`.right`（右键）、`.middle`（中键），用于处理特定鼠标按钮触发的事件。例如：`@click.right="showContextMenu"`。

## 24. 什么是 $parent 属性？

允许子组件通过 `this.$parent` 访问其父组件实例。该属性在组件层级较深时应谨慎使用，推荐优先使用 props/events 进行通信。

## 25. 什么是全局组件？

通过 `Vue.component()` 注册的组件可在任何新创建的 Vue 根实例及其子组件模板中使用，无需重复注册。适合通用组件如按钮、弹窗等。

## 26. 什么是局部组件？

通过组件选项的 `components` 属性注册，仅在当前组件的作用域内可用。示例：

```javascript
const ChildComp = {
  template: '<div>Child Component</div>'
}

new Vue({
  components: {
    'child-comp': ChildComp
  }
})
```

## 27. Vue 单文件组件包含哪几部分？

由 `<template>`（模板）、`<script>`（逻辑）、`<style>`（样式）三个部分组成，后缀为 `.vue` 的文件。例如：

```vue
<template>
  <div>{{ message }}</div>
</template>

<script>
export default {
  data() {
    return { message: 'Hello' }
  }
}
</script>

<style scoped>
div { color: blue; }
</style>
```

## 28. $child 属性的用途

通过 `this.$children` 访问子组件实例（数组形式）。但因组件层级可能变化，推荐使用 `$refs` 进行精确引用。

## 29. Vue.js 的主要优势？

学习曲线低、轻量高效、渐进式框架设计、优秀的响应式系统、丰富的生态系统（如 Vuex、Vue Router 等）以及灵活的单文件组件结构。

## 30. Vue.js 为何性能优秀？

得益于高效的虚拟 DOM 实现和响应式系统优化。相比传统框架，其打包体积更小，运行时的 Diff 算法更加智能，减少不必要的 DOM 操作。

## 31. 什么是 Vue-loader？

Webpack 加载器，用于处理 `.vue` 单文件组件。主要功能包括解析模板、转译 TypeScript/SCSS 等预处理器、支持 CSS 模块化等。

## 32. 解释 Vue 的响应式系统原理？

使用 ES5 的 `Object.defineProperty`（Vue 2）或 Proxy（Vue 3）对数据对象递归添加 getter/setter。当数据变更时触发依赖收集系统中注册的 watcher，执行视图更新或其他回调。

## 33. 什么是 refs？

通过 `ref` 属性标记元素或子组件，之后可通过 `this.$refs.refName` 访问 DOM 元素或组件实例。常用于焦点管理或直接操作 DOM 元素。

## 34. 如何扩展 Vue 应用功能？

通过 Vue 插件（plugins）。插件通过 `Vue.use()` 方法安装，可添加全局功能如自定义指令、混入等。例如：

```javascript
const MyPlugin = {
  install(Vue) {
    Vue.directive('highlight', {
      inserted(el) {
        el.style.backgroundColor = 'yellow'
      }
    })
  }
}

Vue.use(MyPlugin)
```

## 35. 哪种指令用于更新元素的文本内容？

`v-text` 指令用于完全替换元素的 `textContent`。相比插值表达式 `{{ }}`，`v-text` 可防止初始渲染时的未编译模板闪现。

## 36. 什么是 Watcher？

Watcher 用于观察 Vue 实例上的数据变化。可在选项对象中声明 `watch` 属性，或在代码中通过 `this.$watch()` 创建：

```javascript
export default {
  data() {
    return { query: '' }
  },
  watch: {
    query(newVal) {
      this.search()
    }
  }
}
```

## 37. 哪个指令可用于动态绑定 HTML 属性和样式？

`v-bind` 指令（简写 `:`）用于动态绑定属性、类名和样式。例如绑定类名：

```html
<div :class="{ active: isActive }"></div>
```

## 38. Vue Router 的作用？

官方路由管理器，用于构建单页面应用（SPA）。根据 URL 路径映射到组件视图，并提供编程式导航、嵌套路由、路由参数解析等功能。

## 39. Vue 指令有哪些类型？

分为普通指令（如 v-model）、空指令（无具体参数）、字面指令（硬编码参数）以及自定义指令类型。开发者可注册全局或局部自定义指令。

## 40. 使用混入 (Mixin) 的主要优势？

提高代码复用性，允许多个组件共享相同逻辑（如公共方法、生命周期钩子）。但需注意命名冲突问题，Vue 采用特定策略合并 mixin 与组件本身的选项。

## 41. Vue 实例的生命周期钩子有哪些？

完整的生命周期顺序：

1. beforeCreate (实例初始化前)
2. created (数据观测建立完成)
3. beforeMount (挂载前，模板编译完成)
4. mounted (实例挂载到 DOM)
5. beforeUpdate (数据变更前，DOM 还未更新)
6. updated (DOM 更新完成)
7. beforeDestroy (实例销毁前)
8. destroyed (实例销毁)

## 42. 导航守卫 (Navigation Guards) 的作用？

Vue Router 提供的钩子函数，用于控制导航行为。例如全局前置守卫 `router.beforeEach()` 可进行权限验证：

```javascript
router.beforeEach((to, from, next) => {
  if (to.meta.requiresAuth && !isAuth) next('/login')
  else next()
})
```

## 43. 什么是即时原型开发 (Instant Prototyping)？

通过 `@vue/cli-service-global` 提供的全局命令，无需构建配置即可快速运行单个 `.vue` 文件。例如运行 `vue serve demo.vue` 直接启动开发服务器。

## 44. Vue 中组件的定义？

组件是可复用的 Vue 实例，提供模板、逻辑和样式的封装。每个组件应有单一职责，通过 props/events 与父组件通信，通过插槽 (slots) 支持内容分发。

## 45. 在Vue中如何创建组件？
>
> [深入理解] 该问题需要解释创建组件的两种方式及其差异，展示对构建流程的理解

创建Vue组件主要有两种方式：使用单文件组件SFC（Single-File Component）或在无构建步骤时使用JavaScript对象定义。若存在构建流程（如webpack、Vite等），推荐将组件写在`.vue`文件中：

```vue
<script>
export default {
  data() {
    return {
      name: 'Bob'
    }
  }
}
</script>

<template>
  <h1>Hello {{ name }}</h1>
</template>
```

若无构建步骤，则需直接定义为包含模板选项的JavaScript对象：

```javascript
export default {
  data() {
    return {
      name: 'Bob'
    }
  },
  template: `
    <h1>Hello {{ name }}</h1>
  `
}
```

## 46. Vue中的Props是什么？
>
> [考察内容] 旨在验证对父子组件通信核心机制的理解能力

Props（属性）是组件间通信的基础机制，用于父组件向子组件传递数据。通过在子组件中声明props数组，即可将父级传入的值作为实例属性访问：

```javascript
export default {
  props: ['foo'], // 注册prop
  created() {
    console.log(this.foo) // 通过this访问
  }
}
```

父组件通过属性形式传递值：

```html
<ChildComponent :foo="dynamicValue" />
```

## 47. 描述Vue组件间的数据流机制

组件通信采用严格的单向数据流。父组件通过props向下传递数据（Parent -> Child），而子组件通过emit事件向上通信（Child -> Parent）：

父组件：

```html
<ChildComponent 
  :title="parentData" 
  @child-event="handleEvent"
/>
```

子组件通过$emit触发事件：

```javascript
methods: {
  onClick() {
    this.$emit('child-event', payload)
  }
}
```

## 48. 什么是Slot？
>
> [使用场景] 该特性适用于需要内容分发的复杂组件开发

Slot（插槽）使父组件能够向子组件注入模板片段。基本用法通过`<slot>`标签定义内容占位点：

子组件FancyHeader：

```html
<h1 class="fancy-header">
  <slot></slot> <!-- 这里会替换为父级传递的内容 -->
</h1>
```

父级使用时：

```html
<FancyHeader>
  <span style="color:red">Custom header!</span>
</FancyHeader>
```

## 49. 如何为Slot添加默认内容？

在子组件的slot标签内部直接声明默认内容即可实现fallback机制：

```html
<!-- GreetingComponent -->
<div class="greeting">
  <slot>
    <h2>默认问候语</h2> <!-- 当父级未传内容时显示 -->
  </slot>
</div>
```

使用示例：

```html
<GreetingComponent /> <!-- 显示默认h2 -->
<GreetingComponent>新的内容</GreetingComponent> <!-- 覆盖默认内容 -->
```

## 50. Vue中的属性绑定如何实现？

通过`v-bind`指令或简化语法`:`实现动态属性绑定，核心是将DOM属性与响应式数据建立关联：

```html
<!-- 常规写法 -->
<div v-bind:class="{ active: isActive }"></div>

<!-- 简写形式 -->
<img :src="dynamicImageUrl" >

<!-- 绑定多个属性 -->
<div v-bind="objectOfAttributes"></div>
```

>[技术要点] 该指令支持动态参数，可实现类似`:[attrName]="value"`的灵活绑定方式。绑定后的属性值会随组件状态自动更新。

## 51. Vue中如何实现双向数据绑定？

Vue的双向绑定系统在处理用户输入时非常强大且实用，不仅限于文本输入，还能用于单选框、复选框等其他表单组件。核心机制通过`v-model`指令实现数据同步，该指令会在模板变化时更新模型数据，并在模型变化时更新模板。

例如，将输入框的值与组件数据中的`name`属性绑定：

```javascript
export default {
  data() {
    return {
      name: 'Bob'
    }
  },
  template: `
    <h2>Hello {{ name }}</h2>
    <input v-model="name"/>
  `
}
```

> [考察重点] 需要说明`v-model`实际上是语法糖，内部通过`value`属性绑定和`input`事件监听实现数据同步。这对于理解双向绑定的底层原理至关重要。

## 52. 如何向指令传递多个值？

指令可以通过接收JavaScript对象字面量来处理多个值。例如需要传递`name`和`age`时：

```html
<div v-user="{ name: 'Bob',  age: 30 }"></div>
```

在指令定义中通过`binding.value`访问对象属性：

```javascript
app.directive('user', (el, binding) => {
  console.log(binding.value.name) // => "Bob"
  console.log(binding.value.age) // => 30
})
```

> [实际应用] 这种方式常用于需要复杂参数配置的自定义指令场景，如表单验证指令需同时接收校验规则和错误消息等参数。

## 53. 解释普通插槽和作用域插槽的区别？

普通插槽(Standard Slot)作为子组件的占位符，由父组件填充内容。但其局限性在于无法直接访问子组件内部数据——因为它在父组件的上下文中编译。

作用域插槽(Scoped Slot)则通过`slot-scope`特性突破这一限制：

```html
<!-- 子组件 -->
<slot :userData="user"></slot>

<!-- 父组件使用 -->
<template v-slot:default="slotProps">
  {{ slotProps.userData.name }}
</template>
```

> [设计意图] 作用域插槽适用于需要子组件向插槽内容传递数据的场景，例如表格组件中允许自定义单元格渲染逻辑同时访问行数据。

## 54. v-if和v-show有何区别？

`v-if`通过条件表达式控制元素是否加入DOM树。首次渲染时为假值时完全跳过渲染，适合初始加载性能优化，但频繁切换时消耗较大：

```html
<div v-if="isVisible">Conditional Content</div>
```

`v-show`始终渲染元素到DOM，仅通过CSS的`display`属性切换可见性。适合高频切换场景：

```html
<div v-show="isActive">Toggleable Content</div>
```

> [性能考量] `v-if`有更高的初始渲染成本但运行时消耗低，`v-show`初始成本高但切换成本低。大数据量列表渲染推荐用`v-if`，高频交互元素推荐`v-show`。

## 55. 解释Vue响应式原理及常见变更追踪问题？

Vue通过将data选项的属性转换为带有getter/setter的访问器属性实现响应式。当访问或修改这些属性时，Vue能自动触发依赖追踪和更新。

常见问题包括：

1. **对象属性的动态添加/删除**需使用`Vue.set`：

   ```javascript
   Vue.set(this.obj, 'newProp', value)
   ```

2. **数组索引修改**需要使用变异方法或`Vue.set`：

   ```javascript
   Vue.set(this.items, index, newValue)
   ```

> [原理扩展] 使用Object.defineProperty的局限性导致Vue 3改用Proxy实现全能力响应式系统，可自动处理动态属性变更。

## 56. 列举Vue应用内存泄漏的常见原因及解决方法？

主要发生在第三方库的资源未清理时：

```javascript
mounted() {
  this.chart = new PowerGraph()
},
beforeDestroy() {
  this.chart.destroy() // 必须手动销毁
}
```

典型场景包括：

- 定时器未清除
- 全局事件监听未解绑
- 第三方图形库实例残留

> [最佳实践] 在`beforeDestroy`或组合式API的`onUnmounted`钩子中执行资源释放。使用`vue-use`等工具库可自动化处理事件监听等资源。

## 57. 什么是虚拟DOM及其优势？

虚拟DOM(Virtual DOM)是描述真实DOM结构的JavaScript对象树。通过差异对比算法(diffing algorithm)最小化DOM操作：

```javascript
// 伪代码示例
const vnode = {
  tag: 'div',
  props: { id: 'app' },
  children: [
    { tag: 'p', text: 'virtual node' }
  ]
}
```

优势包括：

1. **批处理更新**：合并多数据变更引起的DOM操作
2. **跨平台能力**：可用于SSR、Native渲染等非浏览器环境
3. **性能优化**：减少直接操作DOM的高昂代价

## 58. Vue支持哪些Prop类型？

Prop类型声明提供类型检查与文档化功能：

```javascript
props: {
  // 基本类型检查
  title: String,
  likes: Number,
  isPublished: Boolean,
  
  // 复杂类型
  commentIds: Array,
  author: Object,
  
  // 高级验证
  validator: {
    type: Function,
    default: () => true
  }
}
```

> [类型系统] Vue 3的TypeScript支持允许使用`PropType`进行更精确的类型定义：

```typescript
import { PropType } from 'vue'
props: {
  metadata: {
    type: Object as PropType<{ id: number; tags: string[] }>,
    required: true
  }
}
```

## 59. 什么是非 prop 属性？

非 prop 属性指传递给组件但未在 props 中定义的属性。比如使用需要添加 `data-tooltip` 属性的第三方输入组件时，可以将该属性直接添加到组件实例：

```vue
<custom-input data-tooltip="输入内容" />
```

相同名称的属性在父组件传递时会覆盖子组件的默认值，但 `class` 和 `style` 属性有特殊处理机制——它们的值会被合并到子组件中：

```vue
<!-- 子组件 -->
<input type="date" class="date-control">

<!-- 父组件 -->
<custom-input class="custom-class" />
```

> [注意机制] class 和 style 的智能合并特性是 Vue 的核心特性之一，面试时应优先解释此机制。

## 60. props 的验证机制有哪些类型？

Vue 提供了类型检测、必填项验证、默认值和自定义验证等多种验证机制。props 可接受包含验证需求的对象：

```javascript
Vue.component('user-profile', {
  props: {
    // 基本类型检测（null 表示允许任意类型）
    age: Number,
    // 多类型支持
    identityNumber: [String, Number],
    // 必填字符串
    email: {
      type: String,
      required: true
    },
    // 带默认值的数值类型
    minBalance: {
      type: Number,
      default: 10000
    },
    // 对象类型需通过工厂函数返回默认值
    message: {
      type: Object,
      default: function () {
        return { message: '欢迎使用 Vue' }
      }
    },
    // 自定义验证函数
    location: {
      validator: function (value) {
        return ['印度', '新加坡', '澳大利亚'].includes(value)
      }
    }
  }
})
```

> [特殊案例] 对象或数组类型必须通过工厂函数设置默认值，这是为了避免不同组件实例间共享同一引用的问题。

## 61. 如何为组件自定义 v-model 指令？

组件的 v-model 默认采用 value 作为 prop，input 作为事件。但对于需要特殊处理的表单控件（如复选框），可以通过 model 选项自定义：

```javascript
Vue.component('custom-checkbox', {
  model: {
    prop: 'checked',    // 指定绑定的属性名
    event: 'change'     // 指定更新事件名
  },
  props: ['checked'],
  template: `
    <input
      type="checkbox"
      :checked="checked"
      @change="$emit('change', $event.target.checked)"
    >
  `
})
```

此时使用时与标准表单控件完全一致：

```vue
<custom-checkbox v-model="selectFramework" />
```

> [实现原理] v-model 实质上是语法糖，这个自定义过程展示了 Vue 如何将双向绑定转化为 prop 与事件的组合。

## 62. Vue 中有哪些实现过渡效果的方法？

实现元素 DOM 变化的过渡效果可以通过以下方式：

1. 自动应用 CSS 过渡/动画类名
2. 集成第三方 CSS 动画库（如 Animate.css）
3. 在过渡钩子中使用 JavaScript 直接操作 DOM
4. 集成第三方 JS 动画库（如 Velocity.js）

> [使用场景] CSS 过渡适合简单动画，JavaScript 方案更适合需要复杂控制的场景。实际开发中常组合使用这两种方式。

## 63. Vue Router 是什么及其核心特性？

Vue Router 是 Vue.js 官方开发的路由管理库，专为单页面应用设计。核心特性包括：

- 嵌套路由映射
- 基于组件的模块化路由配置
- 路径参数/查询参数/通配符
- 基于 Vue 过渡系统的视图切换动画
- 细粒度导航控制
- 自动添加激活状态的 CSS 类
- 支持 HTML5 History 模式与 hash 模式
- history 模式下可恢复滚动位置

> [架构设计] 其组件化的设计理念与 Vue 生态系统深度整合，使得路由配置保持声明式风格。

## 64. 使用 Vue Router 的步骤及示例？

实现路由功能的基本流程如下：

**模板配置**：

```vue
<div id="app">
  <router-link to="/home">首页</router-link>
  <router-link to="/services">服务</router-link>
  <router-view></router-view>
</div>
```

**路由初始化**：

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const Home = { template: '<div>主页内容</div>' }
const routes = [
  { path: '/home', component: Home },
  { path: '/services', component: { template: '<div>服务列表</div>' } }
]

const router = new VueRouter({ routes })
new Vue({ router }).$mount('#app')
```

> [核心概念] router-link 生成导航链接，router-view 作为路由组件渲染出口。通过 Vue.use() 激活路由插件是关键步骤。

## 65. 什么是动态路由匹配？

通过动态路径参数映射到同一组件的路由方式。例如用户组件的路径模式：

```javascript
const User = {
  template: `<div>用户 {{ $route.params.name }}, 文章ID: {{ $route.params.postid }}</div>`
}

const router = new VueRouter({
  routes: [
    { path: '/user/:name/post/:postid', component: User }
  ]
})
```

访问 `/user/john/post/123` 时，组件可通过 `$route.params` 获取参数对象 `{ name: 'john', postid: '123' }`。

> [访问方式] 除了模板中的访问方式，在组件逻辑中也可通过 `this.$route.params` 获取当前路由参数。

## 66. 如何使路由参数变化具有响应性？

当使用带参数的路由在同一个组件下导航时（例如/user/foo → /user/bar），Vue会复用相同的组件实例。虽然这比销毁再创建新实例更高效，但组件的生命周期钩子不会被调用。

>[深入理解] 该问题需要展示对Vue Router响应式机制的深层理解，重点在于路由参数变更后的数据更新策略。可以通过以下两种方案实现：

1. **监听$route对象**：
   在组件选项中设置对$route对象的监听器，触发参数变更时的响应逻辑：

   ```javascript
   const User = {
     template: '<div>User {{ $route.params.name }} </div>',
     watch: {
       '$route'(to, from) {
         // 此处应对路由变化作出反应...
         // 比如请求新的用户数据
       }
     }
   }
   ```

2. **使用beforeRouteUpdate导航守卫**（该特性从2.2版本开始可用）：
   该守卫会在当前路由改变且组件复用时触发，特别适合预处理路由参数变化：

   ```javascript
   const User = {
     template: '<div>User {{ $route.params.name }} </div>',
     beforeRouteUpdate(to, from, next) {
       // 必须执行next()以完成导航
       // 通常在这里执行数据请求后再调用next()
       next()
     }
   }
   ```

> [注意] beforeRouteEnter守卫由于组件尚未创建，无法通过`this`访问组件实例，但可通过`next(vm => { ... })`回调获取vm实例。

## 67. 什么是路由匹配优先级？

当多个路由配置同时匹配同一个URL时，路由匹配优先级由路由配置的顺序决定。先定义的路由具有更高的优先级，这个机制解决了路由映射冲突的问题。

>[示例说明] 以下配置中第一个路由总是优先匹配：

```javascript
const router = new VueRouter({
   routes: [
     { path: '/user/:name', component: User }, // 这条路由优先级最高
     { path: '/user/:name', component: Admin },
     { path: '/user/:name', component: Customer }
   ]
})
```

## 68. 什么是嵌套路由？

当应用程序需要呈现多层嵌套的组件结构时，嵌套路由可通过`children`配置实现。URL的各路径段对应不同层级的组件结构，嵌套的`<router-view>`出口用于渲染子组件。

>[配置示例] 用户详情页包含个人资料和文章的嵌套路由：

```javascript
const router = new VueRouter({
  routes: [
    { 
      path: '/user/:id', 
      component: User,
      children: [
        { path: 'profile', component: UserProfile },  // /users/1/profile
        { path: 'posts', component: UserPosts },      // /users/1/posts
        { path: '', component: UserHome }             // /users/1
      ]
    }
  ]
})
```

>[注意] 空路径子路由（path: ''）作为默认视图，在父路由路径被访问时渲染。

## 69. 什么是单文件组件？

单文件组件（.vue文件）将模板、逻辑和样式整合在单一文件中，提供组件化的完整封装。这种方式解决了传统分离文件带来的协作问题：

```vue
<template>
  <div>
    <h1>Welcome {{ name }}!</h1>
  </div>
</template>

<script>
export default {
  data() {
    return { name: 'John' }
  }
}
</script>

<style scoped>
h1 {
  color: #34c779;
  padding: 3px;
}
</style>
```

>[特性] 三个区块使用自定义语言（如Pug、SCSS）时，可通过webpack等构建工具预处理，并与热重载完美配合。

## 70. 单文件组件是否违反关注点分离原则？

在现代前端开发中，关注点分离不等同于文件类型分离。单文件组件通过以下方式实现更合理的关注点管理：

> [设计哲学] 将密切关联的模板、逻辑和样式组合在同一个组件文件中，保持功能内聚。当需要拆分为独立文件时，仍可通过模块化导入：

```vue
<template>
  <div>独立模块化开发的典型案例</div>
</template>
<script src="./component-logic.js"></script>
<style src="./component-style.css"></script>
```

这种方法既可维护组件的整体性，又能实现关注点的逻辑分离，同时享受构建工具（如webpack）的热重载支持和预处理器整合。

## 71. 单文件组件解决了哪些关键问题？

单文件组件主要解决传统开发模式中的四大痛点：

1. **全局命名冲突**：避免通过Vue.component()全局注册组件时的命名限制  
2. **字符串模板缺陷**：解决未格式化的HTML字符串缺少语法高亮和多行书写困难的问题  
3. **CSS隔离缺失**：通过scoped属性实现组件级样式作用域控制  
4. **构建能力局限**：支持使用Pug/Babel/Sass等预处理器提升开发体验

## 72. 如何进行过滤器链式调用？

在表达式后连续使用管道符（|）可形成过滤器链。前一个过滤器的输出将作为后一个过滤器的输入，适用于多步骤数据处理：

```vue
<!-- 格式化日期后转为大写 -->
{{ birthDate | formatDate | uppercase }}
```

>[执行流程] 表达式值依次经过formatDate转换（得到格式化字符串），再传递给uppercase进行大写转换，形成完整的处理流水线。

## 73. 能否给过滤器传递参数？

可通过类似函数调用的语法为过滤器传递额外参数，原生数据作为第一个隐式参数：

```vue
<!-- 计算2的10次方 -->
{{ 2 | exponential(10) }} <!-- 输出1024 -->
```

>[实现示例] 对应的过滤器定义：

```javascript
Vue.filter('exponential', function(base, exponent) {
  return Math.pow(base, exponent)
})
```

>[参数传递] 此处base参数对应原始值2，explicit参数对应显示传递的10。

## 74. 插件及其服务是什么？

插件为 Vue 应用提供全局级别的功能扩展，通常通过以下五种方式实现：

1. 添加全局方法或属性。例如 vue-custom-element 添加自定义元素支持
2. 添加全局资源（directives/指令，filters/过滤器和 transitions/过渡效果）。比如 vue-touch 提供手势指令
3. 通过全局 mixin/混入 扩展组件选项。如 vue-router 的路由混入行为
4. 在 Vue.prototype 上添加实例方法
5. 组合上述方式的综合型 API 库。典型代表是 vue-router

> [深入理解] 这个问题主要考察对 Vue 插件作用域的理解，重点在于区分全局功能扩展的不同维度。

## 75. 如何创建插件？

通过暴露接收 Vue 构造函数和选项参数的 `install` 方法创建。典型插件结构需包含以下核心要素：

```javascript
MyPlugin.install = function(Vue, options) {
  // 1. 全局方法（注意不是实例方法）
  Vue.myGlobalAPI = () => console.log('全局方法调用')

  // 2. 全局指令注册
  Vue.directive('highlight', {
    inserted(el) {
      el.style.backgroundColor = 'yellow'
    }
  })

  // 3. 组件选项混入
  Vue.mixin({
    created() {
      console.log('每个组件都会触发的created钩子')
    }
  })

  // 4. 原型链方法扩展
  Vue.prototype.$utility = function() {
    // 实例方法通过this访问
  }
}
```

> [技术要点] install 方法是插件的必选入口，第一个参数保证 Vue 构造函数的版本兼容性，options 可实现插件配置化。

## 76. 如何使用插件？

通过全局方法 `Vue.use()` 激活插件，必须在 new Vue() 初始化前调用。典型用法：

```javascript
// 可选配置参数作为第二个参数传递
Vue.use(MyPlugin, { theme: 'dark' })

// 初始化应用实例
new Vue({
  el: '#app'
})
```

核心原理是调用插件的 install 方法并标记已安装状态避免重复注册。值得注意的是，某些插件（如官方插件）会自动调用 use 方法。

## 77. 自定义选项合并策略是什么？

Vue 默认使用覆盖策略处理自定义选项的合并。要自定义合并逻辑，需通过 `Vue.config.optionMergeStrategies` 注册策略函数。

示例：针对自定义选项 `myOption` 的策略实现

```javascript
Vue.config.optionMergeStrategies.myOption = (parentVal, childVal) => {
  return childVal !== undefined ? childVal : parentVal
}
```

进阶示例参考 Vuex 1.0 的合并逻辑：

```javascript
const merge = Vue.config.optionMergeStrategies.computed
Vue.config.optionMergeStrategies.vuex = (toVal, fromVal) => ({
  getters: merge(toVal.getters, fromVal.getters),
  state: merge(toVal.state, fromVal.state),
  actions: merge(toVal.actions, fromVal.actions)
})
```

> [使用场景] 主要用于处理自定义组件选项在 mixins 或 extends 场景下的合并逻辑，如 Vuex 的 store 合并。

## 78. 什么是自定义指令？

自定义指令（Custom Directives）是附加到 DOM 元素的轻量级命令，前缀为 v-。常用于需要直接操作 DOM 的低层级交互。以下示例实现输入框自动聚焦：

```javascript
// 全局注册
Vue.directive('focus', {
  inserted(el) {
    el.focus()
  }
})
```

使用方式：

```vue
<input v-focus>
```

> [核心价值] 提供声明式 DOM 操作能力，将 dom 交互逻辑封装为可复用指令。

## 79. 如何局部注册指令？

在组件配置的 directives 选项中注册局部指令：

```javascript
export default {
  directives: {
    focus: {
      inserted(el) {
        el.focus()
      }
    }
  }
}
```

用法与全局指令一致：

```vue
<template>
  <input v-focus>
</template>
```

## 80. 指令提供哪些钩子函数？

指令生命周期包含五个关键钩子：

- bind：仅在指令初次绑定到元素时触发（适合一次性初始化）
- inserted：绑定元素插入父节点后触发（可访问到 DOM 上下文）
- update：所在组件 VNode 更新时触发（但可能子组件尚未更新）
- componentUpdated：所在组件及子组件全部更新后触发
- unbind：指令与元素解绑时触发（适合清理工作）

> [注意点] update 和 componentUpdated 的触发时机差异，例如操作子元素时可能需要等待 componentUpdated。

## 81. 指令钩子的参数有哪些？

所有钩子接收这三个核心参数：

1. `el`：指令绑定的 DOM 元素（可直接操作）
2. `binding`：包含以下属性的对象
   - `name`：指令名（去 v- 前缀）
   - `value`：绑定值（支持动态表达式结果）
   - `oldValue`：仅在 update/componentUpdated 可用
   - `expression`：原始表达式字符串
   - `arg`：指令参数（如 v-my-dir:foo 中的 "foo"）
   - `modifiers`：修饰符对象（如 v-my-dir.bar 得到 { bar: true }）
3. `vnode`：Vue 生成的虚拟节点

update 和 componentUpdated 额外接收 oldVnode 参数用于差异比较。

## 82. 如何传递多值到指令？

通过 JavaScript 对象字面量传递结构化参数：

模板中使用：

```vue
<template>
  <div v-avatar="{ width: 300, text: 'Tony', size: 'lg' }"></div>
</template>
```

指令实现：

```javascript
Vue.directive('avatar', (el, binding) => {
  console.log(binding.value.width) // 300
  console.log(binding.value.text)  // "Tony"
})
```

> [最佳实践] 使用对象传参更具可读性和扩展性，尤其当参数存在多个可选配置项时。

## 83. 什么是指令钩子中的函数简写？

在少数情况下，可能需要为`bind`和`update`钩子定义完全相同的操作而忽略其他钩子。这时候可以直接用函数形式简写指令：

```javascript
Vue.directive('theme-switcher', function (el, binding) {
  el.style.backgroundColor = binding.value
})
```

> [深入理解] 此处考察对指令优化的理解，通过函数简写同时应用逻辑到最常用的两个生命周期钩子。

## 84. Render函数相比模板有什么优势？

VueJS的模板系统非常强大且推荐用于构建HTML界面，但某些特殊场景（例如根据输入动态创建组件或操作插槽内容）需要借助render函数实现。它能够给予开发者完全的程序控制能力，充分发挥JavaScript生态的灵活性。比如当需要基于运行时条件动态生成组件层级时，render函数比模板更直接高效。

> [考察内容] 面试重点在于理解何时需要突破模板的限制，掌握两者在灵活性上的差异。

## 85. 什么是Render函数？

Render函数是接收`createElement`方法作为第一个参数的普通函数，用于创建虚拟节点(VNode)。Vue的模板在编译阶段实际上都会被转换成render函数，可以说模板只是render函数的语法糖。

示例模板代码：

```vue
<template>
  <div :class="{'is-rounded': isRounded}">
    <p>Welcome to Vue render functions</p>
  </div>
</template>
```

对应的显式render函数写法：

```javascript
render: function (createElement) {
  return createElement('div', {
    'class': {
      'is-rounded': this.isRounded
    }
  }, [
    createElement('p', 'Welcome to Vue render functions')
  ]);
}
```

> [技术背景] React组件使用JSX构建，本质上也是render函数。Vue的render函数机制类似但采用纯JavaScript实现。

## 86. 如何在组件中编写重复的虚拟节点？

组件树中的所有虚拟节点必须唯一。如果需要重复相同元素/组件，必须通过工厂函数实现。直接复用变量创建重复节点是无效写法：

```javascript
// 错误示例
render: function (createElement) {
  var myHeadingVNode = createElement('h1', 'This is a Virtual Node')
  return createElement('div', [
    myHeadingVNode, myHeadingVNode, myHeadingVNode
  ])
}
```

正确的工厂函数方式：

```javascript
render: function (createElement) {
  return createElement('div',
    Array.apply(null, { length: 3 }).map(function () {
      return createElement('h1', 'This is a Virtual Node')
    })
  )
}
```

> [原理剖析] 虚拟节点包含上下文信息，直接复用会导致DOM更新异常。工厂函数每次生成独立实例确保状态隔离。

## 87. 列出Render函数中模板特性的等价实现方式？

Vue为模板特性提供了对应的JavaScript实现方案：

| 模板特性                | Render函数实现方式                                                                 |
| v-if / v-for            | 使用JavaScript的if/else和数组map                                                 |
| v-model                 | 手动实现value属性绑定和input事件监听                                             |
| 事件修饰符(.passive等)  | 使用&、!、~符号组合                                                              |
| 按键修饰符(.stop等)     | 通过原生事件处理：event.stopPropagation()、检查event.keyCode等                   |
| 插槽                    | 通过实例属性this.$slots和this.$scopedSlots访问                                   |

## 88. 什么是函数式组件？

函数式组件是通过定义`functional: true`创建的无状态组件，具有两个核心特征：

1. **无状态**：不维护响应式数据
2. **无实例**：没有this上下文

示例声明：

```javascript
Vue.component('my-component', {
  functional: true,
  props: {/*...*/},
  render: function (createElement, context) {
    // 通过context参数访问props、slots等
  }
})
```

> [使用场景] 适合纯展示型组件，相比常规组件有更高的渲染性能。React社区也有类似的函数组件概念。

## 89. VueJS与ReactJS有哪些相似之处？

虽然架构不同，但仍存在核心相似点：

1. 都基于**Virtual DOM（虚拟DOM）**模型进行DOM更新优化
2. 采用组件化架构和响应式数据机制
3. 核心库专注视图层，路由/状态管理等通过扩展库实现
4. 都支持服务端渲染(SSR)能力

## 90. VueJS与ReactJS有何主要区别？

核心差异点对比：

| 特性                  | VueJS                              | ReactJS                          |
| 类型                  | MVVM框架                          | UI库                            |
| 跨平台支持            | 主要针对Web开发                   | 支持Web和Native（React Native） |
| 学习曲线              | 较低，API简洁                     | 较高，需深入理解JSX和函数式编程 |
| 组件状态管理          | Options API / Composition API     | 类组件 / Hooks                  |
| 模板系统              | 支持HTML模板                      | 使用JSX语法                     |

## 91. Vue相比React有哪些优势？

主要优势包括：

1. 更小的体积和更快的运行速度
2. 模板语法对传统Web开发者更友好
3. 单文件组件形式提供更好的代码组织
4. 响应式系统自动追踪依赖，无需手动优化渲染
5. 官方维护的路由、状态管理等生态更集成

## 92. React相比Vue有哪些优势？

核心优势体现于：

1. 更灵活的架构适合大型复杂应用
2. 虚拟DOM差异算法优化更彻底
3. 完善的TypeScript支持
4. 更成熟的跨平台开发方案（React Native）
5. 更庞大的开发者社区和生态系统

## 93. VueJS与Angular有何主要区别？

关键差异对比：

| 特性              | VueJS                          | Angular                      |
| 架构类型          | 渐进式框架                    | 完整MVW框架                  |
| 数据绑定          | 单向绑定                      | 双向绑定                     |
| 学习难度          | 较低，官方文档友好            | 较高，需掌握TypeScript和RxJS |
| 发布背景          | 尤雨溪独立开发                | Google维护                   |
| 初始版本          | 2014年2月                     | 2016年9月（指Angular 2+）    |
| 组件通信          | Props/Events                  | @Input/@Output装饰器         |

## 94. 什么是动态组件 (Dynamic Components)？

动态组件允许在不更新应用路由的情况下动态切换多个组件，并且在切换回初始组件时保留其状态。通过使用带有`v-bind:is`属性的`<component>`标签实现，该属性接收动态组件名。

下面示例展示如何通过 Vue 实例在不同页面间切换：

```javascript
new Vue({
  el: '#app',
  data: {
    currentPage: 'home'
  },
  components: {
    home: { template: "<p>Home</p>" },
    about: { template: "<p>About</p>" },
    contact: { template: "<p>Contact</p>" }
  }
})
```

在 HTML 中使用动态组件：

```html
<div id="app">
  <component v-bind:is="currentPage">
    <!-- 当 currentPage 变化时组件切换 -->
    <!-- 输出：Home -->
  </component>
</div>
```

> [注意] `:is`的值可以是注册的组件名称或直接导入的组件对象。默认切换时会卸载非活跃组件，通过包裹`<keep-alive>`可保持其存活状态。

## 95. keep-alive 标签的作用是什么？

`<keep-alive>`作为抽象组件用于缓存非活跃组件实例，避免状态丢失或重复渲染。适用于需要保留表单数据等场景：

```html
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

处理多条件子组件时，要求同一时间只渲染一个：

```html
<keep-alive>
  <comp-a v-if="a > 1"></comp-a>
  <comp-b v-else></comp-b>
</keep-alive>
```

> [关键点] 该标签不渲染为 DOM 元素，也不出现在组件父链中。主要解决组件切换时状态丢失问题，通过内存缓存提高性能。

## 96. 什么是异步组件 (Async Components)？

异步组件通过工厂函数延迟加载组件定义，用于优化大型应用的代码分割。例如使用 Webpack 的代码拆分功能：

```javascript
Vue.component('async-webpack-example', function (resolve, reject) {
  require(['./my-async-component'], resolve)
})
```

> [工作机制] Vue 只在首次渲染时触发工厂函数并缓存结果。适合按需加载提高初始加载速度，常用于路由懒加载场景。

## 97. 异步组件工厂的结构是怎样的？

异步组件工厂返回包含加载逻辑的配置对象：

```javascript
const AsyncComponent = () => ({
  component: import('./MyComponent.vue'), // Promise 形式的组件加载
  loading: LoadingComponent, // 加载中显示组件
  error: ErrorComponent, // 加载失败显示组件
  delay: 200, // 展示 loading 前的延迟
  timeout: 3000 // 超时时间
})
```

> [应用场景] 这种结构允许精细控制加载过程，适用于需要友好加载提示和错误处理的复杂场景。

## 98. 什么是内联模板 (Inline Templates)？

通过`inline-template`特性将子元素内容作为模板使用：

```html
<my-component inline-template>
  <div>
    <h1>内联模板</h1>
    <p>直接作为组件模板使用</p>
  </div>
</my-component>
```

> [注意事项] 虽提供灵活性，但建议使用`<template>`标签或单文件组件方式定义模板，以保持代码结构清晰。

## 99. 什么是 X-Templates？

通过`script`标签定义模板的特殊方式：

```html
<script type="text/x-template" id="script-template">
  <p>欢迎使用 X-Template</p>
</script>
```

在组件中引用：

```javascript
Vue.component('x-template-example', {
  template: '#script-template'
})
```

> [适用场景] 快速原型设计时使用，但在生产环境推荐单文件组件形式。

## 100. 如何解决组件间的循环依赖？

当组件 A 和 B 相互引用时，采用两种解决方案：

**方案1**：在`beforeCreate`钩子动态注册组件

```javascript
beforeCreate() {
  this.$options.components.componentB = require('./component-b.vue').default
}
```

**方案2**：使用异步导入

```javascript
components: {
  componentB: () => import('./component-b.vue')
}
```

> [原理分析] 动态加载打破编译阶段的依赖链，运行时按需加载避免循环引用错误。

## 101. 如何确保 Vue 应用符合 CSP（内容安全策略）？

使用仅运行时构建 (runtime-only build) 配合 Webpack + vue-loader：

```javascript
// webpack 配置
module.exports = {
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.runtime.esm.js'
    }
  }
}
```

> [背景知识] CSP 禁止`new Function`，而全构建 (full build) 依赖此功能编译模板。运行时构建通过预编译模板绕过此限制。

## 102. 全构建 (Full) 和运行时构建 (Runtime Only) 有何区别？

| 构建类型       | 特性                                                                 |
| **全构建**     | 包含模板编译器，支持`template`选项和动态模板编译                   |
| **运行时构建** | 仅包含核心功能，需预编译模板，体积减小约 6KB（min+gzip）          |

> [使用建议] 生产环境优先使用运行时构建，通过构建工具预编译模板以获得最佳性能和安全性。

## 103. Vue.js 有哪些不同的构建版本？

根据构建类型的不同，Vue.js 主要提供以下构建版本：

| 类型                  | UMD               | CommonJS          | ES Module (for bundlers) | ES Module (for browsers) |
| 完整版                | vue.js           | vue.common.js     | vue.esm.js              | vue.esm.browser.js       |
| 仅运行时版            | vue.runtime.js    | vue.runtime.common.js | vue.runtime.esm.js   | 不支持                   |
| 完整版（生产环境）      | vue.min.js        | 不支持              | 不支持                   | vue.esm.browser.min.js   |
| 仅运行时版（生产环境）   | vue.runtime.min.js | 不支持              | 不支持                   | 不支持                   |

> [深入理解] 构建版本差异主要在于是否包含模板编译器，完整版包含编译器但体积较大，适用于需要动态编译模板的场景；运行时版本更轻量，但需要配合构建工具预编译模板。

## 104. 如何在 Webpack 中配置 Vue.js？

可通过设置别名（alias）来配置 Vue.js 的构建版本。示例配置如下：

```javascript
module.exports = {
  // ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js' // webpack 1 使用 'vue/dist/vue.common.js'
    }
  }
}
```

> [考察内容] 该配置主要解决 webpack 默认使用仅运行时版本（runtime-only）的问题，通过显式指定包含编译器的构建文件来支持模板编译。

## 105. Vue.js 编译器的作用是什么？

编译器负责将模板字符串转换为 JavaScript 渲染函数（render functions）。存在编译器版本和运行时版本的主要区别可以通过以下示例说明：

```javascript
// 需要编译器
new Vue({
  template: '<div>{{ message }}</div>'
})

// 不需要编译器（直接使用渲染函数）
new Vue({
  render (h) {
    return h('div', this.message)
  }
})
```

> [需要解释] 包含编译器的版本会增加约 30% 体积，在构建阶段预编译模板可提高运行时性能，因此生产环境建议使用预编译方案。

## 106. 如何访问根实例？

可通过 `$root` 属性访问通过 `new Vue()` 创建的根实例。示例展示根实例属性的访问方式：

```javascript
// 创建根实例
new Vue({
  data: { age: 26 },
  computed: { fullName: () => { /*...*/ } },
  methods: { interest: () => { /*...*/ } }
})

// 子组件中访问
this.$root.age = 29          // 设置数据
const name = this.$root.fullName // 访问计算属性
this.$root.interest()        // 调用方法
```

> [补充建议] 虽然便捷，但过度依赖 `$root` 会导致组件耦合，推荐使用 Vuex 进行全局状态管理。

## 107. renderError 的作用是什么？

当默认渲染函数遇到错误时，`renderError` 可作为备用渲染方案，接收错误对象作为第二个参数。示例：

```javascript
new Vue({
  render(h) { throw new Error('发生了错误') },
  renderError(h, err) {
    return h('div', { style: { color: 'red' }}, err.stack)
  }
}).$mount('#app')
```

> [考察重点] 此特性主要用于开发阶段展示错误详情，生产环境需通过全局错误处理机制捕获异常。

## 108. 如何访问父组件实例？

通过 `$parent` 属性可访问直接外部作用域的父组件实例，父子组件关系通过该属性建立。访问方式：

```javascript
this.$parent.parentData // 访问父组件数据
this.$parent.parentMethod() // 调用父组件方法
```

父组件中对应的子组件实例保存在 `$children` 数组中。该方式适用于简单层级通信，复杂场景建议使用 props/events 或 Vuex。

## 109. 什么是 Vuex？

Vuex 是专为 Vue.js 应用程序设计的**状态管理模式 + 库**，采用 Flux 架构思想。它提供集中式存储管理应用的所有组件状态，并以可预测的方式修改状态。

## 110. 状态管理模式的核心组件是什么？

该模式包含三个核心要素：

1. **状态（State）**：驱动应用的数据源
2. **视图（View）**：状态的可视化映射
3. **操作（Actions）**：响应视图交互的状态变化途径

示例实现计数器应用：

```javascript
new Vue({
  data() { return { count: 0 } }, // 状态
  template: `<div>{{ count }}</div>`, // 视图
  methods: { increment() { this.count++ } } // 操作
})
```

## 111. 如何在 Vuex 中体现单向数据流？

通过以下模式实现单向数据流：

```
视图 → 触发动作 → 修改状态 → 更新视图
```

与 Vue 原生 props 机制的单向数据流概念一致，Vuex 通过严格的状态变更流程（mutations）确保可追溯性。

## 112. 什么是 Vue Loader？

Vue Loader 是 webpack 的加载器（loader），支持以单文件组件（Single-File Components，SFCs）格式编写 Vue 组件。典型单文件结构如下：

```vue
<template>
  <div class="greeting">{{ message }}</div>
</template>

<script>
export default {
  data() { return { message: 'Hello world' } }
}
</script>

<style>
.greeting { color: blue; }
</style>
```

## 113. 如何在 Webpack 中配置 Vue Loader？

配置需使用 Vue Loader 插件，示例配置展示对 .vue 文件的处理：

```javascript
const VueLoaderPlugin = require('vue-loader/lib/plugin')

module.exports = {
  plugins: [new VueLoaderPlugin()],
  module: {
    rules: [
      { test: /\.vue$/, loader: 'vue-loader' },
      { test: /\.js$/, loader: 'babel-loader' },
      { test: /\.css$/, use: ['vue-style-loader', 'css-loader'] }
    ]
  }
}
```

> [关键点] VueLoaderPlugin 负责将其他规则（如 JS/CSS 处理）自动应用到 SFC 文件的相应区块中。

## 114. 能否为 CSS 模块使用自定义注入名称？

可以通过给 module 属性赋值来自定义注入的计算属性名称。这在单个 *.vue 组件中存在多个 `<style>` 标签时，能有效避免样式注入冲突。

示例代码：

```javascript
<style module="a">
  /* 注入标识符为 a */
</style>

<style module="b">
  /* 注入标识符为 b */
</style>
```

> [深入理解] 该特性主要用于解决多样式模块共存时的命名空间问题，通过不同命名实现样式隔离

## 115. Vue Loader 中的热重载是什么？

热重载并非在编辑 .vue 文件时刷新页面，而是实时替换当前组件所有实例而不刷新页面。这个特性在调试组件模板或样式时能显著提升开发体验。

> [换个问法] "能否解释热更新与普通刷新的区别？" 这里考察对实时编译机制的理解

## 116. 热重载的默认行为是什么？

在以下三种情况下会自动禁用热重载：

1. webpack 的 target 配置为 node（SSR 场景）
2. webpack 进行代码压缩时
3. 环境变量 process.env.NODE_ENV 设置为 production

## 117. 如何显式关闭热重载？

在 webpack 配置中设置 `hotReload: false` 选项即可：

```javascript
module: {
  rules: [
    {
      test: /\.vue$/,
      loader: 'vue-loader',
      options: {
        hotReload: false // 关闭热重载
      }
    }
  ]
}
```

## 118. 如何使用热重载功能？

Vue Loader 内置热重载机制，通过 vue-cli 创建的项目已默认集成。手动配置需在 webpack-dev-server 启动时添加 `--hot` 参数：

> [补充说明] 热重载需要 webpack 的 Hot Module Replacement 配合实现

## 119. 热重载的状态保持规则有哪些？

状态保持遵循三原则：

1. 修改 `<template>` 时，组件实例就地重新渲染，保留私有状态
2. 修改 `<script>` 时，组件实例会被销毁重建
3. 修改 `<style>` 通过 vue-style-loader 单独热更新，不影响应用状态

## 120. 如何使用 Vue Loader 创建函数式组件？

在 template 标签添加 functional 属性即可创建函数式组件：

```javascript
<template functional>
  <div>{{ props.msg }}</div>
</template>
```

## 121. 如何在函数式组件中访问全局属性？

通过 parent 属性链访问 Vue.prototype 上的全局属性：

```javascript
<template functional>
  <div>{{ parent.$someProperty }}</div>
</template>
```

## 122. Vue.js 如何进行测试？

两种主要测试方式：

1. **使用 vue-cli**：提供开箱即用的单元测试与端到端测试配置
2. **手动配置**：通过 mocha-webpack 或 jest 对 *.vue 文件进行单元测试

## 123. 如何实现 CSS 代码检查？

使用 stylelint 检查 Vue 单文件组件样式：

```javascript
stylelint MyComponent.vue
```

或在 webpack 配置 stylelint-webpack-plugin：

```javascript
const StyleLintPlugin = require('stylelint-webpack-plugin');
module.exports = {
  plugins: [
    new StyleLintPlugin({
      files: ['**/*.{vue,htm,html,css,sss,less,scss,sass}'],
    })
  ]
}
```

## 124. 如何使用 ESLint 插件？

通过 eslint-plugin-vue 检查组件模板和脚本：

```javascript
// .eslintrc.js
module.exports = {
  extends: ["plugin:vue/essential"]
}
```

运行检查命令：

```javascript
eslint --ext js,vue MyComponent.vue
```

## 125. ESLint Loader 的作用是什么？

用于开发时自动检查 *.vue 文件，安装后配置为 webpack 预处理器：

```javascript
npm install -D eslint eslint-loader

// webpack.config.js
module: {
  rules: [
    {
      enforce: 'pre',
      test: /\.(js|vue)$/,
      loader: 'eslint-loader',
      exclude: /node_modules/
    }
  ]
}
```

## 126. stylelint 有哪些特性？

主要特性包括：

1. 160+ 内置规则检测错误和规范样式
2. 支持 CSS 新特性如自定义属性和 Level4 选择器
3. 可提取 HTML、Markdown 和 CSS-in-JS 中的样式
4. 支持 SCSS、Sass、Less 等预处理语法
5. 丰富的插件生态体系

## 127. Vuex 应用结构遵循哪些原则？

三个核心原则：

1. 应用级状态集中存储于 store
2. 只能通过提交 mutation 同步修改状态
3. 异步操作封装在 action 中处理

## 128. Vuex 是否支持热重载？

支持开发环境下的 mutations、modules、actions 和 getters 热更新。需使用 webpack 的 HMR API 或 browserify 的热更新插件实现。

## 129. vuex store 中 hotUpdate API 的目的是什么？

主要用于热更新 mutations 和模块。通过调用`store.hotUpdate()`方法实现开发环境中的模块热替换。比如在开发环境中可以这样配置：

```javascript
// store.js
if (module.hot) {
  module.hot.accept(['./mutations', './modules/newMyModule'], () => {
    const newMutations = require('./mutations').default
    const newMyModule = require('./modules/myModule').default
    store.hotUpdate({
      mutations: newMutations,
      modules: {
        moduleA: newMyModule
      }
    })
  })
}
```

> [深入理解] 该问题需要说明热更新的具体场景——通常在代码修改后保持当前应用状态的同时加载最新模块。此处通过 Webpack 的 HMR 机制监听文件变化，动态替换 mutations 或模块实现无刷新更新。

## 130. 如何测试 mutations？

mutations 作为纯函数可通过导入后直接调用测试。例如在测试增量 mutation 时：

```javascript
// mutations.spec.js
it('INCREMENT', () => {
  const state = { counter: 10 }
  increment(state)
  expect(state.counter).to.equal(11)
})
```

> [考察内容] 需要展示如何解构导出 mutations 并模拟状态对象。此处强调测试应关注函数的输入输出，不依赖 Vuex 的运行环境。

## 131. 如何测试 getters？

getters 测试方法与 mutations 类似，重点验证状态派生逻辑。例如测试待办筛选功能时：

```javascript
// getters.spec.js
it('filteredTodos', () => {
  const state = { todos: [...] } // 模拟带三种状态的待办数据
  const result = getters.filterTodos(state, 'Completed')
  expect(result).to.deep.equal([/* 预期完成的待办项 */])
})
```

> [示例说明] 代码展示了如何构造虚拟状态并验证过滤结果。.deep.equal 用于深度比较对象数组，特别适合这种返回复杂结构的测试场景。

## 132. 如何在 node 环境中运行测试？

通过 Webpack 打包测试文件并配合 Mocha 命令行运行，主要步骤：

1. 创建包含 Babel 的 Webpack 配置，指定入口为测试文件
2. 打包后使用 `mocha test-bundle.js` 执行测试

```javascript
// webpack.config.js 关键配置
module.exports = {
  entry: './test.js',
  module: {
    loaders: [{
      test: /\.js$/,
      loader: 'babel-loader'
    }]
  }
}
```

> [技术细节] 需要注意 mock 浏览器 API 的依赖（如 DOM 操作），通过 webpack 的打包能力消除环境差异。

## 133. 如何在浏览器中运行测试？

通过 webpack-dev-server 实时执行测试：

1. 安装 mocha-loader
2. 修改 webpack 入口为 `'mocha-loader!babel-loader!./test.js'`
3. 访问 dev server 的 `/webpack-dev-server/test-bundle` 路由查看结果

## 134. vuex 严格模式的目的是什么？

启用后，任何在 mutation 外部修改状态的操作都会抛出错误。通过配置选项开启：

```javascript
const store = new Vuex.Store({
  strict: true
})
```

> [核心原理] 使用深度观察器追踪状态变更，确保所有变更都通过 mutation 进行。这对调试非常重要，但需注意性能成本。

## 135. 生产环境可以使用严格模式吗？

不建议，因深度监听会产生性能开销。正确做法是通过环境变量控制：

```javascript
strict: process.env.NODE_ENV !== 'production'
```

## 136. 是否需要将全部局部状态迁移到 vuex？

不需要。仅属于单个组件的局部状态可保留在组件内。过度使用 vuex 会导致代码冗余，应仅在需要跨组件共享或复杂状态管理时使用 Store。

> [取舍标准] 当状态需要响应式跨组件共享，或有复杂变更逻辑需要跟踪时，才是引入 Vuex 的合适场景。

## 137. vuex getters 的作用是什么？

作为 store 的计算属性缓存派生状态。例如从 todo 列表中筛选已完成项：

```javascript
getters: {
  completedTodos: state => state.todos.filter(t => t.completed)
}
```

> [特性说明] 类似 computed 的缓存机制，只有依赖数据变化时才重新计算。注意 getters 的第一个参数是 state，且可通过返回函数实现带参数查询。

## 138. 什么是属性风格的getter访问形式？

通过属性方式直接访问store的getters对象（store.getters）被称为属性风格访问。这种方式允许像读取普通属性那样调用计算后的状态值。

示例中访问待办事项状态的方式：

```javascript
store.getters.todosStatus
```

在另一个getter内部调用时，可以将getters作为第二个参数传递：

```javascript
getters: {
  completedTodosCount: (state, getters) => {
    return getters.todosStatus === 'completed'
  }
}
```

> [注意点] 这种属性访问方式会被Vue的响应式系统自动缓存，有效避免重复计算。

## 139. 方法风格的getter访问有什么特点？

通过传递参数实现动态查询的getter访问方式。这类getter实际上返回的是函数，使得调用时可以传入特定条件进行数据筛选。

用户查询的示例实现：

```javascript
getters: {
  getUserProfileById: (state) => (id) => {
    return state.users.find(user => user.id === id)
  }
}
```

调用时使用函数传参方式：

```javascript
store.getters.getUserProfileById(111); // 返回 {id: '111', name: 'John', age: 33}
```

## 140. mapGetters辅助函数的作用是什么？

该工具函数用于将store中的getters自动映射到组件的计算属性。通过简化getter的引入过程，避免手动声明每个计算属性。

待办事项应用中的典型用法：

```javascript
import { mapGetters } from 'vuex'

export default {
  computed: {
    // 使用对象展开运算符混合到计算属性中
    ...mapGetters([
      'completedTodos',
      'todosCount',
      // ...
    ])
  }
}
```

## 141. 如何定义Vuex的mutations？

Mutation是Vuex中唯一允许修改状态的方式，每个mutation包含字符串类型的`type`和对应的处理函数（handler）。处理函数接收state作为第一个参数，负责实际的状态变更。

计数器应用的典型mutation定义：

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

触发时需通过commit方法：

```javascript
store.commit('increment')
```

## 142. 如何在commit时携带payload？

通过为commit方法添加第二个参数传递额外数据。payload可以是任意类型的数据，通常用于动态修改状态值。

带payload的自增mutation：

```javascript
// 定义
mutations: {
  increment (state, payload) {
    state.count += payload.increment
  }
}

// 触发
store.commit('increment', {
  increment: 20
})
```

> [深入理解] 这种参数传递方式使得状态变更更加灵活，可以处理各种需要动态值的场景。

## 143. 什么是对象风格的commit方式？

通过传递包含type属性的对象来触发mutation的方式。此时整个对象会作为payload传递给mutation。这种方式提高了代码可读性，也方便管理复杂参数。

对象风格的commit示例：

```javascript
store.commit({
  type: 'increment',
  value: 20
})

// Mutation处理
mutations: {
  increment (state, payload) {
    state.count += payload.value
  }
}
```

## 144. Vuex mutations使用时有哪些注意事项？

由于Vuex基于Vue的响应式系统，需要遵循相同的响应式限制：

1. 初始化state时应预先定义所有需要的字段
2. 添加新属性时必须使用Vue.set或对象扩展语法

示例添加新属性的正确方式：

```javascript
// Vue响应式方式
Vue.set(stateObject, 'newProperty', 'John')

// 或使用对象扩展语法
state.stateObject = { ...state.stateObject, newProperty: 'John' }
```

## 145. 为什么要求mutations必须是同步的？

同步执行保证devtools能准确追踪状态变化。异步操作会破坏调试工具捕获"before"和"after"快照的时机，导致调试异常。

错误示例中的异步mutation：

```javascript
mutations: {
  someMutation (state) {
    api.callAsyncMethod(() => {
      state.count++
    })
  }
}
```

> [原理说明] 异步操作应该通过actions处理，mutation仅负责同步的状态修改。

## 146. 如何实现组件中的mutation触发？

组件中可通过两种方式提交mutation：

- `this.$store.commit('mutationName')`
- 使用mapMutations辅助函数

mapMutations使用示例：

```javascript
import { mapMutations } from 'vuex'

export default {
  methods: {
    ...mapMutations([
      'increment', // this.increment() 映射为 this.$store.commit('increment')
      'incrementBy' // 支持payload: this.incrementBy(amount) → commit('incrementBy', amount)
    ]),
    ...mapMutations({
      add: 'increment' // 别名映射：this.add() → commit('increment')
    })
  }
}
```

## 147. 必须使用常量定义mutation类型吗？

这不是强制的，但采用常量命名是推荐的实践方式，其优势体现在：

- 集中管理所有mutation类型
- 方便工具进行代码检查(linting)
- 提高团队协作效率

示例项目结构：

```javascript
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'

// store.js
import { SOME_MUTATION } from './mutation-types'

mutations: {
  [SOME_MUTATION](state) {
    // 修改state
  }
}
```

> [最佳实践] 当项目规模扩大时，使用常量能显著提高代码的可维护性。

## 148. 如何进行异步操作处理？

Vuex的mutations必须保持同步，异步操作应该通过actions来处理。actions可以包含任意异步操作，最终通过提交mutation来改变状态。

## 149. mutations和actions的核心区别是什么？

两者主要区别有两方面：

1. **职责不同**：mutations直接修改state，而actions通过提交mutation间接修改
2. **执行机制**：mutations必须是同步操作，actions可以包含异步逻辑

> [流程解析] 当需要执行请求等异步操作时，典型流程是：组件dispatch action → action处理异步 → commit mutation → 改变state

## 150. 如何实现日期时间本地化？

通过预定义格式（如short、long等）实现日期时间本地化。按照以下步骤操作：

1. 为不同语言环境定义格式模板：

```javascript
const dateTimeFormats = {
  'en-US': {
    short: {
      year: 'numeric', month: 'short', day: 'numeric'
    },
    long: {
      year: 'numeric', month: 'short', day: 'numeric',
      weekday: 'short', hour: 'numeric', minute: 'numeric'
    }
  },
  'ja-JP': {
    short: {
      year: 'numeric', month: 'short', day: 'numeric'
    },
    long: {
      year: 'numeric', month: 'short', day: 'numeric',
      weekday: 'short', hour: 'numeric', minute: 'numeric', hour12: true
    }
  }
}
```

2. 注册到VueI18n配置：

```javascript
const i18n = new VueI18n({
  dateTimeFormats
})

new Vue({
  i18n
}).$mount('#app')
```

3. 模板中使用日期格式化：

```javascript
<div id="app">
  <p>{{ $d(new Date(), 'short') }}</p>
  <p>{{ $d(new Date(), 'long', 'ja-JP') }}</p>
</div>
```

> [考察内容] 这里主要考察对Vue国际化方案中日期格式化的配置流程掌握。需注意hour12参数对日本时区的特殊处理，以及格式化方法的参数传递方式。

## 151. 如何实现数字货币本地化？

通过自定义数字格式（如currency）实现本地化。具体步骤：

1. 创建不同语言环境的数字格式配置：

```javascript
const numberFormats = {
  'en-US': {
    currency: {
      style: 'currency', currency: 'USD'
    }
  },
  'ja-JP': {
    currency: {
      style: 'currency', currency: 'JPY', currencyDisplay: 'symbol'
    }
  }
}
```

2. 注入VueI18n配置：

```javascript
const i18n = new VueI18n({
  numberFormats
})
```

3. 模板内调用数字格式化方法：

```javascript
<div id="app">
  <p>{{ $n(10, 'currency') }}</p>
  <p>{{ $n(50, 'currency', 'ja-JP') }}</p>
</div>
```

> [实现细节] currencyDisplay参数控制货币符号展示方式，style: 'currency'自动处理货币符号位置，这些配置在不同语言环境下的差异需要特别注意。

## 152. 如何切换应用语言？

通过修改VueI18n实例的locale属性实现全局语言切换。两种核心方式：

全局修改：

```javascript
i18n.locale = 'en' // 切换所有i18n管理的组件
```

组件级修改（使用$i18n属性）：

```javascript
<template>
  <select v-model="$i18n.locale">
    <option v-for="lang in langs" :value="lang">{{ lang }}</option>
  </select>
</template>

<script>
export default {
  data() {
    return { langs: ['de', 'en'] }
  }
}
</script>
```

> [深入理解] 语言切换本质是通过响应式属性触发整个应用的重新渲染，需要确保所有翻译资源已经加载完成。在大型应用中需要配合异步加载策略。

## 153. 什么是翻译懒加载？

翻译资源的按需加载策略，旨在优化应用启动性能。核心实现使用webpack的动态import：

配置i18n实例：

```javascript
export async function loadLanguageAsync(lang) {
  if (!loadedLanguages.includes(lang)) {
    const msgs = await import(`@/lang/${lang}`)
    i18n.setLocaleMessage(lang, msgs.default)
    loadedLanguages.push(lang)
  }
  return setI18nLanguage(lang)
}
```

路由拦截中应用：

```javascript
router.beforeEach((to, from, next) => {
  const lang = to.params.lang
  loadLanguageAsync(lang).then(next)
})
```

> [优化点] webpackChunkName注释生成可预测的chunk名称，有利于浏览器缓存策略。加载后需更新HTML标签的lang属性保持一致性。

## 154. 方法和计算属性的核心区别？

计算属性（computed property）基于响应式依赖进行缓存，只有当依赖变化时才会重新计算。方法每次调用都会执行函数体。

示例对比：

```javascript
// 计算属性
computed: {
  reversedMessage() {
    return this.message.split('').reverse().join('')
  }
}

// 方法
methods: {
  reverseMessage() {
    return this.message.split('').reverse().join('')
  }
}
```

> [性能考量] 高频更新的数据应优先使用计算属性，DOM事件触发的计算适合用方法。缓存机制是Vue性能优化的重要特性之一。

## 155. 什么是Vuetify？

Vuetify是基于Material Design规范实现的Vue UI组件库。核心优势是提供开箱即用的语义化组件系统：

安装配置流程：

```bash
npm install vuetify
```

```javascript
import Vuetify from 'vuetify'
Vue.use(Vuetify)

const vuetify = new Vuetify({
  theme: { dark: true }
})
```

> [框架特性] 包含主题定制、响应式布局、无障碍支持等企业级功能。组件的Material Design实现经过W3C标准验证。

## 156. 如何监听对象深层属性变化？

使用deep watcher检测对象内部变化：

```javascript
vm.$watch('someObject', (newVal) => {
  console.log('对象变化:', newVal)
}, {
  deep: true,
  immediate: true
})
```

> [注意点] 数组变更通过数组方法自动触发更新，无需deep watch。对象监听可能带来性能开销，应根据实际需求谨慎使用。

## 157. 如何让监听器在初始化时立即触发？

可以设置 `immediate: true` 选项使监听器在 Vue 实例（或组件）创建时立即执行回调。例如下面配置会在初始化阶段立即输出当前值：

```javascript
watch: {
  test: {
    immediate: true,
    handler(newVal, oldVal) {
      console.log(newVal, oldVal)
    },
  },
},
```

> [特性说明] 此选项主要用于处理需要在组件加载时立即获取初始值的场景，传统写法中使用默认值可能无法覆盖真实数据状态的同步需求

## 158. comments 选项的作用是什么？

开启 `comments` 选项后可以保留模板中的 HTML 注释并渲染到最终输出中。默认情况下该选项处于关闭状态。通过示例可见其效果：

```javascript
<template>
  <div class="greeting">
    <!--greeting-->
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  comments: true,
  data () {
    return { msg: 'Good morning' }
  }
}
</script>
```

> [注意] 该选项仅在使用包含模板编译器的完整构建版本时生效，且无法在单文件组件（SFC）中使用

## 159. 如何判断当前代码运行在客户端还是服务端？

通过 `vm.$isServer` 属性可以检测当前 Vue 实例是否运行于服务端环境。典型使用方式如下：

```javascript
const Vue = require('vue');
Vue.prototype.$isServer // 全局检测

// 组件内使用
this.$isServer 
```

> [应用场景] 这在服务端渲染（SSR）的场景中尤为重要，用于避免客户端特定代码在服务端执行时引发错误

## 160. 什么是 Vue Router 的导航守卫？

导航守卫用于通过重定向或取消操作来保护导航过程，提供以下三种级别的控制方式：

1. 全局守卫：通过 router.beforeEach 等全局方法注册
2. 路由独享守卫：在路由配置中直接定义
3. 组件内守卫：通过路由组件内的 beforeRouteEnter 等方法实现

> [原理说明] 其本质是路由切换过程中的生命周期钩子，允许在导航的不同阶段插入控制逻辑

## 161. 是否可以在一个计算属性中使用另一个计算属性？

可以直接像访问 data 属性那样引用其他计算属性。下面示例中 comboTwo 复用 comboOne 的计算结果：

```javascript
data() {
  return {
    propOne: 'prop1',
    propTwo: 'prop2'
  }
},
computed: {
  comboOne() { return this.propOne + ',' + this.propTwo },
  comboTwo() { return this.comboOne.split(',').join('-') }
}
```

> [原理分析] Vue 的计算属性依赖追踪系统会自动检测这种引用关系，建立正确的响应式链式更新

## 162. 如何将导入的常量用于模板？

需将常量映射到 data 属性才能被模板正确解析，如下示例展示三种导入常量的使用方式：

```javascript
<span>
  CREATE: {{CREATE_PROP}}
  UPDATE: {{UPDATE_PROP}}
  DELETE: {{DELETE_PROP}}
</span>
<script>
import {CREATE_DATA, UPDATE_DATA, DELETE_DATA} from 'constants';
new Vue({
  data(){
    return {
      CREATE_PROP: CREATE_DATA,
      UPDATE_PROP: UPDATE_DATA,
      DELETE_PROP: DELETE_DATA
    }
  }
})
</script>
```

> [限制说明] 这种必要的数据层包装是 Vue 响应式系统的工作机制决定的，确保依赖追踪的正确性

## 163. 是否推荐在计算属性中使用异步操作？

不推荐使计算属性异步化。计算属性本质应是同步操作，异步逻辑可能导致预期外的行为。例如下面异步用法存在风险：

```javascript
async someComputedProperty () {
  return await someFunction()
}
```

> [解决方案] 确实需要异步计算逻辑时，建议使用 `vue-async-computed` 等专门插件来实现这类特殊需求

## 164. 组合式 API 的优势有哪些？

组合式 API（Composition API）相比传统选项式 API（Options API）具备以下优势：

1. 更好的逻辑复用与代码组织：可以更灵活地抽取和复用逻辑片段
2. 更优的 TypeScript 支持：基于函数的 API 提供更准确的类型推导
3. 更便捷的测试能力：独立逻辑块易于单元测试
4. 更小的生产包体积：通过 tree-shaking 优化移除未使用代码

## 165. 什么是组合函数？

组合函数（Composition Functions）是通过组合式 API 创建的可复用逻辑单元，用函数封装具有相关性的状态与操作方法，实现类似 React Hooks 的逻辑组织方式。

## 166. 什么是 Teleport？

Teleport 是用来将模板内容渲染到 DOM 结构中不同位置的特性，常用于模态框（Modal）、通知框等需要突破父级样式层限制的场景。示例：

```html
<teleport to="body">
  <div v-if="showModal" class="modal"></div>
</teleport>
```

> [实现原理] 通过虚拟 DOM 层面的特殊处理，将子节点挂载到目标 DOM 节点而不影响组件层级关系

## 167. v-html 指令的作用是什么？

`v-html` 用于更新元素的 innerHTML 内容，等效于 DOM 的 innerHTML 属性。对比插值表达式，可以动态渲染 HTML 内容：

```javascript
<div id="app">
  <div>{{ htmlContent }}</div>  // 显示原始文本
  <div v-html="htmlContent"></div>  // 解析 HTML
</div>
```

> [安全警示] 须仅用于可信内容，用户输入的内容可能引发 XSS 攻击。在必须渲染第三方内容时，应使用 sanitize 库进行净化处理
