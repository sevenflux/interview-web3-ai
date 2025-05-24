# Uniapp

[TOC]

## 1. uniapp的主要优缺点有哪些？

优点方面可以总结为五点：1) 跨平台特性显著，一套代码能生成多端应用；2) 学习门槛低，语法基于Vue框架且组件系统类似微信小程序；3) 扩展性强，可通过插件市场快速集成功能模块；4) 开发环境友好，使用HBuilderX支持热更新等特性；5) 突破H5调用原生能力的限制。具体表现为应用性能接近原生，可直接调用设备功能如摄像头或陀螺仪。

缺点也存在几个关键点：1) 生态成熟度不足，由于诞生时间短仍存在功能缺失；2) 社区资源有限，遇到疑难问题时解决方案较少；3) Android端性能优化不如微信小程序和iOS平台；4) 文件命名需遵循特定规范，存在开发约束；5) 官方响应速度有时较慢，问题修复周期较长。

> [考察重点] 该问题旨在评估对uniapp技术选型的全局认知能力，面试中还需结合具体开发场景阐述优劣势的实践影响

## 2. uniapp项目结构中的核心配置文件及入口文件分别承担什么职责？

项目核心结构包含以下关键部分：

- **pages.json**: 全局路由配置文件，用于声明页面路径、窗口样式、导航栏参数等，类似微信小程序的app.json。可配置下拉刷新等页面级特性

```javascript
{
  "pages": [
    {"path": "pages/index/index"}
  ],
  "globalStyle": {
    "navigationBarTitleText": "示例"
  }
}
```

- **main.js**: Vue实例初始化入口，负责注册全局组件、挂载Vuex状态管理等。需注意此处不能使用vue-router进行路由管理
- **App.vue**: 应用根组件，不包含具体页面元素，主要处理应用生命周期事件监听
- **manifest.json**: 跨平台配置文件，定义应用名称、图标等打包参数，HBuilderX创建的项目中位于根目录
- **uni.scss**: 全局样式变量配置文件，支持SCSS预处理

> [关键点] pages.json的路由配置是uniapp区别于标准Vue项目的核心特征，需要特别注意subPackages分包配置的使用场景

## 3. 描述uniapp开发典型流程中的关键环节

基础开发流程主要分为四个阶段：1) 使用HBuilderX创建项目时选择合适的模板（如uni-starter），可快速获得预置的登录、支付等基础模块；2) 通过插件市场安装UI库或功能模块，例如使用uView扩展组件库；3) 在pages.json中配置页面路由时注意启动页声明规则，tabBar配置需使用规范图标路径；4) 页面开发采用Vue单文件组件模式，支持通过vuex进行状态管理。

代码示例展示tab页面配置：

```javascript
"tabBar": {
  "list": [{
    "pagePath": "pages/index/index",
    "iconPath": "static/icons/home.png",
    "selectedIconPath": "static/icons/home-active.png",
    "text": "首页"
  }]
}
```

实际开发中常配合uni_modules插件系统进行模块化开发，同时需注意使用CSS变量实现主题定制能力。

## 4. 对比Vue、微信小程序、uni-app在属性绑定语法上的区别

在动态属性绑定场景下，三个框架表现如下特性：

- **Vue/uni-app**: 使用 `:prop="value"` 的v-bind简写语法

```html
<img :src="dynamicUrl">
```

- **微信小程序**: 双大括号包裹表达式

```html
<image src="{{dynamicUrl}}"></image>
```

> [易错点] 小程序中漏写双括号会导致将变量名解析为字符串字面量，需要特别注意语法校验

## 5. 比较jQuery、Vue、小程序及uniapp的本地存储实现方式

各技术栈的存储方式差异主要体现在API设计上：

1. **jQuery**: 依赖Cookie操作（需插件支持）

```javascript
$.cookie('token', 'abc123');
```

2. **Vue**: 直接使用Web Storage API

