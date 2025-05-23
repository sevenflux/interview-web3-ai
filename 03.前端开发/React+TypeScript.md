# React+Typescript

[TOC]

## 1. React 组件会经历哪四个生命周期阶段？
>
> [换个问法] 换种方式问：React 组件在生命周期中会发生哪些变化？

组件生命周期机制是 React 的核心价值所在，准确理解组件随时间变化的运行机制对构建可维护应用至关重要。

React 组件经历四个关键阶段：  

1. **初始化（Initialization）** - 用给定的 props 和默认 state 初始化组件  
2. **挂载（Mounting）** - 执行渲染方法生成 JSX 结构并注入 DOM
3. **更新（Updating）** - 当组件 state 变化时触发重新渲染，界面同步更新  
4. **卸载（Unmounting）** - 组件从 DOM 树中移除前的清理阶段  

> [考察内容] 需要清晰阐述各阶段核心任务和触发时机

## 2. 什么是 React？它与其他 JavaScript 框架有何不同？

尽管看起来属于基础问题，但实际考察对 React 与其他竞品框架差异的深入理解，检验候选人对前端技术生态的整体认知。

React 是用于构建用户界面的声明式 JavaScript 库。其核心差异体现在：  

1. **组件化模式** - 通过组合式组件架构构建应用  
2. **虚拟 DOM（Virtual DOM）** - 高效更新界面同时保持性能  
3. **单向数据流** - props 向下传递数据的单向绑定机制  
4. **JSX 语法** - 将标记语言与逻辑代码混合编写  
5. **跨平台能力** - 通过 React Native 实现原生移动端开发  

> [深入理解] 此处需对比 Angular/Vue 等框架的设计哲学差异

## 3. 什么是无状态组件？

> 当被问及"如果 React 组件本质上是生成 UI 的状态机，那无状态组件如何定义"时，需要展示对纯函数组件的理解。

无状态组件本质上是纯函数构成的**可复用组件**，其输出完全由传入的 props 决定：

```jsx
const StatelessCmp = props => {
  return (
    <div className="my-component">
      {props.name}生日: {props.birthday}
    </div>
  );
};

ReactDOM.render(
  <StatelessCmp name="Art" birthday="10/01/1980" />,
  document.getElementById('root')
);
```

> [核心特征] 不需要构造函数或生命周期方法，没有内部 state 管理

## 4. 如何在 React 循环中创建元素？

动态列表渲染是常见需求，需演示数组映射与 key 的使用原则：

```jsx
<div>
  {items.map(item => 
    <ItemComponent 
      key={item.id}  // key 用于优化 diff 算法
      item={item}
    />
  )}
</div>
```

> [实现要点] 采用 JavaScript 的 map 方法遍历数组生成组件列表，key 保证元素稳定性

## 5. 如何更新 React 中的 state 对象？

> 保持 UI 与状态同步是核心机制，应说明 state 更新的正确姿势：

通过 `setState()` 方法异步更新状态：

```jsx
// 正确的方式：传入更新函数
this.setState(prevState => ({
  counter: prevState.counter + 1
}));

// 或传递新状态对象（自动合并） 
this.setState({ username: 'new' });
```

> [原理说明] React 自动合并对象并触发重新渲染，避免直接修改 this.state

## 6. 什么是高阶组件？

高阶组件（HOC）是 React 组合模式的典型实现：

```jsx
const withLogger = WrappedComponent => {
  return class extends React.Component {
    componentDidMount() {
      console.log('Component mounted');
    }
    render() {
      return <WrappedComponent {...this.props} />;
    }
  };
};
```

> [应用场景]  
>
>- 逻辑复用（如鉴权/埋点）  
>- 状态抽象  
>- 渲染劫持  

> [设计模式] 接受组件返回新组件，符合单一职责原则

## 7. 什么是 Render Props？

组件间代码共享技术方案之一：

```jsx
<DataProvider render={data => (
  <h1>Hello {data.target}</h1>
)}/>
```

> [核心机制] 通过 prop 传递渲染函数来动态组合 UI，适用于跨组件数据传递

## 8. React Portals 是什么？

处理脱离 DOM 层级结构的渲染需求：

```jsx
ReactDOM.createPortal(
  <Modal />,
  document.getElementById('modal-root')
);
```

> [使用场景]  

- 模态对话框  
- 通知提醒  
- 悬浮卡片  

> [技术细节] 内容参数可以是任何可渲染元素，容器参数需为已存在的 DOM 节点

## 9. React Profiler 的作用是什么？

性能优化工具链的重要组成部分：

```jsx
<Profiler id="Navigation" onRender={(id, phase, time) => {
  console.log(`${id} 渲染耗时:`, time);
}}>
  <Navigation />
</Profiler>
```

> [核心功能]  

- 测量组件渲染时间  
- 识别性能瓶颈  
- 支持多次提交分析  

> [优化指导] 配合 React Developer Tools 进行可视化性能分析

## 10. React 中的 StrictMode 是什么？
>
> [考察内容] 该问题需要解释该组件的功能、使用场景和优势，并演示基本用法

