# Android

[TOC]

## 1. 什么是 Android？

这是一个开源的移动设备操作系统，主要用于手机和平板等设备。基于 Linux 内核构建的系统，配备丰富的组件库，开发者可以创建能执行基础和高级功能的应用程序。

> [考察重点] 这里主要检验对 Android 系统本质特征的理解。该答案需要准确描述其开源特性、适用场景和核心技术基础。

## 2. 什么是 Google Android SDK？

Google Android SDK 是开发者在 Android 设备上编写应用程序所需的工具集合。包含图形界面模拟器，能够仿真安卓手持设备环境，方便进行代码测试与调试。

> [技术细节] 注意强调 SDK 的功能组成和核心作用。模拟器部分需体现其对真实设备环境的还原能力。

## 3. 什么是 Android 系统架构？

Android 架构由四个关键组件构成：

- Linux 内核层提供硬件抽象和核心驱动
- 库层包含 C/C++ 核心库和运行时环境
- 框架层提供 Java API 管理应用组件
- 应用层包含用户可见的应用程序

## 4. 描述 Android 框架层

作为 Android 架构的核心部分，框架层提供了开发应用所需的各种类和方法。包含 Activity Manager、Window Manager 等关键服务，管理应用的生命周期、界面系统等基础功能。

## 5. 什么是 AAPT？

Android 资源打包工具（Android Asset Packaging Tool）的处理神器。开发者能用它创建、查看和管理 zip 格式的归档文件，例如编译资源文件生成 R.java 的过程就依赖它。

```java
// 示例用法：查看 APK 包内容
aapt list app-debug.apk
```

## 6. 模拟器在 Android 环境中的重要性？

模拟器为开发者提供类真机环境，支持代码编写、测试和调试。尤其在早期开发阶段，可以安全验证基础功能而无需物理设备，例如测试不同屏幕尺寸适配情况。

## 7. activityCreator 的作用？

这是创建新 Android 项目的起点工具，本质是自动生成项目文件结构的 shell 脚本。会建立 AndroidManifest.xml、源码目录等基础框架，不过现代 Android Studio 已自动处理这些初始化工作。

## 8. 描述 Activity

Activity 就像通向用户界面的窗口，类似于桌面应用的对话框窗口。但要注意，Activity 并不强制要求可视化界面（可设置透明主题），例如后台处理场景中的特殊用法。

## 9. 什么是 Intent？

Intent 是系统级的消息传递机制，既可用于组件间通信（如启动 Activity），也能向用户推送通知。通过 PendingIntent 可让用户对通知进行响应，例如点击通知栏消息跳转指定页面。

## 10. Activity 与 Service 的区别？

核心差异在于生命周期和可见性：
Activity 随用户操作随时关闭，主要负责界面交互
Service 无界面长期后台运行，处理音乐播放等持续任务
> [生命周期对比] Activity 受界面状态影响，而 Service 需主动调用 stopService() 终止。

## 11. Android 项目的必备文件？

每个 Android 项目必须包含：

- 清单文件 AndroidManifest.xml
- 构建配置 build.gradle（旧版用 build.xml）
- 编译输出目录 build/
- 源代码目录 src/
- 资源目录 res/
- 原始资源 assets/

## 12. XML 布局的重要性？

XML 布局实现界面与逻辑的分离，保证 UI 定义规范统一。主流做法是将布局参数保存在 res/layout 的 XML 文件中，而动态逻辑写在 Java/Kotlin 代码里，有利团队协作和维护。

## 13. 什么是容器？

容器本质是界面元素的布局管理器，例如 LinearLayout、RelativeLayout 等。它们负责管理子控件的位置关系和排列方式，例如 LinearLayout 通过 orientation 属性决定横向或纵向排列。

```xml
<!-- 典型容器示例 -->
<LinearLayout
    android:orientation="vertical">
    <TextView.../>
    <Button.../>
</LinearLayout>
```

## 14. 什么是 Orientation？

通过 setOrientation() 设置线性布局方向：

- HORIZONTAL 横向排列子元素
- VERTICAL 纵向堆叠子元素
这种特性使得 LinearLayout 成为最基础的布局容器之一。

## 15. Android 在移动市场的优势？

开放源码特性降低开发门槛，统一的应用分发渠道（Google Play）覆盖数十亿设备。开发者只需一次开发即可适配多种设备，通过应用变现机制实现商业价值，例如应用内购买和广告集成。

## 16. Android 的缺点？

主要痛点表现在：

1. 系统碎片化：不同厂商定制导致版本兼容问题
2. 硬件多样性：屏幕尺寸、分辨率适配复杂
3. 功耗管理：后台服务过度消耗电量
这需要开发者花费额外精力处理兼容性测试和优化。

## 17. 什么是 adb？

Android 调试桥 (Android Debug Bridge) 是 PC 与设备/模拟器通信的命令行工具。核心功能包括：

- 安装/卸载应用
- logcat 日志查看
- 文件传输
- shell 命令执行

```shell
# 查看连接设备
adb devices
# 安装 APK
adb install app.apk
```

## 18. Activity 的四个生命周期状态？

完整生命周期包括：
Active（活动）: 界面可见且可交互
Paused（暂停）: 部分遮挡但仍可见（如对话框弹出）
Stopped（停止）: 完全不可见但对象存在
Destroyed（销毁）: 对象被回收
需结合 onStart()/onStop() 等方法理解状态转变。

## 19. 什么是 ANR？

应用无响应（Application Not Responding）的弹窗警告。当主线程阻塞超过 5 秒（例如复杂计算未放在子线程）时触发，严重影响用户体验，需通过异步任务解决。

## 20. AndroidManifest 中必须且唯一的元素？

manifest 根元素与 application 元素必须存在且唯一。其他组件如 activity、service 等可根据需求多次声明。但问题原文可能存在表述歧义，正确理解应为这两个元素的强制性。

## 21. 属性中如何转义特殊字符？

使用双反斜杠转义，典型场景如：

- 换行符 `\\n`
- 引号 `\\"`
- Unicode 字符 `\\u0020`
这些转义主要在 XML 资源文件中使用。

## 22. 权限设置的开发意义？

保护敏感数据和系统功能，例如：

- 网络访问权限
- 相机调用权限
- 定位权限
缺省情况下应用无权限访问这些资源，需在 AndroidManifest.xml 显式声明。

## 23. Intent 过滤器的作用？

Intent 过滤器在清单文件中声明组件能处理的意图类型，例如：

```xml
<activity android:name=".ShareActivity">
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
</activity>
```

这样配置后，该 Activity 就能响应发送文本的意图。

## 24. Activity 监控的三大生命周期？

1. 完整周期（onCreate 到 onDestroy）：管理全局资源申请与释放
2. 可见周期（onStart 到 onStop）：控制界面相关资源（如摄像头）
3. 前台周期（onResume 到 onPause）：处理用户交互
正确管理这些周期是避免内存泄漏的关键。

## 25. 在什么情况下会触发 onStop() 方法？

当 Activity 对用户不再可见时会触发此方法。这种情况通常发生在其他 Activity 完全覆盖当前界面（比如启动新 Activity）或当前 Activity 即将被销毁时。例如，用户导航到其他应用程序或点击返回键结束当前 Activity 都可能触发这个生命周期回调。

> [深入理解] 此处需要区分 onStop() 与 onPause() 的触发场景：前者是整个界面不可见，而后者只是失去焦点但可能仍部分可见（例如对话框覆盖时）。

## 26. 是否存在其他资源限定符优先于语言区域（locale）的情况？

确实存在两种特殊限定符具有更高优先级：MCC（移动国家代码）和 MNC（移动网络代码）。这两个网络相关的标识符会覆盖语言区域设置，例如设备检测到中国移动运营商代码时，即使系统语言设置为英文，仍可能优先加载带 mcc460 限定符的资源目录。

