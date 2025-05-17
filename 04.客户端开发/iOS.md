# iOS

[TOC]

## 1. 在 `UITableViewCell` 的构造方法中，`reuseIdentifier` 的用途及优势

```objc
- (instancetype)initWithStyle:(UITableViewCellStyle)style
              reuseIdentifier:(NSString *)reuseIdentifier;
```

* **用途**：`reuseIdentifier` 将具有相同布局的行分组，允许 `UITableView` 重用已创建的 `UITableViewCell` 对象。
* **优势**：避免每次滚动都创建新单元格，显著提高滚动性能，减少内存分配与释放，防止卡顿。

## 2. 在 `UIView` 中指定元素布局的几种方式

1. **Interface Builder**：使用 Storyboards 或 XIB 可视化拖拽布局。
2. **Auto Layout**：通过 `NSLayoutConstraint`、布局锚点（Layout Anchors）或可视化格式语言（VFL）定义约束。
3. **Frame‑based Layout**：直接设置视图的 `frame`（`CGRect`）属性。
4. **SwiftUI**（iOS 13+）：使用声明式视图库，通过修饰符描述布局。

## 3. `atomic` 与 `nonatomic` 属性区别

* **atomic**（默认）

  * 保证在多线程环境下，setter/getter 不会读到半初始化的值。
  * 开销较大，性能略低。
* **nonatomic**

  * 不保证原子性，多线程同时访问可能出现竞态。
  * 性能开销小，通常用于 UI 属性或已由其他同步机制保护的场景。

## 4. 全局 `NSString *startTime;` 与 `+initialize` 的执行时机

```objc
// 在 MyClass.h
extern NSString *startTime;

// 在 MyClass.m
NSString *startTime = nil;
+ (void)initialize {
    if (self == [MyClass class]) {
        startTime = [NSDate date].description;
    }
}
```

* **运行结果**：在 `application:didFinishLaunchingWithOptions:` 中打印 `startTime` 会输出实际的时间字符串（不是 `(null)`）。
* **原因**：Runtime 在向 `MyClass` 发送第一条消息（如 `+initialize`）前会自动触发 `+initialize`。`application:didFinishLaunchingWithOptions:` 由系统发送给 AppDelegate，本身会触发 AppDelegate 类及其父类的 `+initialize`。
* **替代方案**：如需更早初始化，可使用 `+load`；若需线程安全一次性初始化，推荐使用 `dispatch_once`。

在实际项目中，更可靠的做法是直接在 +load 或 dispatch_once 中初始化，或者在使用前显式调用某个类方法来触发初始化。

## 5. 断开 `TTParent` 与 `TTChild` 的保留环

```objc
@interface TTParent : NSObject
@property (nonatomic, strong) NSMutableArray<TTChild *> *children;
@end

@interface TTChild : NSObject
@property (nonatomic, weak) TTParent *parent;  // 改为 weak
@end
```

* **问题**：若 `parent` 也是 strong，会导致 retain cycle，内存无法释放。
* **修复**：将子对象对父对象的引用声明为 `weak`，打破循环。

## 6. 在 `NSOperationQueue` 块中更新 UI

```objc
[backgroundQueue addOperationWithBlock:^{
    // 后台耗时操作
    [NSThread sleepForTimeInterval:10];

    // UI 更新必须在主线程执行
    [[NSOperationQueue mainQueue] addOperationWithBlock:^{
        self.alertLabel.text = @"Thanks!";
    }];
}];
```

* **错误**：直接在后台队列中操作 UI；
* **修复**：使用 `NSOperationQueue mainQueue` 或 `dispatch_async(dispatch_get_main_queue(), ^{ … })`。

## 7. “Bundle ID” 与 “App ID” 区别

* **Bundle ID**：在 Xcode 项目中设置，唯一标识一个应用（如 `com.company.app`），用于安装、推送等。
* **App ID**：Apple Developer Portal 上配置，由 Team ID + Bundle ID 搜索字符串 组成，用于启用推送、应用组、In‑App Purchase 等服务，可包含通配符（如 `com.company.*`）。

## 8. `strong` 与 `weak` 引用

* **strong**：默认，持有对象并增加其引用计数。
* **weak**：不增加计数，对象被释放时自动置为 `nil`。
* **用途**：使用 `weak` 打破 retain cycle；常见于 delegate、parent→child 关系等。

## 9. `NSManagedObjectContext`（托管对象上下文）