StrictMode 是用来高亮应用程序中潜在问题的开发工具。虽然以组件形式存在，但不会在 DOM 中创建可见 UI 元素，仅为其包裹的子组件启用额外检查。通过将目标组件嵌套在 `<React.StrictMode>` 中实现功能激活：

```jsx
import React from 'react';

function Application() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <BlogContent /> {/* 仅被包裹的组件接受检查 */}
        </div>
      </React.StrictMode>
    </div>
  );
}
```

主要作用包括：

- 识别具有不安全生命周期（unsafe lifecycle）的组件
- 警告旧版字符串 ref（legacy string ref API）的使用
- 检测废弃的 findDOMNode 方法调用
- 发现意外副作用（unexpected side effects）
- 识别旧版上下文 API（legacy context API）
- 验证可复用状态（reusable state）管理

## 11. React Fragments 的作用是什么？
>
> [应用场景] 此处需要展示如何解决必须包裹 HTML 元素的限制，并提供代码示例

用于在不添加额外 DOM 节点的情况下返回多个同级元素。通过空标签语法 `<></>` 或显式 `<Fragment>` 实现组件多元素返回。例如在表格场景中避免破坏 HTML 结构：

```jsx
function Columns() {
  return (
    <> {/* 使用 Fragment 替代 div 包裹 */}
      <td>Column 1</td>
      <td>Column 2</td>
    </>
  );
}
```

## 12. 调用 setState 时会发生什么？

当调用 setState 时会执行状态对象合并，触发调和（reconciliation）过程。React 会创建新的元素树（React elements tree），通过与旧树比较（diffing）确定需要更新的最小 DOM 操作。整个过程包含三个阶段：

1. 将新状态合并到暂存状态（pending state）
2. 生成虚拟 DOM 对比差异（virtual DOM diff）
3. 批量更新（batch update）实际 DOM

## 13. React 元素（Element）和组件（Component）有何区别？
>
> [概念辨析] 要强调元素是 UI 的描述对象，组件是生成元素的逻辑单元

- **元素（Element）**：轻量不可变对象，描述屏幕显示内容的虚拟节点，包含 type/props 等属性。由 JSX 转换生成，例如 `<Button />` 编译为 `React.createElement()` 调用
- **组件（Component）**：可复用代码单元，通过函数或类形式定义。接收 props 作为输入，返回描述 UI 的元素树。例如函数组件直接返回 JSX，类组件通过 render 方法输出

## 14. 何时应选择类组件（Class Component）而非函数组件（Functional Component）？

在以下场景需要考虑类组件：

- 需要管理组件内部状态（state）
- 使用生命周期方法（lifecycle methods）
- 需要错误边界（error boundaries）实现
*但现在可通过 Hooks（如 useState/useEffect）在函数组件中实现相同功能

## 15. React 中如何创建组件？
>
> [代码示例] 需要演示两种组件的定义方式

React 支持两种组件创建方式：

**函数式组件**（使用箭头函数或普通函数）：

```jsx
function Welcome({ message }) {
  return <h1>{`Hello, ${message}`}</h1>
}
```

**类组件**（继承 React.Component）：

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>{`Hello, ${this.props.message}`}</h1>
  }
}
```

主要区别在于类组件支持 state 管理和生命周期方法（Hooks 出现前）

## 16. React 中的 props 是什么？
>
> [数据传递] 演示父子组件间如何传递数据

props（properties 缩写）是组件接收的只读输入数据。通过组件标签属性传递，支持各类型数据包括对象：

```jsx
<Profile userName="Alice" avatar={<Avatar />} />
```

函数组件通过参数接收，类组件通过 `this.props` 访问：

```jsx
// 函数组件
function Profile(props) {
  return <div>{props.userName}</div>
}

// 类组件
class Profile extends React.Component {
  render() {
    return <div>{this.props.avatar}</div>
  }
}
```

## 17. React 中的 state 是什么？

组件内部维护的可变数据存储，当状态变更时会触发重新渲染。与 props 的关键区别：

- **作用域**：state 是局部且封装的，只能由其所属组件修改
- **更新方式**：必须通过 setState() 方法更新（类组件）
- **生命周期**：与组件实例绑定，随组件卸载销毁

## 18. React 中的 context 是什么？
>
> [应用场景] 需说明适合跨层级数据共享的场景如主题、语言偏好等**

上下文（context）提供组件树跨层级数据共享的方案，避免逐层传递 props。使用三步模式：

1. `React.createContext()` 创建 Context 对象
2. `<MyContext.Provider>` 提供数据
3. `useContext()` Hook 或 `<MyContext.Consumer>` 消费数据

适用于全局数据如：

- 用户认证信息
- UI 主题配置
- 本地化语言设置

## 19. 如何实现条件渲染？
>
> [实现方式] 重点讲解短路运算和三元表达式的使用

利用 JSX 特性实现条件逻辑：

1. **逻辑与短路（&&）**：

```jsx
{hasNotification && <Notification />} 
```

2. **三元表达式**：

```jsx
{isLoading ? <Spinner /> : <Content />}
```

3. **辅助变量**：

```jsx
let message;
if (error) message = <Error />;
return <div>{message}</div>
```

注意空值（null/false/undefined）不会被渲染，可灵活用于动态控制 UI 元素显示

## 20. 如何在 JSX 回调中绑定方法或事件处理程序？

处理事件是前端开发中的常见需求，React 提供了三种主要绑定方式：

**箭头函数绑定**：直接在 JSX 中使用箭头函数包裹方法调用  

```jsx
class Greeting extends React.Component {
  handleClick() {
    console.log('Clicked');
  }
  render() {
    return <button onClick={() => this.handleClick()}>Click Me</button>;
  }
}
```

**构造函数绑定**：手动绑定方法的 this 上下文  

```jsx
class Greeting extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() { /* ... */ }
  render() {
    return <button onClick={this.handleClick}>Click Me</button>;
  }
}
```

**类属性语法**：使用箭头函数自动绑定 this  

```jsx
class Greeting extends React.Component {
  handleClick = () => { // 自动绑定 this
    console.log('Clicked');
  }
  render() {
    return <button onClick={this.handleClick}>Click Me</button>;
  }
}
```

> [考察点] 理解不同绑定方式的 this 上下文处理差异及其性能影响。箭头函数方式可能导致不必要的重新渲染，而构造函数绑定和类属性语法能避免这个问题。

## 21. 如何条件式应用 CSS 类属性？

通过 JavaScript 表达式动态组合类名字符串：  

```jsx
<div className={'btn-panel ' + (this.props.disabled ? 'disabled' : 'default')}>
```

更现代的写法可使用模板字符串或第三方库（如 classnames）：  

```jsx
// 模板字符串
<div className={`base-class ${isActive && 'active'}`}>