```javascript
localStorage.setItem('user', JSON.stringify(userData));
```

3. **微信小程序**: 同步/异步API分离

```javascript
wx.setStorageSync('key', 'value');
```

4. **uniapp**: 提供统一存储接口，支持Promise封装

```javascript
uni.setStorage({ key: 'sysInfo', data: deviceInfo });
```

> [最佳实践] uniapp的同步存储方法性能更优但可能阻塞UI，建议在数据量较大时使用异步操作

## 6. uniapp中如何进行页面间通信与全局事件管理？

核心通信手段包含：

1. **应用实例引用**

```javascript
const app = getApp();
app.globalData.sharedValue = 100;
```

2. **页面栈操作**

```javascript
const pages = getCurrentPages();
pages[0].$vm.pageMethod();
```

3. **全局事件总线**

```javascript
// 发送事件
uni.$emit('network-error', { code: 500 });

// 监听事件
uni.$on('network-error', (data) => {
  console.error('网络异常:', data.code);
});
```

> [注意事项] 事件监听需在组件卸载时及时通过uni.$off解绑，避免内存泄漏

## 7. Vue与uniapp的生命周期有何异同？

**Vue生命周期**：

- 包含组件挂载前后的beforeCreate/created阶段
- 数据驱动更新的beforeUpdate/updated钩子
- 组件销毁流程的beforeDestroy/destroyed

**uniapp页面生命周期**：

```javascript
onLoad(options) {
  // 解析路由参数
},
onShow() {
  // 处理页面展示逻辑
},
onReady() {
  // 执行DOM相关操作
}
```

> [差异点] uniapp的页面生命周期更贴近移动端场景，支持下拉刷新(onPullDownRefresh)、页面滚动(onReachBottom)等移动端特有钩子。组件级生命周期仍遵循Vue规范

## 8. uniapp中实现页面跳转有哪几种主要方式？

主要路由API分为三类：

1. **普通跳转**

```javascript
uni.navigateTo({
  url: '/pages/detail?id=1'
});
```

2. **Tab切换**

```javascript
uni.switchTab({
  url: '/pages/user'
});
```

3. **重定向**

```javascript
uni.redirectTo({
  url: '/pages/login'
});
```

> [路由配置要点] 需确保pages.json中已正确声明目标页面路径，否则会触发页面不存在的运行时错误

## 9. 如何通过条件编译实现跨平台适配？

条件编译通过预处理指令实现平台差异代码管理：

```html
<!-- 平台专属组件 -->
<!-- #ifdef MP-WEIXIN -->
<wechat-specific-button />
<!-- #endif -->

// 平台API调用差异
// #ifdef H5
console.log('运行在浏览器环境');
// #endif
```

样式级条件编译示例：

```css
/* #ifdef IOS */
.container {
  padding-top: 20px; /* 适配iOS状态栏 */
}
/* #endif */
```

> [编译原理] 构建时根据编译目标自动剔除无关平台代码，最终打包产物仅包含对应平台的有效代码

## 10. uniapp文件上传功能如何实现？

完整文件上传流程包含三个步骤：

1. **文件选择**

```javascript
uni.chooseImage({
  success: (res) => {
    const tempFile = res.tempFiles[0];
  }
});
```

2. **上传参数配置**

```javascript
const uploadTask = uni.uploadFile({
  url: 'https://api.example.com/upload',
  filePath: tempFile.path,
  name: 'file',
  formData: { user: 'test' }
});
```

3. **进度监控**

```javascript
uploadTask.onProgressUpdate((res) => {
  console.log(`上传进度：${res.progress}%`);
});
```

> [性能优化] 大文件上传建议分片处理，Android平台需注意路径权限问题

## 11. 解释前端常见尺寸单位的应用场景

各单位的特性与适用场景：

