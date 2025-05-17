# React Native 面试知识

[TOC]

## 1\. 什么是 React Native？

React Native 是一个由 Facebook 开发的开源框架，用于使用 JavaScript 和 React 构建原生移动应用程序,默认使用Hermes引擎。它允许开发者利用现有的 Web 开发技能，从单一代码库创建在 iOS 和 Android 平台上运行的、具有原生外观和体验的应用。React Native 使用与原生平台相同的 UI 构建块，并通过桥接机制调用原生模块。

## 2\. React Native 的主要特性有哪些？

* **跨平台开发**：编写一次代码，部署到 iOS 和 Android。
* **原生 UI 组件**：使用原生的 UI 元素构建界面，提供原生体验。
* **JavaScript 和 React**：利用流行的技术栈，降低 Web 开发者学习曲线。
* **快速刷新（Fast Refresh）**：即时预览代码更改，加快开发节奏。
* **庞大且活跃的社区**：拥有丰富的资源、库和社区支持。
* **丰富的第三方插件**：提供众多功能模块，扩展开发能力。

## 3\. 使用 React Native 有哪些优势？

* **代码复用**：大部分业务逻辑和 UI 可在多个平台之间共享。
* **开发速度快**：借助快速刷新和熟悉的技术栈加快开发周期。
* **成本效益**：一个团队可同时维护 iOS 和 Android 应用，降低维护成本。
* **接近原生的性能**：得益于桥接原生模块，性能优于 WebView 方案。
* **开发者生态完善**：易于招聘具备 JavaScript/React 技能的开发者。

## 4\. 使用 React Native 有哪些劣势？

* **原生代码依赖**：某些平台特性需通过自定义原生模块实现。
* **性能瓶颈**：在动画、图像密集或计算密集型场景下不如原生应用。
* **调试复杂性**：涉及 JavaScript 与原生的协作时调试难度较高。
* **原生模块兼容性**：第三方原生模块质量参差不齐，可能存在兼容或维护问题。
* **升级成本**：React Native 升级涉及原生依赖，可能面临破坏性更新。

## 5\. React Native 与 Flutter 或 Xamarin 等其他跨平台开发框架相比如何？

比较取决于项目需求和团队技能：

* **React Native**：使用 JavaScript/React，社区庞大，渲染原生 UI，生态成熟。
* **Flutter**：使用 Dart 语言，依赖自有渲染引擎，UI 定制性强，性能优越。
* **Xamarin**：使用 C#/.NET，集成良好，代码共享比例高，微软支持。

选择框架需综合考虑团队背景、开发效率、性能需求、UI 复杂度和项目生命周期。

## 6\. 你能列举一些用 React Native 构建的流行应用程序吗？

一些流行应用包括：Walmart, Shopify, SoundCloud Pulse

## 7\. React Native 的核心组件有哪些？

核心组件是构建 UI 的基本元素：

* `View`：基础容器，类似 Web 中的 `div`。
* `Text`：显示文本内容。
* `Image`：显示静态图片或网络图像。
* `TextInput`：处理用户文本输入。
* `ScrollView`：用于滚动展示长内容。
* `FlatList`：用于高性能渲染长列表。
* `SectionList`：支持分组渲染的列表。
* `TouchableOpacity`、`Pressable`：处理用户点击事件。

## 8\. Expo 在 React Native 开发中扮演什么角色？

Expo 是一个开箱即用的工具集，提供开发、构建、测试和部署 React Native 应用的完整流程。它的优势包括：

* 提供大量原生模块（如摄像头、定位、通知）无需配置原生代码。
* 支持构建应用的云服务。
* 更快的开发启动体验。
* 新版中将“弹出”（eject）流程重命名为“预构建”（prebuild），允许开发者切换至裸 React Native 项目，接入自定义原生模块。

但对于需要大量自定义原生代码的项目，使用纯 React Native 会更灵活。

## 9\. 在 React Native 中如何处理状态管理？

状态管理方式与 React 保持一致，可根据复杂度选择：

* `useState`：管理组件内部状态。
* `useReducer`：处理更复杂的本地状态逻辑。
* `useContext`：在组件树中共享全局状态。
* Redux：集中式状态管理，适合大型应用。
* MobX：响应式状态管理，语法简洁。
* Recoil：由 Facebook 开发，适用于模块化、异步状态管理。
* Zustand、Jotai：轻量现代的替代方案，社区日益增长。

## 10\. 在 React Native 中如何为组件设置样式？

React Native 使用 JavaScript 样式对象进行样式声明：

* 属性使用驼峰命名，如 `backgroundColor`。
* 推荐使用 `StyleSheet.create` 创建样式对象，提高性能。
* 使用 Flexbox 进行布局，与 Web 类似，但不支持某些 CSS 功能（如继承、伪类）。
* 可结合动态样式、平台差异处理实现复杂界面。

## 11\. 在 React Native 中如何在不同屏幕之间导航？

最常用的导航库是 **React Navigation**，支持多种导航模式：

* `createStackNavigator`：用于页面堆栈导航。
* `createBottomTabNavigator`：用于底部 Tab 导航。
* `createDrawerNavigator`：用于侧边抽屉导航。
* `createMaterialTopTabNavigator`：顶部标签页导航。

React Navigation 提供灵活的参数传递、导航守卫、中间件等能力。

## 12\. 在 React Native 中如何处理用户输入？

用户输入通过组件的事件处理器捕获：

* `TextInput`：通过 `onChangeText`, `onSubmitEditing` 等处理文本输入。
* `Button`, `TouchableOpacity`, `Pressable`：通过 `onPress` 响应点击。
* `Switch`, `Slider`, `Picker`：提供多样输入控件，使用对应事件处理。

注意键盘弹出遮挡问题可通过 `KeyboardAvoidingView` 和 `ScrollView` 组合处理。

## 13\. 在 React Native 中如何进行 API 调用？

常用方法：

```js
fetch('https://example.com/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
````

* 可使用 `Axios` 提供更强的请求配置、拦截器和取消请求功能。
* 结合 `useEffect` 实现生命周期控制，避免重复请求。
* 建议封装网络请求逻辑，统一管理错误处理和重试策略。

## 14. 在 React Native 中如何持久化数据？

React Native 提供多种本地持久化方案：

* `@react-native-async-storage/async-storage`：社区维护的轻量键值对存储。
* `react-native-mmkv`：高性能、加密的本地键值数据库。
* SQLite：通过 `react-native-sqlite-storage` 等库提供结构化存储。
* Realm：强大的对象数据库，支持复杂数据模型和同步。
* 云端方案：如 Firebase、AWS Amplify 提供跨设备同步能力。

选择方案应根据数据复杂度、安全性、同步需求等因素考虑。

## 15. 在 React Native 中优化性能的最佳实践有哪些？

* 使用 `FlatList`/`SectionList` 替代 `ScrollView` 渲染长列表。
* 避免无意义的重渲染：使用 `React.memo`、`useMemo`、`useCallback`。
* 拆分大组件，提升可维护性和性能。
* 图像优化：压缩资源、使用 `Image` 的缓存策略。
* 减少桥接调用：避免频繁 JS 与原生之间通信。
* 使用性能监控工具（如 Flipper）识别性能瓶颈。
* 懒加载资源与组件，避免初始加载开销。
* 定期更新依赖，获取性能和稳定性改进。
