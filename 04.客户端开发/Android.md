# Android面试知识核心指南

[TOC]

## 1. 什么是 Android？

Android 是一个为移动设备设计的软件栈，包含操作系统、中间件和关键应用程序。Android 应用在各自的进程中运行，主要运行在 ART（Android Runtime）环境中。ART 通过预先编译（AOT）、垃圾回收优化等机制提升了应用性能。应用在独立进程中运行，保障了组件隔离和系统安全性。

## 2. 描述 Android 应用架构

* **Activity**：管理具有用户界面（UI）的单个屏幕。
* **Service**：处理无 UI 的后台操作。
* **Broadcast Receiver**：响应全局广播事件。
* **Content Provider**：在不同应用间安全共享结构化数据。
* **Intent**：组件间通信机制，用于启动组件或传递数据。

## 3. 什么是 APK 格式？

APK（Android Package Kit）是一个压缩文件，包含了应用所需的所有组件：

* 编译后的代码（DEX 文件）
* UI 布局（XML 文件）
* `res/` 资源目录、`assets/` 资源目录
* 清单文件（AndroidManifest.xml）
* 原生库（`lib/` 目录）
* `META-INF/`（签名信息）

所有这些文件被打包压缩，用于分发和安装 Android 应用。

## 4. 什么是 AndroidManifest.xml 文件，为什么需要它？

AndroidManifest.xml 是每个 Android 应用的核心配置文件，通常位于 `src/main/` 目录下。系统在运行任何应用代码之前，会读取此清单文件来获取：

* 应用组件声明（Activity、Service、BroadcastReceiver、ContentProvider）
* 请求的权限（如 `INTERNET`、`CAMERA` 等）
* 应用入口（Launcher Activity）
* 应用主题、图标、最小/目标 SDK 版本等配置

## 5. Android 架构的分层理解

Android 架构可分为：

1. **Linux 内核层**：进程管理、内存管理、驱动支持。
2. **原生库 & HAL**：如 WebKit、SQLite、OpenGL 等核心功能。
3. **Android Framework**：Java/Kotlin API（Activity 管理、通知系统等）。
4. **应用层**：系统和第三方应用均运行于此层，调用框架 API 提供服务。

## 6. 什么是 Activity？

Activity 是 Android 中代表单一用户界面的核心组件。每个 Activity 都对应一个屏幕，负责管理视图层次和用户交互。多个 Activity 共同构成应用的导航和功能流。

## 7. Activity 的生命周期以及 onDestroy() 何时可能不会被调用？

**生命周期回调方法**：

```
onCreate() → onStart() → onResume()
       ↕           ↕
     onPause() ← onStop()
       ↕
    onRestart()
       ↕
    onDestroy()
```

* 当系统为回收内存而强行终止应用进程时，`onDestroy()` 可能不会被调用。
* 如果在 `onCreate()` 中立即调用 `finish()`，通常不会执行 `onPause()`、`onStop()`。

**建议**：不要过度依赖 `onDestroy()`，关键清理可在 `onPause()` 或 `onStop()` 中完成。

## 8. Fragment 和 Activity 的区别及关系

* **Activity**：完整的屏幕级组件。
* **Fragment**：Activity 内的可重用 UI 模块，拥有独立生命周期，但受宿主 Activity 生命周期控制。

Fragment 可动态添加、删除或替换，用于构建响应不同屏幕尺寸的复杂界面。

## 9. 如何向 Fragment 提供构造参数？

使用 `Bundle`：

1. 在创建时构建参数 `Bundle`，调用 `fragment.setArguments(bundle)`。
2. 在 `onCreate()` 或 `onCreateView()` 中通过 `getArguments()` 获取参数。

不推荐使用自定义构造函数传参。

## 10. 在 Fragment 生命周期中, onAttach() 何时会被调用？

虽 `onAttach()` 会在每次 Fragment 附加到 Activity 时调用，但通常首次创建时调用一次；在配置更改等场景，也可能再次调用。

## 11. Activity、类（Class）和文件（File）在 Android 中的区别

* **文件（File）**：物理存储单元，如源代码（.java/.kt）、布局（.xml）、图片等。
* **类（Class）**：面向对象编程概念，定义属性和行为的代码蓝图。
* **Activity**：继承自 `Activity` 基类的一个类实例，代表一个 UI 屏幕，定义代码保存在对应的源文件中。