// classnames 库
import cn from 'classnames';
<div className={cn('base', { 'active': isActive })}>
```

> [深入理解] 这里重点考察条件语句的应用能力和对 CSS-in-JS 方案的熟悉程度。直接字符串拼接可能产生多余空格，使用专用库能更安全地处理类名组合。

## 22. 什么是 React Refs？为什么它们很重要？

Refs 提供了直接访问 DOM 元素或组件实例的能力，典型使用场景包括：  

- 表单焦点管理  
- 触发强制动画  
- 集成第三方 DOM 库  

使用 createRef API 的示例：  

```jsx
class MyComponent extends React.Component {
  inputRef = React.createRef();
  
  focusInput = () => {
    this.inputRef.current.focus();
  }

  render() {
    return <input ref={this.inputRef} />;
  }
}
```

> [注意点] 函数组件需要使用 useRef Hook，且不应过度使用 refs（优先考虑声明式数据流）。在表单处理中，受控组件通常是更好的选择。

## 23. 什么是 React Keys？为什么它们很重要？

Keys 帮助 React 识别列表元素的变更，需满足：  

1. **唯一性**：兄弟元素间唯一（非全局唯一）  
2. **稳定性**：不随渲染改变  

正确使用示例：  

```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
}
```

> [核心原理] 在协调算法（reconciliation）中，Keys 用于元素追踪。无 Key 时列表重排会导致性能下降和状态错位。例如若列表项包含复选框状态，错误的 Key 会导致复选框状态与错误内容关联。

## 24. 受控组件与非受控组件有何区别？  

| 特征                | 受控组件                     | 非受控组件          |
|---|---|---|
| 数据存储位置        | 组件 state                  | DOM 节点         |
| 更新方式            | 通过事件处理程序            | 直接操作 DOM      |
| 校验时机            | 即时验证                    | 提交时验证        |
| React 最佳实践      | ✔️ 推荐                    | △ 特定场景使用   |

**受控组件实现**：  

```jsx
class ControlledForm extends React.Component {
  state = { username: '' };
  
  handleChange = (e) => {
    this.setState({ username: e.target.value });
  }

  render() {
    return <input value={this.state.username} onChange={this.handleChange} />;
  }
}
```

**非受控组件实现**：  

```jsx
class UncontrolledForm extends React.Component {
  inputRef = React.createRef();
  
  handleSubmit = () => {
    console.log(this.inputRef.current.value);
  }

