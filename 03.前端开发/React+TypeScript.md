# React+TypeScript

[TOC]

## 1\. React 中 Element 和 Component 的区别是什么？

React Element 是一个描述你希望在屏幕上看到的 UI 的普通 JavaScript 对象。它可以是 DOM 节点或组件在特定时间点的表示。React Component 是一个函数或一个类，它可选地接受输入（props）并返回一个 React Element（通常通过 JSX）。

## 2\. 为什么在 React 中渲染列表时 `key` 很重要？使用数组索引作为 `key` 有哪些风险？

`key` 帮助 React 识别哪些列表项发生了改变、被添加或被移除。`key` 在其同级元素中必须是唯一的。如果列表项的顺序可能改变，不推荐使用索引作为 `key`，因为这可能对性能产生负面影响，并可能导致组件状态出现问题。当列表重新排序时，若 `key` 是索引，React 可能会混淆元素，导致不正确的状态传递或不必要的重新渲染。

## 3\. 解释表单中受控组件和非受控组件的区别

受控组件是由 React 控制其值的表单输入元素。组件的 state 是表单数据的“唯一数据源”，每当输入值改变时，都会通过 `onChange` 事件处理器更新 state。非受控组件的表单数据由 DOM 本身处理。你需要使用 `ref` 来从 DOM 中获取其当前值。

## 4\. 什么是虚拟 DOM？解释 React 的协调过程（diffing 算法）

虚拟 DOM (VDOM) 是真实 DOM 的内存中表示。当组件状态改变时，React 会在 VDOM 中重新渲染整个 UI。然后，它计算新的 VDOM 与旧的 VDOM 之间的差异（diff）。最后，真实 DOM 只会用实际发生变化的部分进行更新。这个过程称为协调 (reconciliation)。React 实现了一个基于两个假设的启发式 O(n) diffing 算法：1. 不同类型的两个元素将产生不同的树。2. 开发者可以通过 `key` prop 来暗示哪些子元素在不同渲染中可能保持稳定。

## 5\. 什么是高阶组件 (HOC)？

高阶组件 (HOC) 是一个函数，它接受一个组件并返回一个新的组件。这是一种源于 React 组合性质的模式，用于重用组件逻辑。HOC 不应修改输入组件，也不应复制其任何行为；它们应该是纯组合函数。

## 6\. 解释 React Fragments。为什么它们比使用额外的 `<div>` 更可取？

Fragments 允许你将子元素列表分组，而无需向 DOM 添加额外的节点。它们比额外的 `<div>` 更可取，因为：1. Fragments 更快，并且通过不创建额外的 DOM 节点来减少内存使用（这对非常大和深的树有实际好处）。2. 某些 CSS 机制（如 Flexbox 和 Grid）具有特殊的父子关系，在中间添加 `div` 会使保持所需布局变得困难。3. DOM 检查器更整洁。

## 7\. React 中的 Portals 是什么？描述一个典型用例

Portal 提供了一种将子节点渲染到父组件 DOM 层级之外的 DOM 节点中的推荐方式。Portal 的典型用例是当父组件具有 `overflow: hidden` 或影响堆叠上下文的属性（例如 `z-index`），而你需要子组件在视觉上“跳出”其容器时，例如对话框、全局消息通知、悬浮卡片和工具提示。

## 8\. 什么是错误边界 (Error Boundaries)？它们如何工作？

错误边界是 React 组件，它可以捕获其子组件树中任何地方的 JavaScript 错误，记录这些错误，并显示一个备用 UI，而不是让崩溃的组件树显示出来。如果一个类组件定义了 `static getDerivedStateFromError()` 或 `componentDidCatch()` 这两个生命周期方法中的任何一个（或两个），它就成为一个错误边界。

## 9\. 什么是“属性逐层传递”(prop drilling)，如何避免它？

Prop drilling 是指将数据从 React 组件树的一个部分传递到另一个部分时，数据需要通过许多本身并不需要这些数据的中间组件的过程。避免 prop drilling 的常见方法包括使用 React Context API 或状态管理库（如 Redux）。

## 10\. React 应用中常见的性能优化技术有哪些？

1. 使用 `React.memo` (用于函数组件) 或 `PureComponent` (用于类组件) 来防止不必要的重新渲染。
2. 使用 `useMemo` Hook 记忆计算开销大的值。
3. 使用 `useCallback` Hook 记忆回调函数，以防止传递给优化子组件时不必要的渲染。
4. 代码分割：使用 `React.lazy` 和 `Suspense` 按需加载组件。
5. 列表虚拟化 (Windowing)：对于长列表，只渲染可见项，库如 `react-window`。
6. 为列表项提供稳定且唯一的 `key`。

## 11\. React 中的代码分割是什么？如何使用 `React.lazy` 和 `Suspense` 实现？

代码分割是由 Webpack 等打包工具支持的一项功能，它可以创建多个包，这些包可以在运行时动态加载，从而减少应用的初始加载时间。React 通过动态 `import()` 和 `React.lazy` 函数支持代码分割。`React.lazy` 允许你将动态导入的组件作为常规组件进行渲染。`Suspense` 组件用于在懒加载组件加载时显示备用内容（如加载指示器）。

```javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <React.Suspense fallback={<div>Loading...</div>}>
      <OtherComponent />
    </React.Suspense>
  );
}
```

## 12\. React 中的 `StrictMode` 是什么？