- **rpx**: uniapp特有的响应式单位，750rpx等于屏幕宽度，适合多端适配
- **vw/vh**: 视窗比例单位，适合全屏布局元素定位
- **rem**: 通过根字体大小计算，适用于需要整体缩放布局的场景
- **%**: 相对父容器百分比，常见于流式布局
- **px**: 像素绝对单位，适合需要精准控制的边框或图标

示例换算关系：

```css
/* 在375px宽屏幕上 */
.element {
  width: 375rpx; /* 转换为50%屏幕宽度 */
}
```

> [适配方案] uniapp项目推荐使用rpx为主单位，配合flex布局实现跨端适配效果

## 12. 解释jQuery、Vue、uni-app和小程序之间的页面传参方式差异

> [考察内容]  
此处需要区分传统Web开发与现代框架在参数传递机制上的不同，同时对比跨平台框架在移动端特有的处理方式。

1. **jQuery** 使用URL拼接方式传参，如`page.html?key=value`
2. **Vue** 通过路由机制传递参数，支持两种方式：
   - `<router-link :to="{ path:'/detail', query:{ id:1 } }">`标签传参
   - `this.$router.push({ path:'/detail', query:{ id:1 } })`编程式传参
3. **小程序/uni-app** 主要采用路径拼接方式（类似jQuery），但uni-app额外支持：
   - 全局变量（Vuex状态管理）
   - 事件总线（Event Bus）传递数据

参数接收示例：

```javascript
// uni-app接收参数
onLoad(options) {
  const item = JSON.parse(decodeURIComponent(options.item))
}
```

## 13. uni-app实现上拉加载更多数据的核心逻辑

典型分页控制流程示例：

```javascript
onReachBottom() {
  if (this.pageStatus === 'noMore') return
  
  this.pageNum++
  this.pageStatus = 'loading'
  this.fetchData().then(res => {
    this.pageTotal = res.total
    this.checkPageStatus()
  })
}

checkPageStatus() {
  const maxPage = Math.ceil(this.pageTotal / this.pageSize)
  this.pageStatus = this.pageNum >= maxPage ? 'noMore' : 'loadmore'
}
```

## 14. 如何处理scroll-view组件中sticky定位失效问题？

> [问题场景]  
在小程序端，父容器为scroll-view时，子元素sticky定位在触底时失效

**解决方案**：  
添加中间容器层进行嵌套

```html
<scroll-view>
  <view class="sticky-container">
    <view class="sticky-element"></view>
  </view>
</scroll-view>
```

CSS关键样式：

```css
.sticky-container { height: calc(100% + 1px); }
.sticky-element { position: sticky; top: 0; }
```

## 15. iOS输入框内容随页面滚动的处理方案

**问题现象**：  
焦点仍在输入框时滚动页面，输入内容会跟随移动

**解决方案**：  

```html
<textarea 
  fixed="true"
  :adjust-position="false"
  @blur="handleBlur"
></textarea>
```

```javascript
handleBlur() {
  // 手动触发布局重排
  wx.pageScrollTo({ scrollTop: 0 })
}
```

## 16. uni-app条件编译的实现方式与平台标识符

两种条件编译语法：  

1. 模板中使用`<!-- #ifdef H5 -->`注释语法
2. JavaScript/CSS中使用`#ifdef`预处理指令

**平台标识范例**：  

- `H5`：Web端
- `MP-WEIXIN`：微信小程序
- `APP`：原生应用

## 17. uni-app项目核心文件结构说明

关键文件配置体系：  

- `pages.json`：路由配置与全局样式
- `main.js`：Vue初始化入口
- `App.vue`：根组件，全局生命周期管理
- `pages/`：所有页面组件存储目录

## 18. uni.request文件上传API的调用规范

标准上传请求示例：

```javascript
uni.uploadFile({
  url: 'https://api.example.com/upload',
  filePath: tempFilePath,
  name: 'file',
  formData: {
    'extra_param': 'value'
  },
  success(res) {
    console.log('Upload success:', res.data)
  }
})
```