  render() {
    return <input ref={this.inputRef} />;
  }
}
```

> [设计思想] 考查对 React 声明式编程模型的理解。受控组件遵循单向数据流原则，使表单数据始终与 React state 同步，便于实现复杂交互逻辑。

## 25. 在 Class 组件中应该在哪个生命周期方法发起 AJAX 请求？

应当使用 `componentDidMount` 生命周期方法发起 AJAX 请求。这样做的核心原因是确保组件已经挂载完成：如果在挂载前请求提前完成，会尝试在未挂载组件上调用 `setState`，这会引发错误和 React 警告。`componentDidMount` 提供了安全触发状态更新的执行环境。

> [补充说明] 此处需要展示对 React 组件生命周期的理解，关键点在于异步请求与组件状态更新时机的匹配

## 26. shouldComponentUpdate 的作用及其重要性是什么？

该生命周期方法用于控制组件是否参与重新渲染流程，默认返回 `true`。当返回 `false` 时，React 会跳过当前组件及其子组件的协调过程。其重要性在于优化性能：通过阻断无须更新的组件树遍历，减少虚拟 DOM 的对比开销。在复杂组件层级结构中可以显著提升渲染效率。

> [技术背景] React 的调和（Reconciliation）机制会遍历所有可能变更的组件树分支，而该方法为开发者提供了主动干预这一过程的能力

## 27. 如何设置 React 生产环境构建？会产生哪些影响？

通过设置 `process.env.NODE_ENV` 为 `production` 启用生产模式。此模式会移除开发环境中存在的调试警告、性能检测工具等开发专用代码，显著减小 bundle 体积并提升运行时性能。现代打包工具（如 Webpack）通常会在生产构建时自动配置该环境变量。

> [开发实践提示] 可通过 Chrome React Developer Tools 的图标颜色（深色为生产模式）快速验证当前环境

## 28. React 事件处理机制是如何工作的？

React 使用合成事件（SyntheticEvent）系统实现跨浏览器事件处理。其核心机制包含两个要点：

1. **事件委派**：通过顶层单事件监听实现，所有事件首先由根容器捕获后分发至具体组件
2. **统一接口**：`SyntheticEvent` 标准化了不同浏览器的事件对象接口

```javascript
function handleClick(e) {
  e.preventDefault(); // 使用与原生事件一致的 API
  console.log('Event type:', e.type);
}
```

> [性能考量] 此设计减少了内存消耗，避免了频繁绑定/解绑事件监听器的开销，特别适合动态组件场景

## 29. createElement 与 cloneElement 的核心区别是什么？

- **createElement**：JSX 编译后的基础元素创建方法，生成描述 UI 结构的普通对象（React Element）
- **cloneElement**：用于克隆现有元素并合并/覆盖其 props 的工具方法

```javascript
// 典型 cloneElement 用例
React.Children.map(children, child => 
  React.cloneElement(child, { newProp: value })
)
```

> [设计理念] 命名直观反映功能差异：create 是初始创建，clone 是带有属性修改的复制操作

## 30. setState 方法的第二个可选参数是什么？其作用如何？

该参数是状态更新完成后的回调函数。由于 `setState` 的异步特性，当需要确保在状态应用后再执行某些操作（如基于新状态的 DOM 测量）时，可使用此回调：

```javascript
this.setState(
  { count: prevCount + 1 },
  () => console.log('Updated count:', this.state.count)
);
```

> [最佳实践警告] 更推荐使用 componentDidUpdate 来处理此类逻辑，避免过度依赖回调函数导致流程混乱

## 31. 什么是状态突变？如何有效预防？

状态突变指绕过 `setState` 直接修改状态对象的内存引用。典型错误场景包括：

```javascript
// 错误示例：直接修改嵌套对象
this.state.user.profile.name = 'newName'; 

// 正确做法：创建新对象
this.setState({
  user: {
    ...this.state.user,
    profile: {...this.state.user.profile, name: 'newName'}
  }
});
```

预防策略：

1. 应用不可变数据结构（如 Immutable.js）
2. 在 Redux 等状态管理中强制 reducer 返回新对象
3. 使用严格模式（Strict Mode）检测意外修改

> [框架机制] React 的浅比较依赖对象引用变化来判断是否需要更新组件

## 32. 单元测试中浅渲染的优缺点分析？

**优点**：

- 测试执行速度显著提升，避免深层组件树的渲染开销
- 确保测试关注点隔离，符合单元测试单一职责原则

**缺点**：

- 无法检验跨组件的交互问题（如子组件异常导致父组件渲染失败）
- 与真实 DOM 环境的差异可能掩盖样式相关缺陷

> [平衡建议] 采用分层测试策略：基础组件使用浅渲染，集成测试使用完整渲染

## 33. 如何诊断优化 React 应用的渲染性能问题？

**诊断步骤**：

1. 使用 React DevTools 中的 Profiler 录制组件渲染时序
2. 筛选高耗时且高频渲染的组件
3. 检查是否开启 Strict Mode 的重复渲染检测

**优化手段**：

- 对函数组件应用 `React.memo`，配合自定义比较函数
- Class 组件继承 `PureComponent`
- 复杂状态结构使用不可变更新

```javascript
// 优化示例：避免内联对象导致的无效重新渲染
const MemoizedList = React.memo(
  ({ items }) => <List data={items} />,
  (prev, next) => _.isEqual(prev.items, next.items)
);
```

> [性能陷阱] 不恰当的浅比较可能增加计算开销，推荐通过 Profiler 确认优化收益后决定是否应用

## 34. 什么是虚拟DOM（VDOM）？React如何利用它进行DOM渲染？

虚拟DOM（Virtual DOM）是一个编程概念，构成React架构的核心部分。它作为DOM的轻量级抽象，代替直接操作真实DOM。当状态变化时，React会先在虚拟DOM上执行渲染，生成新的虚拟DOM树。

> [深入理解] 这里主要考察对渲染优化机制的理解。React通过批处理改动，生成当前虚拟DOM与上一次状态的差异（diff），仅将差异应用到真实DOM上。这种机制避免了频繁的DOM操作，提升了性能。

通过比较新旧虚拟DOM树的差异（diffing算法），计算出最小化更新操作。此过程使得开发者可以用声明式编程范式编写界面，而React在底层处理复杂的DOM更新策略。关键优势体现在复杂UI场景下保持高效渲染，同时简化开发心智模型。

## 35. 什么是prop drilling？如何避免？

当深层嵌套组件需要使用高层组件的数据时，prop drilling指数据通过多层中间组件逐级传递的现象。例如`<EditUsersPage>`的状态`selectedUserAddress`需要传递到`<UserAddress>`，中间经过`<User>`和`<UserDetails>`组件。

> [解决方案] 使用React Context API可避免该问题。通过创建Provider组件包裹数据源，深层组件通过`useContext`直接消费数据。另一个典型方案是引入状态管理库（如Redux），但Context更适合局部状态共享。[深入理解] 中间组件被迫传递不相关props会导致代码冗余和维护困难，破坏组件封装性。

## 36. React（库）与Angular（框架）的核心架构差异是什么？这对项目技术选型有何影响？

核心差异在于架构完整度。React定位为UI渲染库，需配合Redux等第三方库构建完整应用。Angular提供全功能框架，内置路由、状态管理等解决方案。

> [选型指南] React生态灵活适合长期演进的复杂项目，允许选择最新社区方案。Angular开箱即用的特性适合需要快速启动和强规范约束的团队。[考察重点] 这里需要对比库和框架的设计哲学对开发流程的影响。

## 37. React组件命名有哪些例外情况？

通常组件名需大写字母开头，但以下情况例外：

```jsx
// 通过对象属性访问的小写组件名仍有效
<obj.component /> // 编译为React.createElement(obj.component)
```

> [技术细节] JSX编译器将带点的小写标签识别为合法组件引用。这种模式常见于动态组件场景，例如从配置对象中读取组件。

## 38. 常用的React动画库有哪些？

主流选择包括_React Transition Group_和_React Motion_。前者提供基础过渡动画控制，后者基于物理模型实现更复杂的动画效果。

## 39. 什么是React Router？

React Router是基于React的声明式路由库，核心功能包括：

- 保持UI与URL同步
- 支持嵌套路由配置
- 提供编程式导航API

```jsx
// 基础使用示例
import { BrowserRouter as Router, Route } from 'react-router-dom'