`React.StrictMode` 是一个用于在开发模式下突出显示应用程序中潜在问题的工具。它不会渲染任何可见的 UI，而是为其后代组件激活额外的检查和警告。例如，它可以帮助识别具有不安全生命周期的组件、检测意外的副作用等。

## 13\. Hooks 的规则是什么？

使用 Hooks 时需要遵循两条主要规则：

1. **只在顶层调用 Hooks**：不要在循环、条件语句或嵌套函数中调用 Hooks。确保 Hooks 在每次渲染时都以相同的顺序被调用。
2. **只在 React 函数中调用 Hooks**：只能在 React 函数组件或自定义 Hooks 中调用 Hooks。不要在常规 JavaScript 函数中调用。

## 14\. 解释 `useState` 和 `useRef` 的区别和用例

`useState` 用于在函数组件中添加和管理状态；当状态更新时，它会触发组件重新渲染。`useRef` 返回一个可变的 ref 对象，其 `.current` 属性被初始化为传递的参数；更改 `ref.current` 的值不会触发重新渲染。`useRef` 的用例包括访问 DOM 元素或 React 组件实例，以及存储不需要触发重新渲染的可变值（如定时器 ID）。

## 15\. 比较 `useEffect` 和 `useLayoutEffect`

`useEffect` 在浏览器完成布局和绘制之后异步执行，不会阻塞浏览器绘制。它适用于大多数副作用操作，如数据获取和订阅。`useLayoutEffect` 在所有 DOM mutations 完成之后、浏览器进行绘制之前同步执行。它适用于需要同步读取 DOM 布局并可能触发同步重新渲染的操作，以避免用户看到 UI 的中间状态或闪烁。应优先使用 `useEffect`，仅在确实需要同步操作 DOM 布局时才使用 `useLayoutEffect`。

## 16\. `useReducer` Hook 是什么？什么时候比 `useState` 更合适？

`useReducer` 是 `useState` 的替代方案，用于管理更复杂的状态逻辑。它接受一个 reducer 函数 `(state, action) => newState` 和一个初始状态作为参数。当状态逻辑复杂（例如，状态对象包含多个子值，或下一个状态依赖于前一个状态），或者需要在深层嵌套组件中更新状态时，`useReducer` 通常能提供更好的组织性和可维护性。

## 17\. `useCallback` 和 `useMemo` Hooks 如何帮助优化？

`useMemo` 用于记忆化一个计算结果值，只有在其依赖项之一发生改变时才重新计算。`useCallback` 用于记忆化一个函数，只有在其依赖项之一改变时才会返回新的函数实例。这在将回调传递给依赖引用相等性的优化子组件（例如用 `React.memo` 包装的组件）时非常有用，以防止不必要的重新渲染。

## 18\. TypeScript 中的接口 (Interface) 是什么？它们如何定义实体（如对象）的结构？

接口 (Interface) 是 TypeScript 中定义任何实体（如对象）语法契约的一种方式。接口定义了属性、方法和事件，这些是接口的成员。它只包含成员的声明，由派生类负责定义这些成员。接口有助于提供一个标准结构，供派生类遵循。

## 19\. TypeScript 中的泛型 (Generics) 是什么？它们如何用于创建可重用的代码组件？

泛型 (Generics) 是 TypeScript 中的一种工具，能够抽象类型。它们用于创建可重用的代码组件，这些组件可以处理多种数据类型而不是单一的固定类型，同时保持类型安全。

## 20\. React 中的合成事件 (SyntheticEvent) 是什么？

`SyntheticEvent` 是 React 对浏览器原生事件的跨浏览器包装器。它具有与浏览器原生事件（如 `stopPropagation()` 和 `preventDefault()`）相同的接口，但其行为在所有浏览器中都是一致的。

## 21\. React 如何批量处理多个状态更新？React 18 中的自动批量处理是什么？

React 通过将事件处理程序内的多个状态更新组合成一次重新渲染来提高性能，这个过程称为批处理。React 18 引入了自动批量处理，这意味着在 `Promise`、`setTimeout`、原生事件处理程序或任何其他异步操作内部的状态更新也会被默认批处理，从而减少不必要的重新渲染次数。

## 22\. 什么是 React hydration，它在什么上下文（例如 SSR）中使用？

React hydration 是指将服务器端渲染 (SSR) 生成的静态 HTML 与客户端 JavaScript“激活”并使其具有交互性的过程。在 SSR 中，服务器发送 HTML 骨架给浏览器快速显示，然后客户端的 React 代码加载后，通过 hydration 将事件监听器和状态附加到现有的 DOM 结构上，而不是重新创建它们。这用于改善初始加载性能和 SEO。

## 23\. 什么是 JSX？

JSX (JavaScript XML) 是一种 ECMAScript 的语法扩展，它允许在 JavaScript 代码中编写类似 HTML 的模板语法。它为 `React.createElement(type, props, ...children)` 函数提供了语法糖。React 使用 JSX 来描述 UI 应该是什么样子。

## 24\. React 中 props 和 state 的区别是什么？

Props（属性）是父组件传递给子组件的输入数据，子组件不能直接修改其 props（它们是只读的）。State（状态）是组件内部管理的数据，它可以随时间改变（例如响应用户交互），并且当 state 改变时，组件会重新渲染。State 是组件私有的，由组件自身完全控制。