## 12. 什么是 Intent？它能用来向 ContentProvider 提供数据吗？

Intent 是组件间通信的消息对象，用于启动 Activity、Service 或发送广播。
**不能**直接用 Intent 向 ContentProvider “发送”数据，必须通过 `ContentResolver` 和相应的 URI 进行增删改查。

## 13. 显式 Intent 和隐式 Intent 区别

* **显式 Intent**：指定目标组件的类名，直接启动。
* **隐式 Intent**：只声明动作（Action）、数据（Data）、类别（Category），由系统匹配能响应的组件。

## 14. 使用 Intent 的三个常见用例

1. **启动 Activity**：`startActivity()`、`startActivityForResult()`。
2. **启动 Service**：`startService()`、`bindService()`。
3. **发送广播**：`sendBroadcast()`、`sendOrderedBroadcast()`。

## 15. Intent Filter 在 Android 中做什么？

在 AndroidManifest.xml 中声明组件能响应哪些隐式 Intent，系统根据 Intent Filter 匹配并启动相应组件。

## 16. 什么是 Sticky Intent？

Sticky Intent 是粘性广播，最近一次发送后“粘”在系统中，新注册的接收者仍能收到。
**注意**：`sendStickyBroadcast()` 在 API 21 被废弃，不推荐使用，建议使用 LiveData、EventBus 或 JobScheduler 等机制代替。

## 17. 启动模式（Launch Modes）

决定 Activity 实例与任务栈的关联方式。可通过两种方式定义：

1. 在 Manifest 中使用 `android:launchMode` 属性。
2. 在 Intent 中设置 Flags（如 `FLAG_ACTIVITY_NEW_TASK`、`FLAG_ACTIVITY_SINGLE_TOP` 等）。

**支持的模式**：

* **standard**（默认）：每次都创建新实例。
* **singleTop**：若栈顶已有实例则重用。
* **singleTask**：在任务中只保留一个实例并作为根。
* **singleInstance**：自 Android 10 起不推荐，Android 12 引入 singleInstancePerTask 作为替代，但目前兼容性有限，仅适用于特定 use-case。

## 18. 什么是 Service？

Service 是后台组件，用于执行长时间运行或远程进程任务，通常无用户界面。适用于音乐播放、文件下载等场景。

## 19. Service 与 IntentService 区别

* **Service**：在主线程运行，需手动管理子线程来执行耗时任务。
* **IntentService**：子类，自动创建工作线程按序处理 Intent，并在完成后自动停止。
  **注意**：`IntentService` 在 API 30 被废弃，推荐使用 WorkManager、Kotlin 协程或 JobIntentService。

## 20. AsyncTask 生命周期问题及替代方案

AsyncTask 生命周期与宿主 Activity 不一致，可能导致：

* 已销毁 Activity 的 UI 更新错误／崩溃
* 内存泄漏（持有 Activity 强引用）

**替代方案**：

* Jetpack ViewModel + LiveData
* WorkManager
* Kotlin Coroutines
* RxJava

**提示**：Android 11（API 30）起标记废弃，不推荐用于新项目。

## 21. 什么是 ANR？它为什么发生，如何避免？

ANR（Application Not Responding）指主线程（UI 线程）被阻塞超过 5 秒未响应用户或系统事件，触发系统对话框。
**原因**：在主线程进行网络请求、磁盘 I/O、大量计算或死锁等操作。
**避免**：将耗时操作移至后台线程，使用协程、WorkManager、IntentService（已废弃）等异步方案。

## 22. 屏幕旋转时视图状态恢复问题

若某视图在屏幕重新定向后未恢复，需检查该视图是否具有唯一且有效的 `android:id` 属性；系统自动保存/恢复视图状态依赖于此。

## 23. 什么是 ViewGroup？

ViewGroup 是可包含并管理其他 View 和 ViewGroup 的特殊 View，定义子 View 的布局方式。常见子类有 `LinearLayout`、`RelativeLayout`、`ConstraintLayout` 等。

## 24. 如何获取布局中定义的视图引用？

* 传统方式：`findViewById(R.id.view_id)`。
* 现代方式：开启 **View Binding** 或 **Data Binding**，在 Gradle 配置中启用后，可直接使用生成的绑定类访问视图。

## 25. drawable 文件夹是什么？