<Router>
  <Route path="/about" component={About} />
</Router>
```

## 40. React Router v6中的Router组件有哪些？

v6版本提供四类Router组件：

1. `<BrowserRouter>`: 使用HTML5 History API（pushState）
2. `<HashRouter>`: 基于URL hash的路由方案
3. `<MemoryRouter>`: 内存路由，适合测试和非浏览器环境
4. `<StaticRouter>`: 服务端渲染专用路由

> [技术原理] 这些组件创建不同类型的history实例，通过context传递导航信息。v6版本简化了路由配置逻辑，推荐函数式组件写法。

## 41. history对象中push()和replace()方法的作用？

- `push()`: 添加新条目到历史记录（浏览器后退按钮可返回）
- `replace()`: 替换当前历史记录条目（无法通过后退返回）

例如提交表单后使用`replace()`防止重复提交：

```javascript
history.push('/dashboard') // 增加新记录
history.replace('/login') // 替换当前记录
```

## 42. 如何实现404页面？

在React Router v5及之前版本使用`<Switch>`：

```jsx
<Switch>
  <Route exact path="/" component={Home} />
  <Route path="/user" component={User} />
  <Route component={NotFound} /> {/* 无path属性作为兜底路由 */}
</Switch>
```

v6版本改用`<Routes>`和`path="*"`：

```jsx
<Routes>
  <Route path="/" element={<Home />} />
  <Route path="/user" element={<User />} />
  <Route path="*" element={<NotFound />} />