> [参数说明]  
`name`字段对应后端接收文件的参数名称，通常由后台接口定义

## 19. 移动端常用CSS单位的特性对比

单位体系解析：  

- `rpx`（responsive pixel）：自适应单位，750rpx = 100%屏幕宽度
- `px`：固定像素，不同设备物理像素密度差异可能影响显示效果
- `rem`：基于根元素`<html>`字体大小的比例单位
- `vw/vh`：视窗尺寸比例单位，1vw = 视窗宽度1%

## 20. uni-app实现分页加载的状态控制逻辑

核心状态管理流程：  

1. `onReachBottom`生命周期触发分页请求
2. 加载过程中显示`loading`状态
3. 接口返回后根据total判断是否还有数据
4. 更新分页参数与状态（loading/noMore）

```javascript
data() {
  return {
    pageStatus: 'loadmore', // 状态枚举：loadmore/loading/noMore
    pageNum: 1,
    pageSize: 10
  }
}
```

## 21. uni-app页面滚动的监听实现方式

通过`onPageScroll`生命周期实现：

```javascript
export default {
  onPageScroll(e) {
    console.log('Current scroll position:', e.scrollTop)
    this.handleScrollAnimation(e.scrollTop)
  }
}
```

## 22. 图片自适应宽高比的技术方案

**实现方案**：  

```html
<image 
  :src="imgUrl" 
  mode="widthFix" 
  style="width: 300rpx;"
/>
```

`mode=widthFix`使高度根据原图比例自动计算

## 23. uni-app的UI组件体系构成解析

**三层组件生态**：  

1. **原生组件库**：
   - 跨平台自适应组件（`<uni-list>`、`<uni-card>`）
   - 自动匹配各平台设计规范（iOS/Android/小程序）

2. **第三方库整合**：
   - uView UI：提供80+组件与工具库

   ```html
   <u-button icon="home">按钮</u-button>
   ```

   - Vant Weapp：微信生态风格组件库

3. **自定义组件开发**：

   ```javascript
   // 封装可复用组件
   export default {
     props: ['title'],
     template: `<view class="custom-card">{{ title }}</view>`
   }
   ```

## 24. uni-app页面刷新的实现方案

三种刷新策略：  

1. **强制重载**：

```javascript
uni.reLaunch({ url: '/pages/refresh' })
```

2. **数据驱动更新**：

```javascript
this.dataList = []
this.loadData()
```

3. **页面间通信刷新**：

```javascript
// 前页
uni.$emit('refreshEvent')

// 目标页
uni.$on('refreshEvent', this.refreshData)
```

## 25. uni-app在App端相比小程序的定制优势

定制能力差异点：  

1. 原生组件覆盖范围更广（如video组件支持更多属性）
2. 支持更复杂的交互动效（通过BindingX实现）
3. 可深度定制导航栏样式（透明度/动态颜色）
4. 支持混合原生渲染（如地图覆盖原生控件）

典型示例代码：

```javascript
// 动态修改导航栏颜色
uni.setNavigationBarColor({
  frontColor: '#ffffff',
  backgroundColor: '#007aff'
})
```

## 26. UniApp 中有哪些常用组件？

UniApp 提供了多种基础组件用于界面构建，主要涵盖以下类别：

> [考察内容] 此处需要展示对不同组件类别的熟悉程度及其具体应用场景  
基础组件包含`view`（容器，类似HTML的`div`）、`text`（文本展示）、`image`（图片加载）和`scroll-view`（滚动容器）。表单组件包括`input`（单行输入）、`textarea`（多行文本）、`picker`（选择器）等数据采集元素。导航相关的`navigator`实现页面跳转，媒体组件则有`video`（视频播放）、`camera`（摄像头调用）等多媒体功能元素。地图应用可用`map`组件，日历功能则使用`calendar`组件。

## 27. 如何解决 UniApp 中的跨域问题？