* **作用**：Core Data 中用于管理 `NSManagedObject` 实例的“工作区”，跟踪更改并协调将数据保存到持久存储。
* **功能**：

  * 对象增删改查（Fetch Requests、insert、delete）。
  * 变更跟踪、撤销/重做。
  * 验证属性合法性。
  * 维护对象关系。
  * 支持多种并发类型（`mainQueueConcurrencyType`、`privateQueueConcurrencyType`）。

## 10. iOS/OS X 中的并发方式

1. **NSThread**：最原始的线程管理，手动创建、同步，易错。
2. **GCD (Grand Central Dispatch)**：C 语言接口，系统管理线程池，提交代码块到串行或并行队列。
3. **NSOperationQueue**：基于 GCD 的 Cocoa 抽象，支持操作依赖、优先级、取消和 KVO 通知。

> 现代开发推荐 GCD 或 Operation Queues，几乎不直接使用 NSThread。

## 11. 用 `==` 比较两个 `NSString` 会输出什么？

```objc
NSString *a = @"testuser";
NSString *b = @"testuser";
if (a == b) {
    NSLog(@"areEqual");
} else {
    NSLog(@"areNotEqual");
}
```

* **输出**：`areEqual`
* **原因**：编译器常量池重用了相同字符串字面量，`a` 与 `b` 指向同一对象。
* **正确内容比较**：使用 `[a isEqualToString:b]`。

## 12. iOS 应用状态

* **Not running**：未启动或已被系统终止。
* **Inactive**：在前台但暂不接收事件（如来电界面）。
* **Active**：前台并接收事件。
* **Background**：在后台执行代码（短暂任务、位置更新等）。
* **Suspended**：在后台驻留内存，不执行代码，系统可随时终止。

## 13. `willFinishLaunchingWithOptions:` vs. `didFinishLaunchingWithOptions:`

* **`willFinishLaunchingWithOptions:`**：应用启动后首个可执行自定义代码的机会，适用于轻量级初始化。
* **`didFinishLaunchingWithOptions:`**：在上述方法后调用，用于完成窗口和根控制器的设置，为用户界面显示做准备。

## 14. `JSONSerialization` 的读写选项

* **写入选项** (`JSONSerialization.WritingOptions`)：

  * `prettyPrinted`：格式化缩进。
  * `sortedKeys`（iOS 11+）：按键排序。
  * `withoutEscapingSlashes`（iOS 13+）：不转义 `/`。
* **解析选项** (`JSONSerialization.ReadingOptions`)：

  * `mutableContainers`、`mutableLeaves`、`allowFragments`。

## 15. Swift 值类型 vs 引用类型

* **值类型**（`struct`、`enum`、`Array`、`Dictionary`、`String` 等）：赋值/传递时会复制一份独立副本。
* **引用类型**（`class`、函数闭包）：赋值/传递时传递引用，多个变量指向同一实例。

## 16. Swift Optional

* **定义**：可包含值或 `nil` 的类型，显式表明可缺失值。
* **优势**：避免空指针，编译时强制解包（`if let`、`guard let`），提高安全性和可读性。

## 17. Swift `guard` 语句

* **用途**：验证前置条件，条件不满足时 `else` 块中必须提前退出（`return`、`break`、`throw` 等）。
* **优势**：减少嵌套层级，使主流程更清晰；可选绑定后的变量在后续作用域中可用。

## 18. Swift `weak` vs `unowned`

* **weak**：必须为 Optional，引用的对象释放后自动置为 `nil`；适用于被引用对象可能先于引用结束的场景。
* **unowned**：非 Optional，假定被引用对象生命周期不短于引用者；访问已释放对象将崩溃；适用于生命周期相同或更长的场景。

## 19. Objective‑C 快速枚举

* 使用 `for (Type obj in collection)` 语法遍历集合，比传统索引或枚举器更高效，内部做了优化，减少方法调用。

## 20. Category vs Extension

* **Category**：为现有类（系统或自定义）添加方法，无需源代码；**不能**添加实例变量。
* **Class Extension**：需拥有源代码，可在 `.m` 中声明私有方法或属性；不能在不修改原始 @interface 的情况下添加实例变量（可通过关联对象实现私有存储）。

## 21. `copy` vs `retain`（ARC 下为 `strong`）

* **retain/strong**：指向同一对象，并增加引用计数。
* **copy**：复制一份新对象并指向它，避免外部可变对象修改影响到属性值。常用于 `NSString`、`NSArray` 等。