## 27. Android 进程的不同状态有哪些？

根据进程中的组件状态，分为四种主要类型：

1. 前台进程：持有正在与用户交互的 Activity
2. 可见进程：存在用户可见但无焦点的界面（如弹出对话框的后台 Activity）
3. 后台进程：包含对用户不可见但尚未终止的 Activity
4. 空进程：不包含任何活动组件的缓存进程

## 28. Dalvik 在 Android 开发中扮演什么角色？

Dalvik 是 Android 早期使用的虚拟机（Virtual Machine），负责执行 dex 格式的字节码。其特点包括寄存器架构、多进程隔离和针对移动端优化的内存管理。每个应用运行在独立的虚拟机实例中，2014 年之后逐步被 ART（Android Runtime）取代，但仍需要理解其作为 Android 执行环境的基础作用。

> [演进说明] ART 采用预编译（AOT）替代 Dalvik 的即时编译（JIT），显著提升应用启动速度。

## 29. 什么是 AndroidManifest.xml 文件？

这是每个 Android 应用的配置清单文件，包含四大组件声明、权限申请和硬件特征需求等重要信息。例如注册 Activity 时需要在这个文件中添加 `<activity>` 节点。还定义应用版本、支持屏幕方向、主题风格等元数据。

```xml
<manifest>
    <application android:icon="@drawable/icon">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
```

## 30. 如何正确设置 Android 设备进行应用调试？

需要三个关键步骤：

1. 在 AndroidManifest.xml 的 `<application>` 标签中添加 `android:debuggable="true"`
2. 进入手机开发者选项启用 USB 调试模式
3. 配置 ADB（Android Debug Bridge）驱动保证设备可被识别。可在终端执行 `adb devices` 验证连接。

## 31. 列举用 AIDL 创建绑定服务的步骤

整个流程分成三个关键阶段：

1. 定义 AIDL 接口文件：使用 Java 语法声明跨进程调用的方法
2. 实现 Stub 抽象类：继承生成的接口 Stub 并覆写业务逻辑方法
3. 通过 Service 的 onBind() 方法返回接口实例：

```java
public IBinder onBind(Intent intent) {
    return new MyBinder();
}

private class MyBinder extends IMyAidlInterface.Stub {
    public int calculate(int a, int b) {
        return a + b;
    }
}
```

## 32. 默认资源的重要性是什么？

默认资源目录（如 res/values/）包含无任何限定符的基础资源。当系统无法匹配设备配置的特殊资源（如中文语言包缺失）时，会自动回退到默认资源。缺少必要的默认资源会导致应用崩溃，例如没有英文字符串文件时在某些区域设置下会抛出 Resources.NotFoundException。

## 33. 多资源匹配时哪个限定符具有最高优先级？

在 MCC/MNC 等特殊情况之外，语言区域（locale）通常是最高优先级限定符。当系统确定设备语言为法语时，会优先加载 res/values-fr/ 目录下的字符串资源，即使其他条件（如屏幕方向）也更符合其他资源包。

## 34. 什么情况下会出现 ANR？

触发 ANR 的有两类场景：

1. 输入事件（如按键或屏幕触摸）5 秒内无响应
2. BroadcastReceiver 的 onReceive() 执行超过 10 秒未完成

后台服务不会直接导致 ANR，但前台的组件响应不及时会产生该问题。需要监控主线程的耗时操作，例如数据库查询和网络请求都应通过后台线程处理。

## 35. 什么是 AIDL？

AIDL（Android 接口定义语言）用于跨进程通信（IPC），允许不同应用组件以类型安全的方式进行交互。它通过将对象分解为原始数据类型进行序列化传输，主要解决进程间无法直接共享内存的限制。典型用例包括音乐播放器的后台服务与控制界面的通信。

> [对比说明] 与 Messenger 不同，AIDL 支持并发方法调用和异步回调。但实现相对复杂，更适合需要高频通信的场景。

## 36. AIDL 支持哪些数据类型？

允许使用的数据类型包括：

- Java 基本类型（int/boolean 等）
- String 和 CharSequence
- List（元素需实现 Parcelable）
- Map（键值需是基本类型或可序列化对象）
- 实现了 Parcelable 接口的自定义对象

使用非基本类型时需要导入对应的 AIDL 接口声明。例如：`parcelable User;`

## 37. 什么是 Fragment？

Fragment 是 Activity 的模块化组成部分，具备自己的生命周期和界面布局。优点包括复用性强（例如同时适配手机和平板的多窗格布局）和动态组合能力。常见的 FragmentTransaction 操作包括添加/替换/移除：

```java
getSupportFragmentManager().beginTransaction()
    .replace(R.id.container, new DetailFragment())
    .addToBackStack(null)
    .commit();
```

## 38. 什么是可见 Activity？

当其他窗口（如对话框或透明 Activity）覆盖部分界面时，底层的 Activity 处于可见状态。虽然无法直接交互，但系统仍会保持其界面绘制。此时 Activity 处于 onPause() 状态，但未被完全停止（onStop()）。

## 39. 何时需要终止前台 Activity？

当系统面临极端内存压力时，可能回收前台进程。这种情况通常发生在设备整体内存即将耗尽的状态（例如运行占用大量内存的游戏）。系统会优先保留用户当前感知的界面，只有当其他回收方式无法释放足够资源时才会终止前台进程。

## 40. Fragment 能否不带用户界面？

是的，可以通过 add(Fragment, String) 方法添加无界面的 Headless Fragment。这种模式适合后台任务管理或数据持久化，例如在配置变更时通过 setRetainInstance(true) 保持长期运行的任务。

## 41. 如何移除 Android 主屏幕的图标和组件？

长按需要删除的图标/小组件直到设备振动，此时屏幕顶部或底部会出现垃圾桶图标/移除提示。将其拖放到该区域即可移除。不同厂商的 ROM 可能在动画效果或操作细节上有所差异，但基本逻辑保持一致。

## 42. Android 应用架构的核心组件有哪些？

关键组件构成包括：

- Activity：界面容器
- Service：后台服务
- Broadcast Receiver：系统事件监听
- Content Provider：数据共享接口
- Intent：组件间通信载体

这五类组件需在 manifest 文件中预先注册，并通过 Intent 进行协同工作。

## 43. 典型的 Android 应用项目由哪些部分组成？

APK（Android Package）文件包含：

- classes.dex：编译后的字节码
- AndroidManifest.xml：配置清单
- res/：编译后的资源文件
- resources.arsc：资源索引表
- native 库（可选）
- assets/ 原始文件（可选）

构建流程通过 Gradle 将这些元素打包，形成可安装的应用包。

## 44. 什么是粘性 Intent（Sticky Intent）？

通过 sendStickyBroadcast() 发送的广播会持续驻留系统，后续注册的 Receiver 仍能接收到最后发送的 Intent 副本。典型应用场景是电池状态监测，即使接收者注册时，也能立即获取当前电量信息。Android 5.0（API 21）后弃用该机制，推荐使用 JobScheduler 替代。

## 45. 所有手机都能升级到最新 Android 系统吗？

硬件支持和厂商策略两个因素决定能否升级。芯片组驱动适配需要时间，例如老款设备可能因 GPU 不兼容而无法使用 Vulkan 图形 API。此外，部分厂商为促进新机销售会停止旧机型系统更新。

## 46. 什么是便携式 Wi-Fi 热点？

该功能将手机的蜂窝网络数据通过 Wi-Fi 共享给其他设备，实质上是将手机转为无线路由器。典型场景包括：

- 无固网环境下的笔记本联网
- 多设备分享移动数据
需要注意运营商可能限制热点使用或计费方式不同。

## 47. Android 开发中的 Action 是什么？