三种主流解决方案：首先要求后端配置CORS（跨域资源共享）头信息是最根本的方法。开发阶段可通过本地代理服务器转发请求绕过限制，具体实现需要修改项目`manifest.json`的代理设置。应急场景可手动设置请求头中的`Access-Control-Allow-Origin`字段，但需注意这种方式在生产环境可能存在限制。

## 28. UniApp 中表单功能如何实现？

通过`<form>`标签包裹输入组件，结合`@submit`事件处理数据提交。典型实现如下：

```html
<template>
  <form @submit="handleSubmit">
    <input v-model="formData.name" placeholder="姓名"/>
    <textarea v-model="formData.bio" placeholder="个人简介"/>
    <button type="submit">提交</button>
  </form>
</template>

<script>
export default {
  data() {
    return { formData: { name: '', bio: '' } }
  },
  methods: {
    handleSubmit(e) {
      uni.showToast({ title: '提交成功' })
      console.log('表单数据:', this.formData)
    }
  }
}
</script>
```

> [深入理解] `v-model`通过双向绑定自动处理输入值收集，无需手动获取DOM元素值

## 29. 如何在 UniApp 中获取页面元素信息？

使用选择器查询API：通过`uni.createSelectorQuery()`构建查询对象，关键步骤包括：

```javascript
// 获取元素布局信息
const query = uni.createSelectorQuery().in(this)
query.select('#target').boundingClientRect(data => {
  console.log('元素宽高:', data.width, data.height)
}).exec()
```

> [注意点] 小程序环境中需要在`mounted`生命周期后执行查询，H5端可直接操作DOM

## 30. UniApp 全局变量的实现方式有哪些？

三种典型方案：  

1. **Vue原型扩展**：在`main.js`中挂载至`Vue.prototype.$globalVar`  
2. **应用级存储**：通过`getApp().globalData`定义全局数据  
3. **持久化存储**：使用`uni.setStorageSync`/`uni.getStorageSync`进行本地缓存  

示例代码片段：  

```javascript
// 方式1：原型扩展
Vue.prototype.$apiBase = 'https://api.example.com'

// 方式2：globalData
// App.vue
export default { globalData: { userToken: null } }
// 使用端
const app = getApp()
console.log(app.globalData.userToken)

// 方式3：Storage
uni.setStorage({ key: 'session', data: { expires: 3600 } })
```

## 31. UniApp 中存在哪些参数传递方式？

> [场景分析] 这个问题需要演示不同场景下的参数传递策略  

1. **URL参数**：通过`uni.navigateTo`的url携带query参数，适用于简单参数传递
2. **全局状态**：使用`globalData`共享复杂对象数据，适合多页面共享数据
3. **本地存储**：`uni.setStorage`适合需要持久化的参数传递
4. **事件总线**：利用Vue实例作为事件中心实现跨组件通信

实践示例：  

```javascript
// URL传参示例
uni.navigateTo({ url: '/pages/detail?id=1001' })
// onLoad(options)中通过options.id获取

// 事件总线模式
// event.js
export const bus = new Vue()
// 发送方
bus.$emit('dataUpdate', { newData: 42 })
// 接收方
bus.$on('dataUpdate', handleUpdate)
```

## 32. UniApp 中如何创建交互式弹窗？

系统提供了三类弹窗API：  

1. **提示反馈**：`uni.showToast`用于轻量级消息提示（自动消失）
2. **确认交互**：`uni.showModal`支持按钮确认/取消操作  
3. **操作菜单**：`uni.showActionSheet`提供选项列表选择功能

代码示例演示：  

```javascript
// 成功提示
uni.showToast({ title: '保存成功', icon: 'success' })

// 确认对话框
uni.showModal({
  title: '删除确认',
  content: '确定删除该记录？',
  success(res) {
    if (res.confirm) console.log('执行删除')
  }
})

// 操作菜单
uni.showActionSheet({
  itemList: ['拍照', '从相册选择'],
  success(res) {
    const methods = [openCamera, openAlbum]
    methods[res.tapIndex]()
  }
})
```