</Routes>
```

## 43. 哪些场景下错误边界无法捕获异常？

错误边界不捕获以下情况的错误：

1. 事件处理器内的错误（需手动try/catch）
2. 异步代码（setTimeout、Promise回调）
3. 服务端渲染期间
4. 错误边界组件自身抛出的错误

```jsx
class ErrorBoundary extends React.Component {
  componentDidCatch(error) {
    // 无法捕获子组件的异步错误
  }
}
```

## 44. Next.js是什么？它的核心特性有哪些？

Next.js是基于React的轻量级框架，核心功能包含：

1. 开箱即用的服务端渲染（SSR）
2. 自动代码分割优化加载性能
3. 基于文件系统的路由机制
4. 支持热模块替换（HMR）的开发环境
5. 灵活的部署选项（支持Serverless/Node.js服务器）

> [使用场景] 适合需要SEO优化的营销页面、需要服务端逻辑的复杂应用等。内置API路由功能可构建全栈应用。

## 45. 如何防止函数被高频调用？

常用三种节流/防抖方案：

1. **节流（Throttle）**：固定时间间隔执行

```javascript
import throttle from 'lodash.throttle'
const handleScroll = throttle(() => {...}, 1000)
```

2. **防抖（Debounce）**：等待操作停止后执行

```javascript
import debounce from 'lodash.debounce' 
const handleResize = debounce(() => {...}, 300)
```

3. **requestAnimationFrame节流**：匹配浏览器重绘周期

```javascript
import rafSchd from 'raf-schd'
const handleAnimation = rafSchd((scrollValue) => {...})
```

> [性能考量] 高频事件（如滚动、调整尺寸）需合理选用控制策略，避免过度渲染导致性能问题。

## 46. JSX 如何防止注入攻击？

React DOM 在渲染之前会对所有嵌入 JSX 的值进行转义处理。这种方式确保应用中无法注入任何非明确编写的代码——所有内容在渲染前都会被转换为字符串。

举个例子，用户输入可以安全嵌入如下：

```javascript
const name = response.potentiallyMaliciousInput;
const element = <h1>{name}</h1>;
```

通过这种机制，能够有效预防 XSS（跨站脚本）攻击。

> [深入理解] 该问题主要考察对 React 安全机制的理解，关键点在于解释转义处理如何截断潜在攻击代码的执行路径。

## 47. 表单处理的主流选择是什么？

**Formik** 是 React 表单处理库，提供验证机制、访问字段追踪和提交处理等解决方案。其主要功能可分为：

1. 表单状态数据存取
2. 错误校验与提示信息
3. 提交事件处理

该库通过精简 API 解决复杂表单场景的常见痛点，注重扩展性与性能平衡。

> [考察内容] 此处需要展示对 React 生态中表单解决方案的熟悉程度，重点突出 Formik 的三层能力结构。

## 48. Formik 相比 Redux Form 有何优势？

推荐 Formik 的三大核心原因：

- **状态管理合理性**：表单状态本质是临时且局部的，引入 Redux 等全局状态管理反而增加复杂度
- **性能损耗显著**：Redux Form 在每次输入时触发整个 Redux 树的更新，对大型应用造成输入延迟
- **体积优势明显**：Redux Form 压缩后体积 22.5KB，而 Formik 仅 12.7KB

> [对比要点] 通过状态管理模型、性能影响和资源体积三个维度进行技术选型分析。

## 49. 为何不推荐使用继承机制？

React 提倡通过组合（composition）而非继承（inheritance）实现组件代码复用。组件属性（Props）和组合模式能够安全、清晰地控制组件的表现与行为逻辑。

对于非 UI 逻辑复用，推荐抽取为独立 JavaScript 模块，通过导入使用而非继承扩展。这种方式既保持组件独立性，又提升可维护性。

## 50. 推荐哪些数组操作进行状态更新？

状态更新时建议采用以下安全操作方式：

| 操作类型   | 推荐方式                 | 避免方式                 |
|---|---|---|
| **添加**   | `concat`, `[...arr]`     | `push`, `unshift`        |
| **删除**   | `filter`, `slice`        | `pop`, `shift`, `splice` |
| **替换**   | `map`                    | `splice`, 直接索引赋值   |
| **排序**   | 创建新数组副本           | `reverse`, `sort`        |

使用 Immer 库可突破这些限制，安全使用任意数组方法。

> [注意要点] 直接修改原数组会破坏 React 的状态不可变原则，导致渲染问题。

## 51. 如何定义嵌套函数组件？

虽然技术上允许函数组件嵌套定义，但实践中极其不推荐。这种做法可能导致：

- 组件在每次父级渲染时重新创建，引发性能损耗
- 状态与生命周期管理混乱
- 热重载（HMR）等开发工具行为异常

## 52. key 能用于非列表元素吗？

虽然 key 主要用于列表渲染优化，但实际可用于任何需要明确区分组件实例的场景。当未显式指定 key 时，React 默认使用组件在父级中的顺序索引，这在组件顺序可能变化时会导致渲染异常。

示例场景：

```javascript
<Modal>
  {isLogin ? <Login key="login" /> : <Signup key="signup" />}
</Modal>
```

## 53. getDerivedStateFromError 的作用是什么？

该静态生命周期方法用于错误边界（Error Boundary）处理，当子组件抛出错误时被触发。它接收错误对象作为参数，并返回新的状态对象用于更新 UI。

典型实现模式：

```javascript
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError(error) {
    return { hasError: true }; // 更新状态显示降级UI
  }

  render() {
    return this.state.hasError 
      ? <h1>系统异常</h1>
      : this.props.children;
  }
}
```

> [重要说明] 此方法仅用于渲染降级 UI，副作用操作应放在 componentDidCatch 中。

## 54. 组件重渲染时的生命周期顺序？

更新触发的生命周期调用顺序为：

1. `static getDerivedStateFromProps()`: 基于新 props 计算状态
2. `shouldComponentUpdate()`: 返回 false 可阻断渲染
3. `render()`: 生成虚拟 DOM
4. `getSnapshotBeforeUpdate()`: 获取 DOM 更新前的快照
5. `componentDidUpdate()`: 执行副作用操作

## 55. 错误处理会触发哪些方法？

组件树抛出错误时依次触发：

1. `static getDerivedStateFromError()`: 更新状态以显示错误 UI
2. `componentDidCatch()`: 记录错误信息，执行副作用

这两个方法构成错误边界组件的核心机制。

## 56. unmountComponentAtNode 的用途？

该 DOM 方法用于卸载指定容器内的 React 组件树，执行清理操作包括：

- 移除事件监听器
- 清除组件状态
- 触发卸载生命周期方法

方法签名示例：

```javascript
import ReactDOM from 'react-dom';

const success = ReactDOM.unmountComponentAtNode(container);
// 返回布尔值表示卸载操作是否执行
```

## 57. 如何阻止组件渲染？

通过条件判断返回 `null` 可跳过组件渲染逻辑：

```javascript
function ConditionalRender({ visible }) {
  if (!visible) return null;
  
  return <div>动态内容</div>;
}
```

在父组件中控制：

```javascript
class Parent extends React.Component {
  state = { show: false };
  