`res/drawable/` 及其限定符版本用于存放可绘制资源：

* 位图（PNG、JPG、WebP）
* Nine‑Patch 图（`.9.png`）
* XML 定义的 Shape、Selector、LayerList、Vector 等 Drawable

## 26. 什么是 Nine‑Patch 图像工具？

Nine‑Patch 是带有可拉伸区域标记的特殊 PNG（`.9.png`），可定义图像的拉伸和内容填充区域。Android Studio 内置编辑器或 `draw9patch` 工具可用于编辑。

## 27. Android 支持的对话框类型

* **AlertDialog**：通用对话框，可自定义标题、消息、按钮、列表或布局。
* **DatePickerDialog**、**TimePickerDialog**：日期/时间选择。
* **DialogFragment**：推荐用于管理对话框生命周期。

> **注意**：`ProgressDialog` 在 API 26 被废弃，建议用 `ProgressBar` + `DialogFragment` 实现。

## 28. Android 不同的存储方式

* **Shared Preferences**：存储少量键值对，常用于用户设置。
* **内部存储（Internal Storage）**：应用私有文件目录，无需额外权限。
* **外部存储（External Storage）**：应用专属或公共目录，需注意运行时权限。
* **SQLite 数据库**：结构化关系型数据，推荐使用 Jetpack Room。
* **网络存储**：通过 HTTP/HTTPS API 与远程服务器交互，适用于跨设备同步。

## 29. Shared Preferences 是什么？

轻量级键值对存储机制，数据以 XML 形式保存在应用内部存储。通过 `SharedPreferences.Editor` 的 `commit()`（同步）或 `apply()`（异步）提交。现代替代：Jetpack DataStore。

## 30. 主要的本地存储数据库

Android 平台主要使用 **SQLite**，推荐使用 Jetpack **Room Persistence Library**，提供编译时校验、ORM 支持和与 LiveData/Flow 集成等优势。

## 31. 什么是 ContentProvider？通常用于什么？

ContentProvider 管理结构化数据的访问，为其他授权应用提供基于 URI 的 CRUD 接口。

* **跨应用数据共享**
* **访问系统数据**（联系人、媒体等）

## 32. 什么是 Google Android SDK？包含哪些工具？

Android SDK 是 Google 提供的开发、测试和调试工具集合，主要组件包括：

* **Android Emulator**：模拟器
* **ADB（Android Debug Bridge）**：设备通信工具
* **AAPT（Android Asset Packaging Tool）**：资源打包
* 构建工具、平台 API、系统镜像、文档等
* Android Studio Profiler（取代 DDMS 大部分功能）

## 33. 什么是 ADB？

ADB 是命令行工具，包含客户端、守护进程（adbd）、服务器三部分。用于安装/卸载应用、文件传输、执行 Shell 命令、查看日志（Logcat）等操作。

## 34. 什么是 DDMS？功能有哪些？

DDMS（Dalvik Debug Monitor Server）曾提供设备/进程信息、Logcat、文件浏览器、线程/堆内存分析、模拟器控制等功能。大部分已被 Android Studio Profiler 所取代。

## 35. Serializable 与 Parcelable 的区别

* **Serializable**：Java 标准接口，通过反射序列化，性能较低，产生临时对象多。
* **Parcelable**：Android 特有，高性能、手动实现读写逻辑，产生垃圾少，适用于 IPC。

在 Android 中推荐使用 Parcelable。

## 36. 与传感器相关的四个核心类

核心管理接口：

1. **SensorManager**：系统服务，列出并管理传感器，注册/注销监听器。
2. **Sensor**：代表具体传感器，提供元数据。
3. **SensorEventListener**：接口，`onSensorChanged()` 和 `onAccuracyChanged()` 回调传感器数据和精度变化。

数据载体：
4. **SensorEvent**：封装一次传感器事件数据（values、时间戳、精度、Sensor 对象）。

## 37. 什么是 Toast？语法示例

Toast 是一种非侵入式短暂浮动消息提示：

```java
Toast.makeText(context, "要显示的消息", Toast.LENGTH_SHORT).show();
```

参数说明：

* `context`：通常为 Activity 或 Application Context
* 文本：`CharSequence` 或资源 ID
* 显示时长：`Toast.LENGTH_SHORT` 或 `Toast.LENGTH_LONG`