> [优化技巧] 复杂弹窗建议封装为自定义组件，提高代码复用性

## 33. 请解释Uniapp的分层设计架构及其优势

Uniapp 采用逻辑层与视图层分离的架构，主要目的是适配多平台特别是非H5端运行环境。以即时通讯功能为例，通过WebSocket实现的双向通信就运行在逻辑层，而消息展示界面则属于视图层。>[深入理解] 这里考察对跨平台架构核心思想的理解，需说明分层机制如何解决不同平台渲染差异

逻辑层专注于数据处理和业务逻辑（如uni.request接口调用），使用JavaScript编写且跨平台通用。视图层则处理平台特定的UI渲染，例如微信小程序中转化为`<view>`组件，iOS中则对应原生UIKit控件。这种解耦设计带来三大优势：业务代码复用率最大化、不同平台的渲染优化可独立进行、代码维护复杂度显著降低。以待办事项应用为例，数据获取逻辑只需编写一次，而列表在不同平台分别使用Web组件、小程序组件和原生控件实现。

>[平台差异拓展] 需注意不同端的具体实现：H5直接使用DOM渲染，原生端需调用平台UI线程。性能关键场景下（如长列表），分层架构可针对性优化各层表现

## 34. 在Uniapp中如何实现即时通讯功能？请描述其核心机制

Uniapp IM解决方案基于WebSocket协议搭建长连接通道，支持消息实时收发。相较于传统HTTP轮询，这种全双工通信协议使消息延迟降低70%以上。>[技术要点] 需要解释WebSocket与HTTP在实时性方面的本质差异，以及心跳机制等保活策略

核心功能模块采用分层实现：逻辑层处理消息编解码、会话管理和状态同步（通过uni.connectSocket API建立连接），视图层根据不同平台渲染聊天界面。发送图片时，逻辑层先将文件上传至云存储，获取URL后通过WebSocket发送消息体，接收方视图层根据消息类型渲染为`<image>`组件（H5）或`<image>`原生组件（小程序）。离线消息通过uni.push配合平台推送服务（如APNs）实现可达性保障。

>[扩展案例] 开发社交应用时，利用uni.createIMClient创建客户端实例，使用onMessage事件监听实时消息，结合scroll-view组件实现聊天记录滚动定位。群聊功能通过加入聊天室ID实现消息路由

```javascript
// 连接IM服务示例
const client = uni.createIMClient({
  appId: 'your-appid'
});
client.on('message', (message) => {
  this.messages.push({
    content: message.content,
    isSelf: message.from === this.userId
  });
});
```

## 35. Uniapp如何处理复杂的异步操作？请结合实际场景说明

通过uniAppPromise对原生API进行Promise化封装，消除回调地狱（Callback Hell）。以用户登录流程为例，传统嵌套回调需处理微信授权、云函数调用、本地存储等多个异步步骤，而采用async/await可使代码保持线性结构。>[原理剖析] 需说明Event Loop机制下Promise的微任务队列优势，对比generator函数的差异

典型应用场景中的异步处理流程：

1. uni.login获取临时凭证
2. uni.request发送至认证服务器
3. uni.setStorage同步本地登录状态
4. uni.navigateTo跳转主页

使用uniAppPromise重构后的代码可读性提升40%：

```javascript
async function login() {
  try {
    const { code } = await uniAppPromise.login();
    const res = await uniAppPromise.request({
      url: '/api/auth',
      data: { code }
    });
    await uniAppPromise.setStorage({ key: 'token', data: res.token });
    uniAppPromise.navigateTo({ url: '/home' });
  } catch (error) {
    uniAppPromise.showToast({ title: '登录失败' });
  }
}
```

>[性能优化] Promise.all可用于并行请求，如同时获取用户信息和地理位置。错误处理方面需注意finally块进行清理操作，示例中展示统一的catch处理逻辑