  toggle = () => this.setState(p => ({ show: !p.show }));
  
  render() {
    return (
      <div>
        <button onClick={this.toggle}>切换</button>
        <ConditionalRender visible={this.state.show} />
      </div>
    );
  }
}
```

> [实现原理] 返回 null 时 React 会跳过该组件的 DOM 挂载与更新，提升渲染性能。

## 58. 举例说明如何使用Context（上下文）？

Context（上下文）用于在React组件树中共享全局数据。比如下面的例子手动传递"theme"属性来样式化按钮组件：

```javascript
// 创建默认主题为"luna"的Context
const ThemeContext = React.createContext("luna");
// App组件用Provider传递主题值
class App extends React.Component {
  render() {
    return (
      <ThemeContext.Provider value="nova">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}
// 中间组件无需传递theme属性
function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}
// 在按钮组件中读取主题值
class ThemedButton extends React.Component {
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

> [深入理解] 这里展示通过Provider向下传递主题值，中间组件无需逐层传递props的特性。注意类组件通过contextType访问上下文的设计模式。

## 59. 如何使用contextType属性？

contextType用于消费上下文对象，两种主要用法：

1. **类属性方式**：
   将React.createContext()创建的上下文对象赋值给类的contextType属性，即可在生命周期方法和render函数中通过this.context访问最近值：

```javascript
class MyClass extends React.Component {
  componentDidMount() {
    const value = this.context; // 使用上下文值
  }
  render() {
    return <div>{this.context}</div>;
  }
}
MyClass.contextType = MyContext; // 显式赋值
```

2. **静态字段语法**：
   使用ES6公共类字段语法直接初始化：

```javascript
class MyClass extends React.Component {
  static contextType = MyContext; // 直接声明
  render() {
    return <div>{this.context}</div>;
  }
}
```

> [考察内容] 重点理解在类组件中两种访问上下文的标准化方法，注意静态字段语法更符合现代React开发模式。

## 60. 什么是Consumer？

Consumer是订阅上下文变化的React组件，要求接收函数作为子元素。这个函数接收当前上下文值并返回React节点：

```javascript
<MyContext.Consumer>
  {value => (
    <div>当前主题：{value.theme}</div>
  )}
</MyContext.Consumer>
```

> [需要解释] Consumer主要用于函数组件中访问上下文，或在单个组件需要消费多个上下文时使用。其工作机制是寻找最近的Provider节点的value属性。

## 61. 如何解决使用Context时的性能问题？

当Provider父组件重新渲染时，因value属性总是生成新对象会导致Consumer非必要重渲染：

```javascript
// 错误示例：每次渲染产生新对象
<Provider value={{ something: 'something' }}>
```

解决方案是将value提升到父级state中维护：

```javascript
class App extends React.Component {
  state = { value: { something: 'something' } };
  
  render() {
    return (
      <Provider value={this.state.value}>
        <Toolbar />
      </Provider>
    );
  }
}
```

> [性能优化] 通过保证value对象的引用稳定性，避免因Provider重渲染导致下游Consumer的无意义更新。必要时可使用memoization技术优化对象生成。

## 62. HOC中forwardRef的作用是什么？

在HOC中ref不会透传（因ref不是prop属性）。使用React.forwardRef API可显式转发ref到内部组件：

```javascript
function logProps(Component) {
  class LogProps extends React.Component {
    render() {
      const { forwardedRef, ...rest } = this.props;
      return <Component ref={forwardedRef} {...rest} />;
    }
  }

  return React.forwardRef((props, ref) => (
    <LogProps {...props} forwardedRef={ref} />
  ));
}

// 使用示例
const FancyButton = logProps(class extends React.Component {
  focus() { /* ... */ }
});

// ref直接指向内部按钮组件
const ref = React.createRef();
<FancyButton ref={ref} />;
```

> [关键点] forwardRef通过将ref作为特殊参数传递，解决了HOC组件ref指向外层容器而非实际子组件的问题。这在暴露组件DOM方法时尤为重要。

## 63. ref参数是否对所有函数/类组件可用？

常规函数组件或类组件不会接收ref参数，ref也不会出现在props中。只有通过React.forwardRef定义的组件才拥有第二个ref参数：

```javascript
// 普通函数组件无法接受ref
function MyComponent() {
  return <div />;
}

// 只有forwardRef组件才能接收
const Forwarded = React.forwardRef((props, ref) => (
  <MyComponent ref={ref} />
));
```

## 64. 使用forwardRef时为何要特别注意组件库开发？

在组件库中使用forwardRef时需视为重大变更并发布主版本更新，因为这可能破坏现有用户的使用方式：

1. ref指向位置改变影响用户获取DOM节点的方式
2. 组件类型定义变化可能导致类型检查失败
3. 可能影响其他HOC组成的复合组件

> [最佳实践] 组件库升级forwardRef应遵循语义化版本控制，提供迁移指南并建议用户测试ref相关功能。

## 65. 如何在不使用 ES6 的情况下创建 React 类组件？

需要采用 create-react-class 模块来实现。定义默认属性时需在对象中编写 getDefaultProps() 方法，初始化状态则通过 getInitialState() 方法返回初始值。

```javascript
var Greeting = createReactClass({
  getDefaultProps: function () {
    return {
      name: "Jhohn",
    };
  },
  getInitialState: function () {
    return { message: this.props.message };
  },
  handleClick: function () {
    console.log(this.state.message);
  },
  render: function () {
    return <h1>Hello, {this.props.name}</h1>;
  },
});
```

> [特性说明] createReactClass 方法中的所有方法会自动绑定 this 上下文，因此无需在构造函数中手动调用 .bind(this) 处理事件处理器

## 66. 什么是 TypeScript？

由微软开发的开源编程语言，作为 JavaScript 的超集（superset）添加了可选静态类型检查(class/interface 等特性。其代码会被编译成可在任意浏览器运行的干净 JavaScript，同时支持静态检查(staging checking)和代码重构(code refactoring)等高效开发工具实践。

核心差异点：

- JavaScript 不支持 ES6 全特性，TypeScript 支持更完整
- 重用组件时 JS 采用函数(function)和原型继承(prototype-based inheritance)，TS 支持面向对象的类(class)
- JS 缺少接口(interface)和静态类型检查(static typing)，TS 这两项均为核心能力
- TS 支持可选参数(optional parameter)，JS 不具备该特性

## 67. TypeScript 中的泛型是什么？

泛型(Generics)是用于创建可与多种类型协作的复用代码组件的类型抽象工具。其核心价值在于定义组件时不预先指定具体类型，而是在使用时动态指定：

```typescript
function identity<T>(arg: T): T {
  return arg;
}
let output = identity<string>("hello"); // 显式指定类型参数
let output2 = identity("hello"); // 类型推断自动判断
```

> [应用场景] 常用于容器类结构(如数组)或通用工具函数的类型约束

## 68. TypeScript 的内置（原始）类型有哪些？

五种主要原始类型(primitive types)：

1. **number** - 双精度浮点数

   ```typescript
   let decimal: number = 6;
   ```

2. **string** - UTF-16 字符串

   ```typescript
   let color: string = "blue";
   ```

3. **boolean** - 逻辑值 true/false

   ```typescript
   let isDone: boolean = false;
   ```

4. **null** - 空值引用

   ```typescript
   let n: null = null;
   ```

5. **undefined** - 未定义值（所有类型的子类型）

   ```typescript
   let u: undefined = undefined;
   ```

## 69. TypeScript 中的模块是什么？

通过模块(modules)实现跨文件代码组织的能力。模块具有独立作用域，导出的成员才能被外部访问：

```typescript
// shapes.ts
export namespace Shapes { // 命名空间导出
  export class Circle {
    constructor(public radius: number) {}
  }
}

// app.ts
import { Shapes } from "./shapes";
let circle = new Shapes.Circle(10);
```

> [最佳实践] 模块编译时可配置模块加载方式(commonjs/amd 等)，配合 Webpack 等工具管理依赖

## 70. TypeScript 中的接口是什么？

接口(interface)是定义对象结构的形式契约，仅声明成员不提供实现：

```typescript
interface LabelledValue {
  label: string;
  size?: number;  // 可选属性
  readonly id: number; // 只读属性
}

function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}
```

> [运行时特性] 接口不会生成实际 JavaScript 代码，仅在编译阶段进行类型校验

## 71. 如何将多个 TypeScript 文件编译为单个文件？

通过 tsc 命令的 --outFile 参数合并编译结果：

```bash
tsc --outFile output.js file1.ts file2.ts file3.ts
```

> [注意] 该方式适用于小规模项目，大型项目建议使用模块打包工具(Webpack/Rollup)

## 72. 如何在 TS 子类中调用基类构造方法？

通过 super() 方法显式调用父类构造函数：

```typescript
class Animal {
  constructor(public name: string) {}
}

class Snake extends Animal {
  constructor(name: string) {
    super(name); // 必须首先调用
    this.move();
  }
}
```

## 73. TypeScript 中的命名空间是什么？如何声明？

命名空间(namespace)用于组织相关代码的逻辑容器（旧称内部模块）：

```typescript
namespace Validation {
  export interface StringValidator {
    isAcceptable(s: string): boolean;
  }

  const lettersRegexp = /^[A-Za-z]+$/;
  export class LettersOnlyValidator implements StringValidator {
    isAcceptable(s: string) {
      return lettersRegexp.test(s);
    }
  }
}
```

> [现代方案] 推荐使用 ES6 模块替代命名空间进行代码组织

## 74. TypeScript 中的鸭子类型是什么？

鸭子类型(Duck Typing)通过对象结构而非类型名判断类型兼容性。当对象具有相同结构时被认为是相同类型：

```typescript
interface Point {
  x: number;
  y: number;
}

function logPoint(p: Point) {
  console.log(`${p.x}, ${p.y}`);
}

const point = { x: 12, y: 26, z: 89 }; // 包含 x/y 属性即可
logPoint(point); // 参数校验通过
```

> [类型规则] 只需匹配接口的必要属性，额外属性不会导致类型不兼容