作为 Intent 的核心属性，Action 定义了组件应当执行的操作类型。例如：

- ACTION_VIEW：显示数据
- ACTION_SEND：发送内容
- 自定义 Action 用于应用内部组件通信

开发者在 manifest 文件中通过 `<intent-filter>` 声明组件支持的动作类型。

## 48. 普通位图与九宫格（Nine-patch）图像有何区别？

九宫格图像通过 `.9.png` 扩展名标识，具有可拉伸标记区域：

1. 四角：保持原始比例不予拉伸
2. 边线：单轴拉伸保证边框不发生形变
3. 中心区域：全向拉伸填充空间

这种技术在按钮背景等需要适应不同尺寸的界面元素中特别有用，可以避免普通位图缩放导致的失真问题。

## 49. Android 应用开发主要支持哪种编程语言？

主要支持[Java 编程语言](https://www.guru99.com/java-tutorial.html)。Java 是移动应用开发领域最流行的语言，即使 Android 开发新手也能通过它快速学习如何在 Android 环境中创建和部署应用。

> [深入理解] 虽然 Kotlin 已成为 Android 开发的官方推荐语言，但该问题重点考察基础平台的初始技术选型认知。

## 50. Android 平台中与传感器使用相关的四个 Java 类是什么？请列举并解释其作用

与传感器交互的四个核心类包括：

- `Sensor` 类：提供识别传感器硬件能力的方法，例如确定设备是否配备特定类型的传感器。
- `SensorManager` 类：管理传感器系统的服务，用于注册监听器、校准设备以及获取传感器列表。
- `SensorEvent` 类：封装传感器事件的原始数据（如加速度计读数）和精度信息。
- `SensorEventListener` 接口：定义 `onSensorChanged()` 和 `onAccuracyChanged()` 回调方法，用于接收传感器数据更新。

> [技术细节] 实现传感器功能时通常需要先通过 `SensorManager.getSystemService()` 获取服务实例，再注册实现了该接口的监听器对象。完整实现流程可参考[官方传感器指南](http://developer.android.com/guide/topics/sensors/sensors_overview.html)。

## 51. `ContentProvider` 的定义和典型应用场景是什么？

`ContentProvider`（内容提供器）是管理跨进程结构化数据访问的核心组件。它通过 URI 机制封装数据并提供安全控制功能，典型场景包括：

- 跨应用共享私有数据库内容
- 集中管理文件或 SQLite 数据库的访问权限
- 为 `CursorAdapter` 提供标准数据接口以绑定到 `ListView` 等控件

> [实际案例] 系统内置的联系人信息和媒体库都是通过该组件实现数据共享的。开发时需继承 `ContentProvider` 类并重写 `query()`、`insert()` 等方法。详细创建方法参见[官方文档](http://developer.android.com/guide/topics/providers/content-providers.html)。

## 52. 以下代码片段可能导致应用崩溃的条件是什么？如何修复？

```java
Intent sendIntent = new Intent();
sendIntent.setAction(Intent.ACTION_SEND);
sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage); 
sendIntent.setType("text/plain");
startActivity(sendIntent); 
```

当设备中没有能处理 `ACTION_SEND` 隐式意图的应用时会触发崩溃。需要通过 `resolveActivity()` 做前置检查：

```java
if (sendIntent.resolveActivity(getPackageManager()) != null) {
    startActivity(sendIntent);
} else {
    Toast.makeText(this, "无可用应用", Toast.LENGTH_SHORT).show();
}
```

> [机制解析] 隐式意图通过 Intent Filter 匹配目标组件，`resolveActivity()` 会筛选符合 `<intent-filter>` 声明的应用。若多个应用匹配，系统会弹出选择器。显式意图通过 `setComponent()` 直接指定目标组件则不存在此问题。

## 53. 屏幕旋转后视图数据无法恢复，最基础的排查重点是什么？

必须确认视图是否通过 `android:id` 属性定义了有效 ID。Android 系统根据视图的 ID 进行状态保存与恢复，若未设置 ID，系统将跳过该视图的序列化过程。

> [延伸场景] 自定义视图需实现 `onSaveInstanceState()` 和 `onRestoreInstanceState()` 方法处理额外状态。若需完全禁用配置变更重建，可在 Manifest 中添加 `android:configChanges="orientation"` 属性。

## 54. 描述 Activity 生命周期中不触发 `onPause()` 和 `onStop()` 的场景

如果在 `onCreate()` 中直接调用 `finish()`，系统会跳过中间状态直接进入 `onDestroy()`。典型场景包括：

- 权限检查未通过时立即退出
- 初始化关键资源失败导致提前终止
- 安全机制拦截后主动销毁实例

> [资源管理建议] 应在 `onStart()` 中初始化资源并在 `onStop()` 中释放，而非依赖 `onDestroy()`。因为后台进程可能被系统直接终止而不会触发完整生命周期。

## 55. 检测设备是否配备指南针传感器的正确代码是哪段？解释原因

**正确答案是第一个使用 `PackageManager` 的代码段：**

```java
PackageManager m = getPackageManager();
if (!m.hasSystemFeature(PackageManager.FEATURE_SENSOR_COMPASS)) {
    // 关闭指南针功能
}
```

`SensorManager` 和 `Sensor` 类专精于传感器数据采集，而硬件特性检测应通过 `PackageManager` 完成。FEATURE 常量定义了标准硬件功能标识符，此方法比实际检测传感器列表更可靠。

> [适配策略] 若功能非必需，可在 Manifest 中添加 `<uses-feature android:name="android.hardware.sensor.compass" android:required="false"/>` 防止应用商店过滤设备。运行时再通过上述代码动态禁用相关功能模块。

## 56. 描述使用 `Intent` 的三个常见应用场景

> [考察内容] 该问题需要列举 Intent 的核心使用场景并简要说明  
对应答案段落：

`Intent` 常见的三种应用场景包括:

1. **启动 Activity**: 通过将 Intent 传递给 `startActivity()` 方法，可以启动新的 Activity 实例。例如点击按钮跳转到新页面
2. **启动 Service**: 通过传递 Intent 给 `startService()` 启动后台服务执行一次性操作（如下载文件）
3. **发送广播**: 通过 `sendBroadcast()` 等系列方法发送广播事件，可与应用程序内或系统级的接收器通信

> [补充说明] Android 官方文档提供了完整的[Intent 使用指南](https://developer.android.com/guide/components/intents-filters.html)

## 57. 在 Activity 中使用如下代码启动服务时，如果界面正在展示进度动画，可能会遇到什么问题以及如何解决？

```java
Intent service = new Intent(context, MyService.class);
startService(service);
```

对应答案段落：

当通过远程服务进行网络请求时，可能会遇到 **UI 线程阻塞导致动画冻结** 的问题。由于服务请求直接在 UI 线程执行，以下原因可能导致卡顿：

- 网络延迟
- 服务器响应缓慢
- 大数据量处理

> [解决方案] 可以通过两种方式避免:
>
> 1. 使用后台线程处理网络请求（推荐使用`AsyncTask`或`WorkerThread`）
> 2. 采用异步回调机制（如配合`Handler`或`LiveData`）

最后需要特别注意：现代 Android 版本会直接抛出 **NetworkOnMainThreadException** 导致应用崩溃

## 58. 什么是 DDMS？列举其主要功能

对应答案段落：

DDMS ([Dalvik 调试监控服务](http://developer.android.com/tools/debugging/ddms.html)) 是 Android SDK 中的调试工具，核心功能包含：

- 端口转发（Port-forwarding）用于调试
- 屏幕截图与视图层级捕获
- 线程堆栈监控与内存堆分析
- 实时网络流量跟踪
- 模拟来电/短信等系统事件
- 自定义网络状况模拟（时延/速度）
- GPS 位置信息模拟

> [深度理解] 该工具主要用于开发阶段的调试与性能分析

## 59. 解释 `AsyncTask` 生命周期与 `Activity` 的关系及潜在问题，如何规避？

对应答案段落：

`AsyncTask` 的生命周期 **不会** 自动绑定所属 Activity。典型问题场景：

1. **界面重建导致异常**: 当 Activity 因配置变更（如旋转屏幕）被销毁重建时，原 Activity 关联的 AsyncTask 完成操作后，尝试更新已失效的旧 Activity 视图会导致 `View not attached` 异常
2. **内存泄漏风险**: 持续运行的 AsyncTask 持有旧 Activity 的引用，阻碍垃圾回收

> [规避方案]:
>
> - 对耗时操作使用 `Service` + `IntentService`
> - 结合 `ViewModel` + `LiveData` 进行 UI 更新
> - 在 Activity 销毁时调用 `cancel(true)` 终止任务（需在代码中处理取消逻辑）

## 60. 什么是 `Intent`？能否用它向 `ContentProvider` 提供数据？为什么？

对应答案段落：

`Intent` 主要用于组件间通信及启动 Activity/Service/Broadcast，但 **不能直接与 ContentProvider 交互**。根本原因在于：

- ContentProvider 的访问必须通过 `ContentResolver` 接口
- `ContentResolver` 充当客户端与 ContentProvider 的中间层，通过 URI 定位 Provider 后执行 CRUD 操作

典型使用示例：

```java
Cursor cursor = getContentResolver().query(
    Uri.parse("content://com.example.provider/table"),
    projection,
    selection,
    selectionArgs,
    sortOrder
);
```

## 61. 简述 Fragment 和 Activity 的区别与关系

对应答案段落：

**Activity** 是 Android 最基本的界面容器，负责:

- 管理窗口生命周期
- 处理系统事件（如返回键）
- 协调应用与系统的交互

**Fragment** 则作为 Activity 的模块化 UI 单元，具备以下特性:

- 拥有独立的生命周期（但受宿主 Activity 状态约束）
- 支持动态添加/移除/替换
- 复用性强（同一 Fragment 可在多个 Activity 中使用）

> [典型配合方式]:
>
> - 手机界面：单 Activity + 全屏 Fragment
> - 平板界面：单 Activity + 多 Fragment 组合布局
> - 通过 `FragmentManager` 处理事务操作

## 62. 对比 Serializable 和 Parcelable 的区别及其在 Android 中的适用场景

对应答案段落：

两种序列化方案的对比：

| 特性            | Serializable         | Parcelable          |
| 实现复杂度       | 自动序列化（零编码） | 需手动实现读写方法  |
| 性能            | 低效（使用反射）     | 高效（显式操作）    |
| 应用场景        | 简单对象/持久化存储  | Android 组件间数据传输 |
| 内存开销        | 较高                | 极低                |

> [最佳实践]:
>
> - 优先选用 `Parcelable` 用于 Intent/Bundle 数据传输
> - 仅当需要本地序列化存储（如文件保存）时使用 `Serializable`

代码实现示例（Parcelable）：

```java
public class User implements Parcelable {
    private String name;
    
    protected User(Parcel in) {
        name = in.readString();
    }

    public void writeToParcel(Parcel dest, int flags) {
        dest.writeString(name);
    }

    public static final Creator<User> CREATOR = new Creator<User>() {
        //... 创建逻辑
    };
}
```

## 63. 什么是“启动模式（launch modes）”？可以通过哪两种机制定义？具体支持哪些类型？

启动模式决定了新启动的 Activity 实例如何与当前任务栈（Task）关联。两种定义机制分别是：

- **清单文件（Manifest）声明**：在 `<activity>` 标签中设置 `android:launchMode`。支持的类型包括：
  - `standard`（默认）：允许多实例共存于同一或不同任务栈
  - `singleTop`：栈顶已存在同类型实例时复用，触发`onNewIntent()`而非新建实例
  - `singleTask`：创建新任务栈作为根实例，全局唯一，已有实例通过`onNewIntent()`接收请求
  - `singleInstance`：专属任务栈且栈内仅允许该实例存在

- **Intent 标志位**：通过`startActivity(intent)`时添加标志位：
  - `FLAG_ACTIVITY_NEW_TASK`：等效`singleTask`
  - `FLAG_ACTIVITY_SINGLE_TOP`：等效`singleTop`
  - `FLAG_ACTIVITY_CLEAR_TOP`：销毁栈顶上方所有实例，目标实例通过`onNewIntent()`接收（无直接对应的清单模式）

> [考察内容] 理解两种机制的关系差异，需注意`FLAG_ACTIVITY_CLEAR_TOP`与清单方式没有直接对应项，以及复用时的回调方法差异。

## 64. `Service`和`IntentService` 的区别是什么？各自的使用场景是怎样的？

`Service`是 Android 服务的基类，运行时位于主线程，直接扩展它需自行处理耗时操作以避免阻塞 UI。适用于需长期运行但不涉及复杂异步逻辑的场景（如需保持后台音乐播放但手动管理线程）。

`IntentService`是`Service`的子类，自动创建工作线程处理`onHandleIntent()`中的异步请求。每次通过`startService()`发起请求会排队处理，任务完成后自动停止。典型应用场景是单次独立的耗时操作（如批量图片处理），其内部线程机制避免了主线程阻塞风险。

> [关键区别] 线程模型：IntentService内置工作线程，而普通Service需自行实现；生命周期管理：IntentService自动终止，Service需手动调用stopSelf()。

## 65. 如何向Fragment传递构造参数？

应通过`setArguments(Bundle)`方法传递参数，并在Fragment生命周期方法中通过`getArguments()`获取。示例：

```java
// 创建Fragment实例时
UserFragment fragment = new UserFragment();
Bundle args = new Bundle();
args.putString("user_id", "123");
fragment.setArguments(args);

// Fragment内部获取参数
public void onCreate(Bundle savedInstanceState) {
    String userId = getArguments().getString("user_id");
}
```

> [常见误区] 避免使用自定义构造函数传参，因为配置变化导致Fragment重建时会丢失参数。Bundle机制可保证参数序列化持久化。

## 66. 什么是Broadcast Receiver（广播接收器）？

用于监听系统或应用发出的全局事件（广播），典型用例包括：

- 响应网络状态变化（如连接WIFI时触发更新）
- 监听电量变化或充电状态
- 接收自定义应用内事件（如数据同步完成通知）

可通过清单文件静态注册或代码动态注册，结合`IntentFilter`指定监听的事件类型。

## 67. Fragment生命周期中哪个方法仅执行一次？

`onAttach()`方法在Fragment与Activity关联时调用一次，该阶段通常用于获取宿主Activity的引用。注意与`onCreateView()`的区分，后者在界面重建时可能多次触发。

## 68. 能否创建没有用户界面的Activity？如何实现？

可以，这类Activity称为无界面（Headless）Activity。实现方式为不在`onCreate()`中调用`setContentView()`：

```java
public class BackgroundActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        // 不设置布局
        startBackgroundWork();
    }
}
```

用途包括：

- 执行后台初始化任务
- 作为事件处理的中转站
- 跨应用通信的中间层

> [注意事项] 需添加`android:theme="@android:style/Theme.Translucent.NoTitleBar"`防止Window绘制残留。

## 69. 新建Android项目时主要包含哪些关键文件和目录？

项目结构核心部分：

- **src/**：Java/Kotlin源代码目录，按包结构组织
- **gen/**：自动生成的R类（资源索引），手动修改无效
- **Android SDK库**：版本相关的系统API依赖（如android.jar）
- **assets/**：原始资源文件（字体、预置数据库等），通过`AssetManager`访问
- **bin/**（旧版）：编译生成的APK文件存放目录（Android Studio中已弃用）
- **res/**：资源文件子目录：
  - drawable：图像资源
  - layout：界面XML布局
  - values：字符串、颜色等常量定义
  - menu：选项菜单配置

> [版本变化] Android Studio中部分目录结构调整，如编译输出改到build/目录，bin/逐渐淘汰。

## 70. 详细解释 AndroidManifest.xml 文件

对应答案段落（不使用任何列表符号）

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" 
          package="com.example.careerride" 
          android:versionCode="1" 
          android:versionName="1.0">
    <uses-sdk android:minSdkVersion="8" android:targetSdkVersion="18" />
    <application 
        android:allowBackup="true" 
        android:icon="@drawable/ic_launcher" 
        android:label="@string/app_name" 
        android:theme="@style/AppTheme">
        <activity 
            android:name="com.example.careerride.MainActivity" 
            android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
```

> [关键要素解析]  
AndroidManifest.xml 文件包含以下关键信息：  

- `package` 属性定义应用程序的唯一包名  
- `versionCode` 作为内部版本标识符，`versionName` 为用户可见版本号  
- `uses-sdk` 节点声明兼容的 API 级别范围（此处最低支持 SDK 8，目标 SDK 18）  
- `application` 元素定义全局属性：备份权限、应用图标（从 drawable 文件夹加载的 ic_launcher）、本地化应用名称（引用 strings.xml 中的 app_name）和主题样式  
- `activity` 声明主入口点，通过 `<intent-filter>` 配置为主启动器（LAUNCHER 类别结合 MAIN action）

## 71. 简要描述 Android Activity（活动）及其生命周期
>
> [核心概念]  
Activity 是 Android 应用与用户交互的核心组件，每个可视界面都对应一个 Activity。当在 Eclipse（现 Android Studio）中使用向导创建应用时，默认生成 MainActivity 继承自框架的 Activity 类。

**典型实现代码：**

```java
package com.example.careerride;
import android.os.Bundle;
import android.app.Activity;

public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
    
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }
}
```

> [生命周期重点]  
应用程序运行时首先触发 `onCreate()`，负责初始化界面（通过 `setContentView` 加载布局）和基础设置。其他重要生命周期方法包括：`onStart()`、`onResume()`、`onPause()`、`onStop()`、`onDestroy()`，共同构成 Activity 的状态流转过程，开发时需合理重写以管理资源。

## 72. 详解 Android Intent（意图）机制及其应用场景
>
> [基础原理]  
Intent 是 Android 组件间通信的核心机制，主要用于启动 Activity 或服务。分为显式（Explicit）和隐式（Implicit）两种类型。

**1. 显式 Intent 示例（指定明确目标组件）：**

```java
Intent intent = new Intent(this, SecondActivity.class);
startActivity(intent);
```

**2. 隐式 Intent 示例（通过 action 和 data 匹配）：**

```java
// 浏览网页
Intent webIntent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://www.amazon.com"));
startActivity(webIntent);

// 拨打电话（需权限）
Intent callIntent = new Intent(Intent.ACTION_CALL, Uri.parse("tel:+123456789"));
startActivity(callIntent);
```

> [权限配置提醒]  
执行敏感操作如拨号时，必须在 AndroidManifest.xml 添加对应权限声明：

```xml
<uses-permission android:name="android.permission.CALL_PHONE" />
```

## 73. 如何在 Android 中发送短信？附带示例说明

发送 SMS 的核心步骤涉及以下三个层面：

**1. 界面布局（activity_main.xml）：**

```xml
<Button
    android:id="@+id/btnSendSMS"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:onClick="sendmySMS"
    android:text="发送短信" />
```

**2. 权限声明（AndroidManifest.xml）：**

```xml
<uses-permission android:name="android.permission.SEND_SMS" />
```

**3. 发送逻辑（MainActivity.java）：**

```java
public void sendmySMS(View v) {
    SmsManager smsManager = SmsManager.getDefault();
    smsManager.sendTextMessage(
        "5556",         // 接收号码
        null,           // 短信中心地址（null 表示默认）
        "来自 careerRide 的问候",  // 短信内容
        null,           // 发送状态回调
        null            // 送达状态回调
    );
}
```

> [模拟测试提示]  
在模拟器环境中测试时，可通过同时运行两个模拟器（如 5554 和 5556）模拟设备间短信交互。注意真实设备需处理运行时权限（Android 6.0+）。

## 74. 解释 Android 中的 SmsManager 类及其核心方法
>
> [类作用]  
SmsManager 是系统 SMS 服务的核心控制器，需通过单例模式获取实例：

```java
SmsManager smsManager = SmsManager.getDefault();
```

**sendTextMessage() 参数详解：**

```java
smsManager.sendTextMessage(
    destinationAddress,  // 接收方号码（String）
    scAddress,           // 可选短信中心地址（通常填 null）
    textMessage,         // 短信内容
    sentIntent,          // 发送状态 PendingIntent（可空）
    deliveryIntent       // 送达状态 PendingIntent（可空）
);
```

> [高级应用]  
对于超长短信（超过 160 符），应使用 `divideMessage()` 分割配合 `sendMultipartTextMessage()` 发送。

## 75. 如何在应用中调用系统内置短信功能？

通过隐式 Intent 触发系统预装短信应用的示例：

```java
Intent smsIntent = new Intent(Intent.ACTION_VIEW);
smsIntent.putExtra("address", "5556;5558");  // 多个收件人用分号分隔
smsIntent.putExtra("sms_body", "朋友的问候！"); 
smsIntent.setType("vnd.android-dir/mms-sms");
startActivity(smsIntent);
```

> [功能扩展]  
此方法将打开系统默认短信应用并预填充内容和收件人，用户可手动确认发送。这种方式免去了处理短信发送权限的麻烦，适用于非后台发送需求的场景。但缺点是界面跳转可能破坏应用连贯性体验。

## 76. Android 中有哪些不同的数据存储选项？

Android 主要提供以下存储方案：  

- SharedPreferences（共享首选项）
- SQlite（关系型数据库）
- ContentProvider（内容提供器）  
- File Storage（文件存储）  
- Cloud Storage（云存储）

> [考察内容] 该问题主要检测对 Android 存储体系结构的全局认知  

## 77. 用示例说明 SharedPreferences 存储机制

这种采用 XML 文件存储键值对的方式适合保存简单配置。比如记住用户名、保存用户设置等场景。以下示例演示数据存取：  

**保存数据**（MainActivity 保存按钮点击事件）：

```java
// 获取私有模式的 SharedPreferences 对象
SharedPreferences sf = getSharedPreferences("MyData", MODE_PRIVATE);
SharedPreferences.Editor ed = sf.edit();
ed.putString("name", txtUsername.getText().toString()); // 用户名输入框
ed.putString("pass", txtPassword.getText().toString()); // 密码输入框
ed.apply(); // 异步提交更安全
```

**读取数据**（SecondActivity 加载按钮点击事件）：

```java
// 定义默认值常量防止空指针
public static final String DEFAULT = "N/A"; 

SharedPreferences sf = getSharedPreferences("MyData", Context.MODE_PRIVATE);
String name = sf.getString("name", DEFAULT);
String pass = sf.getString("pass", DEFAULT);

if (DEFAULT.equals(name) || DEFAULT.equals(pass)) {
    Toast.makeText(this, "未找到数据", Toast.LENGTH_LONG).show();
} else {
    txtUsername.setText(name);
    txtPassword.setText(pass);
}
```

> [注意点]  
>
> 1. MODE_PRIVATE 表示仅本应用可访问  
> 2. apply() 与 commit() 的区别在于前者异步写磁盘  
> 3. XML 文件路径为 /data/data/<包名>/shared_prefs/MyData.xml  

## 78. Android 架构的核心组件是什么？

Android 系统架构包含四层：  

1. **Linux 内核**：硬件抽象层，提供驱动程序  
2. **原生库**：包含 WebKit、OpenGL 等 C/C++ 库  
3. **应用框架**：提供 Activity Manager 等 API  
4. **应用层**：用户直接交互的 APP 程序  

## 79. Android 模拟器的核心优势有哪些？

- [开发效率] 无需物理设备即可模拟不同机型和系统版本  
- [安全性] 避免在真机上进行不稳定版本的早期测试  
- [调试能力] 支持内存监测、网络延迟模拟等高级调试功能  

> [补充] 可通过 AVD Manager 创建多种分辨率/API 级别的虚拟设备  

## 80. ActivityCreator 的作用是什么？

该工具用于快速生成 Android 项目的基础目录结构，包含以下关键文件：  

- AndroidManifest.xml 模板  
- build.gradle 配置雏形  
- 默认的 res/ 资源目录布局  
现已逐步被 Android Studio 的 Gradle 构建系统替代  

## 81. 显式 Intent 与隐式 Intent 的本质区别？

直接指明目标组件类的属于显式 Intent（如 `new Intent(this, TargetActivity.class)`），常用于应用内跳转。隐式 Intent 通过动作（ACTION_VIEW）和数据类型（如 image/*）匹配系统注册的组件，适合跨应用交互。

## 82. 布局文件为何使用 XML 格式？

XML 布局文件（如 activity_main.xml）的优势在于：  

- 视觉控件层级关系清晰易懂  
- 与业务逻辑代码解耦  
- 便于多屏幕尺寸适配（通过资源限定符）  
- Android Studio 提供可视化编辑器实时预览  

## 83. 容器控件的主要功能？

容器（如 LinearLayout、ConstraintLayout）负责管理子控件的排列方式。例如：  

- LinearLayout 通过 orientation 决定线性排列方向  
- FrameLayout 采用层叠式布局  
- RecyclerView 管理动态列表项的复用  

## 84. 如何设置 LinearLayout 排列方向？

通过 `android:orientation` 属性或 Java 代码设置：

```java
linearLayout.setOrientation(LinearLayout.HORIZONTAL); // 横向排列
// 或 XML 中设置 android:orientation="vertical"
```

垂直方向（VERTICAL）时子控件纵向堆叠，水平方向（HORIZONTAL）则横向排列。

## 85. 为什么应用需要权限声明？

权限系统保护用户隐私和设备安全。例如：  

- 请求 `INTERNET` 权限以访问网络  
- 需要 `READ_CONTACTS` 才能读取通讯录  
未声明权限会导致相关 API 调用抛出 SecurityException  

## 86. AIDL 的核心作用？

Android Interface Definition Language 用于跨进程通信（IPC）。典型场景：  

1. 定义服务端暴露的接口  
2. 客户端通过绑定服务获取代理对象  
3. 通过代理调用远程方法  

示例 aidl 文件：

```aidl
// IMyService.aidl
interface IMyService {
    int calculate(in int a, in int b);
}
```

## 87. 什么是九宫格图片？

- 四角区域不拉伸  
- 上下边缘控制水平拉伸区域  
- 左右边缘控制垂直拉伸区域  
常用于按钮背景等需要适配多尺寸的场景  

## 88. Android 支持哪些对话框类型？

主要有四种：  

1. **AlertDialog**：基础对话框，支持标题、消息、按钮和可选列表  
2. **ProgressDialog**：显示环形或条形进度（已弃用，推荐 ProgressBar）  
3. **DatePickerDialog**：日期选择器  
4. **TimePickerDialog**：时间选择器  

## 89. Dalvik 虚拟机的特点？

Dalvik VM 是 Android 4.4 前的默认运行时环境，特征包括：  

- 专为嵌入式系统设计，占用内存少  
- 执行 .dex 格式字节码（多个 .class 合并优化）  
- 采用寄存器架构而非 JVM 的栈结构  
ART（Android Runtime）在 5.0 后取代 Dalvik，引入 AOT 编译提升性能

## 90. Android新版本升级涉及哪些步骤？
>
> [考察内容] 该问题重点考察Android系统升级流程的体系化理解，需要展示对OTA升级流程各环节的熟悉程度

新版发布需要经历多重改进并构建完整的构建流程。关键步骤包括：

1) 初始阶段基于既有系统镜像构建基础版本，集成必要的安全认证、部署规范等基础配置
2) 版本进入运营商测试阶段，此环节对于发现和修复各类系统缺陷至关重要
3) 提交给监管部门审核，完成源代码发布前的法律协议签署与质量验证，涉及贡献者协议的合规性检查
4) 最终进入量产阶段，完成设备分发和OTA推送流程

## 91. Android兼容性的核心作用是什么？

兼容性确保不同厂商设备能正确运行第三方开发者基于Android SDK/NDK开发的应用程序。其运作特点包括：

- 通过严格认证机制区分兼容设备，通过兼容性测试的设备可获得Android商标授权
- 未通过兼容认证的设备只能使用开源版本，无法获得官方授权标识
- 兼容性框架为开发者提供统一的应用运行平台，同时保持安卓生态的开放性

## 92. Android应用支持哪些通信模式？

**主要通信技术方案包括：**

**1) SIP互联网电话协议**：通过完整的SIP协议栈实现语音通话功能，集成通话管理服务自动处理会话建立/维护，无需手动管理传输层连接。例如典型VOIP应用的实现：

```java
SipManager manager = SipManager.newInstance(this);
SipProfile.Builder builder = new SipProfile.Builder(username, domain);
SipProfile profile = builder.build();
SipSession session = manager.makeAudioCall(profile, null, null, 30);
```

**2) NFC近场通信**：支持设备间短距数据交换，广泛应用在移动支付、智能标签识别等场景。开发时可利用NfcAdapter处理Tag发现：

```kotlin
val adapter = NfcAdapter.getDefaultAdapter(context)
val intent = Intent(context, TagDispatchActivity::class.java).apply {
    flags = Intent.FLAG_ACTIVITY_SINGLE_TOP
}
val pendingIntent = PendingIntent.getActivity(context, 0, intent, 0)
val filters = arrayOf(IntentFilter(NfcAdapter.ACTION_NDEF_DISCOVERED))
```

## 93. Android的多媒体特性如何推动其市场普及？

三大核心多媒体功能支撑用户体验：

- **音频特效混合**：通过AudioEffect API提供均衡器、低音增强等专业级音效处理，支持多音轨实时混音
- **新型编码格式**：全面支持VP8开源视频编码，配合AAC/AMR音频编码实现高清视频录制与传输
- **多相机访问**：Camera2 API支持同时访问前后置摄像头，提供不同分辨率的图像流处理能力

> [技术细节] Camera2实现示例：

```java
CameraManager manager = (CameraManager) getSystemService(Context.CAMERA_SERVICE);
String[] cameraIds = manager.getCameraIdList();
CameraCharacteristics characteristics = manager.getCameraCharacteristics(cameraIds[0]);
StreamConfigurationMap configs = characteristics.get(
    CameraCharacteristics.SCALER_STREAM_CONFIGURATION_MAP);
```

## 94. Android中哪些服务可运行在单进程中？

默认情况下所有组件运行在主进程，但可通过配置实现进程隔离：

- 使用`android:process`属性显式声明组件运行进程（如":background"表示私有进程）
- Service本身并非独立进程，需明确指定才能跨进程运行
- UI线程与Service线程模型差异需要特别注意，建议通过Handler/AsyncTask实现线程间通信

## 95. Android服务的生命周期流程是怎样的？

完整生命周期管理包含以下关键节点：

1. **启动阶段**：通过`Context.startService()`触发->`onCreate()`初始化->`onStartCommand()`处理启动请求
2. **运行管理**：多次调用startService不会嵌套，通过`stopSelf()`或`Context.stopService()`终止服务
3. **终止条件**：必须处理完所有启动请求才会调用`onDestroy()`，可通过`stopSelf(int startId)`精确控制终止时机

> [使用规范] 切忌在onStartCommand中进行耗时操作，应该启动工作线程处理任务

## 96. Android服务有哪几种运行模式？

两种基础模式由`startCommand()`返回值决定：

**1) START_STICKY**：
系统会主动重启被异常终止的服务，适用于需要持续运行的核心服务。典型场景如音乐播放器后台运行：

```java
@Override
public int onStartCommand(Intent intent, int flags, int startId) {
    return START_STICKY;
}
```

**2) START_NOT_STICKY**：
仅在具备未处理指令时保持运行，适合执行一次性任务的场景。例如数据处理服务完成特定任务后自动终止

**绑定模式补充**：通过`Context.bindService()`建立持久连接，服务存活周期与绑定组件保持一致，需配合`ServiceConnection`接口实现双向通信

## 97. 为什么在 Android 中使用进程生命周期（Process Lifecycle）？
>
> [考察内容] 该问题主要考察对 Android 系统资源管理和进程优先级的理解。回答时需要结合实际场景说明生命周期各阶段的处理方法。

当应用程序托管服务（hosting services）时，Android 系统会维持这些进程存在，直到服务停止或客户端断开连接。系统在内存不足时会根据进程优先级决定终止顺序。核心生命周期流程如下：

- 前台运行的**服务进程**会执行`onCreate()`、`onStartCommand()`和`onDestroy()`方法，确保关键操作不被终止
- 已启动的非前台服务进程优先级低于用户可见的界面进程（visible processes），因为设备屏幕同时只能展示有限进程
- 客户端绑定的服务（bound services）在优先级上优于普通启动的服务
- 通过`startForeground(int, Notification)` API 可将服务提升为前台状态。系统仅会终止那些用户不再活跃使用的进程

## 98. Android 中的所有 Activity 如何运行在主线程？
>
> [深入理解] 此处需强调 UI 线程的重要性与阻塞风险，并说明线程管理的实现原理。需要展示系统如何维护线程池。

默认情况下，所有运行中的应用程序都在主线程（用户界面线程）中执行。例外情况是处理来自其他进程的 IPC 调用时的线程分配。系统采用以下机制：

- 为每个进程维护独立的事务线程池（transaction thread pools）用于分发传入的 IPC 调用
- 创建专用线程处理长时运行代码，避免阻塞主线程 UI 操作。例如网络请求或复杂计算应放在工作线程
- 内存不足时会终止后台服务进程，但可通过复写`onStartCommand()`让系统重启服务。典型的实现方式是使用`IntentService`

## 99. Android 中使用哪些数据类型传递数据？

**数据传输主要通过两种形式实现：**

1. **基本数据类型（Primitive Data Types）**：

```java
// 通过 Intent 传递简单数据
Intent.putExtra("key", value);
```

使用`Intent.putExtras()`在 Activity/Service 之间传递持久化数据。支持 int/boolean 等原生类型，适用于简单场景

2. **非持久化对象（Non-Persistent Objects）**：

- 用户自定义的临态复杂对象
- 适用于短期数据共享但会增加系统复杂性
- 通过 parcelable 或 serializable 实现跨进程传递

> [注意事项] 非持久化对象在进程被终止时会丢失，需结合持久化方案使用

## 100. Android 中共享对象的常用方法有哪些？

**主要实现方式包括三种模式：**

1. **单例模式（Singleton Class）**：

```java
public class Singleton {
    private static Singleton instance;
    
    public static synchronized Singleton getInstance() {
        if(instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

适用于同一进程内的组件，通过静态方法`getInstance()`保证全局唯一实例

2. **公共静态字段/方法（Public Static Field/Method）**：

```java
public class SharedData {
    public static Object sharedObject;
}
```

其他组件通过`SharedData.sharedObject`直接访问字段。需注意内存泄漏风险

3. **弱引用哈希表（WeakReference HashMap）**  
使用长整型键值维护对象映射关系，当内存不足时允许自动回收：

```java
HashMap<Long, WeakReference<Object>> objMap = new HashMap<>();
```

## 101. 如何共享持久化用户自定义对象？
>
> [实现要点] 需要重点说明数据持久化与恢复机制，区分不同存储方案的适用场景

需采用持久化存储方案确保进程终止后数据可恢复：

- **应用偏好设置（SharedPreferences）**：存储键值对形式的用户配置
- **文件存储（File Storage）**：设置文件权限实现跨组件共享。适用于结构化文档
- **内容提供者（Content Providers）**：通过标准化接口共享结构化数据，支持跨应用访问
- **数据库（SQLite Database）**：使用 Room 等 ORM 框架管理结构化数据，支持复杂查询

## 102. 如何检测 Android 中 Activity 的状态？

状态检测主要涉及两个核心生命周期状态：

1. 运行状态（Started）：执行`onStart()`后的可视化状态
2. 停止状态（Stopped）：`onStop()`触发时的非活跃状态

检测方法：

```java
// 使用 NEW_TASK_LAUNCH 标志确保追踪完整 Activity 栈
Intent intent = new Intent(context, TargetActivity.class);
intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
startActivity(intent);
```

远程服务（Remote Services）可借助`bindService()`实现跨进程 Activity 状态监控

## 103. 编写实现包添加与移除检测的代码？
>
> [代码要求] 必须包含 BroadcastReceiver 的 XML 配置和基本实现逻辑

通过监听系统广播实现安装/卸载包检测：

```xml
<!-- AndroidManifest.xml 配置 -->
<receiver android:name=".PackageReceiver">
    <intent-filter>
        <action android:name="android.intent.action.PACKAGE_ADDED"/>
        <action android:name="android.intent.action.PACKAGE_REMOVED"/>
        <data android:scheme="package"/>
    </intent-filter>
</receiver>
```

```java
public class PackageReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context ctx, Intent intent) {
        Uri packageUri = intent.getData();
        String packageName = packageUri.getSchemeSpecificPart();
        
        if(Intent.ACTION_PACKAGE_ADDED.equals(intent.getAction())) {
            // 处理包安装逻辑
        } else if(Intent.ACTION_PACKAGE_REMOVED.equals(intent.getAction())) {
            // 处理包卸载逻辑  
        }
    }
}
```

## 104. Android 采取了哪些安全措施来确保系统安全性？

1) Android 采用了多层防护机制抵御黑客攻击。这些措施包括设备硬件层面的改造和软件服务层的安全增强。  
2) 应用程序通过 sandbox（沙盒）机制运行，对敏感信息的访问权限进行严格控制。例如应用间数据隔离确保隐私安全。  
3) 提供细粒度权限管理体系，用户可自主控制应用对数据的访问级别。消息加密技术有效保障通信内容安全性。  
4) 系统内置用户协议约束机制，默认限制未知来源应用安装。开源特性虽存在隐患，但通过快速迭代的漏洞修复机制持续提升安全等级。  

> [考察重点] 此处需要展示对安全架构的系统性理解，包括运行时隔离、权限控制、加密机制等核心要素的掌握程度。

## 105. 创建可重用 UI 布局需要哪些关键 XML 标签？

Android 提供多种 UI 控件用于构建可复用组件，实现方式主要通过 XML 布局映射到类实例：  
\- `<requestFocus />`：强制组件获得初始焦点  
\- `<merge />`：合并多个控件层级消除冗余视图  

- `<include />`：嵌入外部布局文件实现组件复用  

代码示例说明：  

```xml
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <Button android:id="@+id/confirm_btn"/>
    <include layout="@layout/toolbar_template"/>
</merge>
```

> [场景应用] 当需要重复使用复杂布局时，结合 merge 和 include 可显著优化布局层级结构。开发者需注意被 include 布局的根元素兼容性。

## 106. 如何编写实现布局复用的界面代码？

典型实现通过 `<include>` 标签嵌入重复布局模块：  

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <include
        android:id="@+id/header_section"
        layout="@layout/common_header"
        android:layout_margin="16dp"/>

    <include 
        layout="@layout/content_divider"
        android:visibility="gone"/>
        
</LinearLayout>
```

> [实现要点] 通过 @layout/ 引用资源时，被包含布局的根元素属性可能被当前布局的容器属性覆盖。建议给 include 设置明确 ID 以进行动态控制。

## 107. 如何有效避免 Android 中的内存泄漏？

核心策略围绕上下文生命周期管理：  

1. **上下文类型选择**：优先使用 Application Context 访问全局资源

```java
// 正确方式
TextView tv = new TextView(getApplicationContext());
```

2. **静态引用处理**：避免在静态变量中持有 Activity 引用  
3. **监听器解绑**：在 onDestroy() 中注销系统服务监听  
4. **匿名类处理**：使用 WeakReference 包装非静态内部类引用  

> [典型案例分析] 屏幕旋转时未正确释放 Activity 实例可能导致连续内存泄漏，可通过 LeakCanary 等工具监测。注意 Handler 延迟消息的及时清理。

## 108. 解决上下文相关内存泄漏的具体步骤？

1. **生命周期对齐**：确保长生命周期对象不持有 Activity 引用  
2. **弱引用应用**：对必须持有的 Activity 引用使用 WeakReference 包装  
3. **静态类优化**：将内部类声明为 static 并移除外部类强引用  
4. **资源释放验证**：在 onDestroy() 中主动置空 View 监听器及 Bitmap 引用  
5. **诊断工具使用**：结合 Android Profiler 进行 Heap Dump 分析  

```java
// 正确实现示例
private static class SafeHandler extends Handler {
    private final WeakReference<Activity> mActivity;
    
    public SafeHandler(Activity activity) {
        mActivity = new WeakReference<>(activity);
    }
    
    @Override 
    public void handleMessage(Message msg) {
        Activity activity = mActivity.get();
        if (activity != null && !activity.isDestroyed()) {
            // 处理消息
        }
    }
}
```

## 109. 设置 Linkify 调用 Intent 的关键流程？

实现自动链接识别与 Intent 调用的完整流程：  

1. **视图关联**：对 TextView 应用 Linkify 模式匹配  

```java
TextView tv = findViewById(R.id.phone_view);
Linkify.addLinks(tv, Linkify.PHONE_NUMBERS);
```

2. **协议映射**：将匹配内容转换为可点击的 Intent URI（如 tel:）  
3. **意图过滤**：系统自动寻找声明了匹配 VIEW action 和 data scheme 的组件  
4. **内容处理**：通过注册的 ContentProvider 解析 MIME 类型  
5. **安全验证**：确保跳转目标 Activity 具有正确权限声明  

> [扩展应用] 自定义 Linkify 匹配规则可使用 Pattern 配合 TransformFilter：  

```java
Linkify.addLinks(textView, myPattern, "myapp://",
    (match, group) -> Uri.encode(group));
```

## 110. 什么是清单文件（manifest file），它的作用是什么？

每个 Android 应用都必须在根目录下包含名为 `AndroidManifest.xml` 的清单文件。该文件存放应用的关键信息，包括 Java 包名、组件声明（Activity/Service 等）、权限配置和最低 SDK 版本要求等核心设置。

> [考察内容] 通常会追问清单文件第一个元素。在编码声明后的首个子元素是 `<manifest>`，如果面试官限定该标签内部的首元素时，`<uses-permission>` 是最常见答案之一。注意确认问题所指的层级范围

## 111. Android 中有哪些数据存储方式？（最少列举4种）

以下是五种常用方式的任意组合：

1. SharedPreferences（键值对存储）
2. Internal Storage（内部存储，应用私有空间）
3. External Storage（外部存储，如 SD 卡公共空间）
4. SQLite Database（嵌入式数据库）
5. Network Connection（云端或远程服务器存储）

## 112. Android 项目的必要组成部分有哪些？（至少列举4项）

每个项目必须包含以下六个核心要素中的至少四个：

1. `AndroidManifest.xml`（清单文件）
2. `build.gradle`（构建脚本，旧项目可能仍有 `build.xml`）
3. `src/`（源代码目录）
4. `res/`（资源文件目录，含布局/图形等）
5. `assets/`（原始资源目录）
6. 自动生成的编译输出目录（现代项目多为 `build/`，早期可能为 `bin/`）

## 113. 如何避免 ANR？

关键在于减少主线程负载。典型策略包括：

- 异步处理网络请求与数据库操作（使用 AsyncTask/WorkManager）
- 复杂计算移至后台线程（Kotlin 协程 / RxJava）
- 优化布局性能避免 UI 渲染卡顿
- 谨慎使用 BroadcastReceiver 的 onReceive 方法（10秒限制）

> [实现示例] 使用 Thread + Handler 的典型模式：

```java
new Thread(() -> {
    // 后台操作
    Object result = doHeavyWork(); 
    handler.post(() -> updateUI(result)); 
}).start();
```

## 114. 容器（container）在 Android 中的作用是什么？

容器是用于组织界面元素的布局控件，管理内部子视图的排列方式。常见容器类型包括：

- LinearLayout（线性布局）
- RelativeLayout（相对布局）
- FrameLayout（帧布局，常用于碎片）
- ConstraintLayout（约束布局）
它们可嵌套组合以实现复杂 UI 结构，例如在 RecyclerView 的列表项中混合使用多种容器

## 115. Ice Cream Sandwich 和 KitKat 这两个版本的主要区别是什么？

这两个代号分别对应：

- Ice Cream Sandwich（4.0，API 14）：2011年发布，带来 Holo UI 设计风格和硬件加速优化
- KitKat（4.4，API 19）：2013年发布，引入 translucent system UI 和 ART 运行时（测试阶段）

> [深入理解] 此题主要考察对 Android 版本历史演进的熟悉程度。开发者应了解重大 API 变化，例如 4.0 引入的 Fragment，4.4 的 Storage Access Framework 文件存储方案调整

## 116. 什么是应用小部件（App Widget）？

App Widget 是嵌入在主屏/锁屏的微型视图组件，通过定期刷新显示关键信息。典型应用场景包括：

1. 天气实时更新（如每30分钟获取新数据）
2. 音乐播放控制（显示当前曲目和进度）
3. 日历日程预览（点击跳转到完整应用）
需通过 AppWidgetProvider 类实现，遵循特定的更新机制（不可直接执行长时间后台任务）

## 117. AIDL 的基本概念是什么？

AIDL（Android Interface Definition Language，安卓接口定义语言）用于定义跨进程通信（IPC）的接口规范。其核心流程包括：

1. 创建 `.aidl` 文件声明接口方法
2. 实现 Service 端的 Stub 类
3. 客户端通过 binder 代理对象调用远程方法
典型场景如音乐播放器中服务端与通知栏组件的通信

> [实现要点] 需要特别注意线程安全问题——AIDL 方法默认非线程安全，需用 `synchronized` 关键字保护共享数据

## 118. 开发客户 Android 应用前需要哪些基础信息？

需求收集应涵盖以下关键点：

1. **核心功能说明**（实现什么业务目标）
2. **目标用户画像**（设备适配要求/交互习惯）
3. **竞品分析**（差异化和合规性考量）
4. **UI/UX 规范**（设计稿/动效规范/品牌指南）
5. **技术约束**（后端 API 对接方式/遗留系统集成）
重点在于确认视觉素材已定稿，避免开发中途频繁修改布局导致代码重构
