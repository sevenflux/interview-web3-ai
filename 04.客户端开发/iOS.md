# iOS

[TOC]

## 1. 为什么Swift语言被命名为Swift？

名字来源于其两大核心设计目标：1) 编写代码迅速（swift to code），语法简洁易读，适合快速开发；2) 执行速度迅捷（swift to execute），采用LLVM编译器(连接和优化编译器技术链)，能针对不同设备生成优化后的本地代码。

> [深入理解] 这两个核心目标体现了Swift在开发效率和运行性能上的平衡设计，开发者既能快速上手，又能构建高性能应用

## 2. 应该学习Swift还是Objective-C？

Swift是苹果明确指出的未来iOS开发基石。它与Objective-C文件完全兼容，现有库和代码均可复用。随着时间推移，Swift在工具链支持和社区资源方面已形成显著优势。

## 3. Swift容易掌握吗？

苹果官方将其定位为"兼具脚本语言表达力和工业级系统编程品质"的语言。新学者友好的语法设计使其学习曲线平缓，甚至被称为[新时代的BASIC语言基础工具](http://www.zdnet.com/why-apples-swift-might-be-the-new-basic-and-thats-no-small-thing-7000030186/)。

## 4. Swift的运行速度表现如何？

苹果实测数据显示：相比Objective-C快2.6倍，比Python 2.7快8.4倍。这种性能优势直接转换为应用流畅度提升——更快的代码执行意味着更高效的资源利用与更丝滑的用户体验。

## 5. 苹果为何要创建Swift新语言？

过去20年间移动开发范式发生剧变，原有Objective-C在现代化编程特性上显露疲态。创建专为苹果硬件优化的新语言，比改造现有语言更高效，能更好地发挥硬件性能与开发潜力。

## 6. 能用Swift开发并上架App Store吗？

苹果在工具链层面提供完整支持，开发者可完全基于Swift构建符合审核标准的商业级应用。

## 7. 为何需要从Objective-C转向Swift？

主要推动力来自两方面：Objective-C语言本身的时代局限性（特别是对新开发者的高门槛），以及与现代化编程实践脱节。Swift专为苹果平台定制，提供了类型安全、内存管理等现代语言特性。

## 8. Swift的稳定性如何？

目前已经相当稳定。

1.0版本已解决大部分早期问题，但需注意版本升级时的语法变化。例如1.1引入的[failable initializers可失败初始化器](https://developer.apple.com/swift/blog/?id=17)就改变了对象构造的方式。建议持续关注[官方修订日志](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/RevisionHistory.html#//apple_ref/doc/uid/TP40014097-CH40-XID_1655)来应对API演进。

## 9. Swift与哪些语言相似？

语法风格接近Ruby或Python的优雅简练，同时保留C族语言的某些结构特征。这样的设计既吸引脚本语言开发者，也方便传统系统程序员过渡。

## 10. Swift与Objective-C如何实现互操作？

二者通过运行时桥接实现互操作，但并非所有Objective-C特性都可在Swift中使用。关键在于理解动态派发（Dynamic Dispatch）机制：Objective-C运行时在调用方法时动态选择具体实现（如子类覆写父类方法），这种灵活性实现了诸多高级特性。

```swift
class Parent {
    func method() { print("Parent") }
}
class Child: Parent {
    override func method() { print("Child") }
}
// 运行时决定调用Child的method实现
```

## 11. dynamic关键字有什么作用？

通过`dynamic`修饰的类成员将启用动态派发机制，这会将方法调用转交给Objective-C运行时处理。同时隐式添加`@objc`属性使其对Objective-C可见，这是使用KVO(键值观察)或Core Data等依赖运行时特性时的必备条件。

> [关键原理] Swift默认优先使用静态派发(编译时确定方法地址)和虚表派发(通过类虚函数表查找)，这两种方式比动态派发快几个纳秒级。但当需要Objective-C运行时特性时，必须强制启用动态派发机制

## 12. 为何dynamic只能修饰类成员？

结构体(struct)和枚举(enum)不支持继承，不存在运行时选择方法实现的需求。类的继承体系才需要动态派发机制，这是运行时多态性的基础支撑。

```swift
class ObservableObject {
    dynamic var value: Int = 0 // 启用KVO必须添加dynamic
}
struct Point {
    // dynamic var x: Int = 0  // 编译错误：仅类成员可用
}
```

## 13. 何时应该使用动态派发（Dynamic Dispatch）？

当需要利用 Objective-C 的动态特性时，必须使用 `dynamic` 声明修饰符。例如需要启用 **键值观察（Key-Value Observing）** 时，需为 `NSObject` 子类的属性添加 `dynamic` 修饰符以确保运行时动态派发生效。

> [考察内容] 该问题旨在理解动态派发在特定技术场景（如 KVO）中的应用必要性。Objective-C 的运行时机制依赖动态派发来实现灵活的消息处理。

## 14. 分析以下 Swift 数组代码的运行结果

```
var array1 = [1, 2, 3, 4, 5]
var array2 = array1
array2.append(6)
var len = array1.count
```

变量 `len` 的值为 5。数组是值类型（结构体实现），`array2 = array1` 会创建副本，修改副本不会影响原数组：

```
array1 保持为 [1, 2, 3, 4, 5]
array2 变为 [1, 2, 3, 4, 5, 6]
```

> [技术细节] Swift 的值类型（如结构体、枚举、基础类型）在赋值或传递时创建拷贝。字典与数组采用相同机制，这种设计确保了数据独立性。

## 15. 找出以下类型转换代码的错误并修正

```
let op1: Int = 1
let op2: UInt = 2
let op3: Double = 3.34
var result = op1 + op2 + op3
```

编译器报错源于类型不匹配。Swift 要求显式类型转换：

```swift
var result = Double(op1) + Double(op2) + op3 // 统一转为 Double
```

> [注意点] Swift 严格禁止隐式类型转换，即使逻辑上兼容的类型（如 Int 与 UInt）也需要显式处理。该原则确保了类型安全。

## 16. 找出以下 NSUserDefaults 代码的潜在崩溃风险

```
var defaults = NSUserDefaults.standardUserDefaults()
var userPref = defaults.stringForKey("userPref")!
printString(userPref)
```

第二行的强制解包 `!` 是风险点：当键不存在或值无法转换为字符串时，解包 nil 会导致崩溃：

```
// 错误示例：键"userPref"不存在时会触发 fatalError
```

> [解决方案] 使用可选绑定安全处理：

```swift
if let userPref = defaults.stringForKey("userPref") {
    printString(userPref)
}
```

## 17. Swift 中 countElements 函数处理字符串的时间复杂度

`countElements` 的时间复杂度为 **O(n)**。Swift 字符串基于扩展字符簇（Extended Grapheme Clusters）存储：

> [技术解释] 字符可能由多个 Unicode 标量组成（如表情符号 🇨🇳 需要多码位），需遍历整个字符串确定字符边界。因此长度计算无法优化为常数时间。

## 18. 区分 Swift 枚举的原始值（Raw Values）与关联值（Associated Values）

**原始值**：

- 预定义固定字面量，类型统一声明
- 案例必须唯一，编译时确定

```swift
enum Direction: Int {
    case up = 1
    case down = 2 // 原始值类型为 Int
}
```

**关联值**：

- 运行时动态附加数据，案例可携带不同数据类型
- 每个实例可存储不同内容

```swift
enum NetworkResponse {
    case success(data: Data) // 关联值类型随实例变化  
    case failure(code: Int)
}
```

## 19. 解释 Swift 中值类型能被添加到 AnyObject 数组的现象

```swift
var array = [AnyObject]()
array.append(1) // Int 被桥接为 NSNumber
array.append("text") // String 桥接为 NSString
```

> [桥接机制] Swift 基础值类型在与 Cocoa 交互时自动转换为对应的 Foundation 引用类型：

- `Int`/`Double` → `NSNumber`
- `String` → `NSString`
- `Array` → `NSArray`
- 用户自定义结构体无此桥接，故 `Test` 结构体会报类型错误。

## 20. 观察以下代码，指出其中的内存问题及其影响，并提出解决方案

```
class Master {
    lazy var detail: Detail = Detail(master: self)

    init() {
        println("Master init")
    }

    deinit {
        println("Master deinit")
    }
}

class Detail {
    var master: Master

    init(master: Master) {
        println("Detail init")
        self.master = master
    }

    deinit {
        println("Detail deinit")
    }
}

func createMaster() {
    var master: Master = Master()
    var detail = master.detail
}

createMaster()

```

对应答案段落：
>`Master`与`Detail`之间存在_强引用循环（strong reference cycle）_：`Master`实例创建并持有一个`Detail`实例的强引用，而`Detail`反过来也强引用其父`Master`。两个对象的引用计数永远不会归零，导致内存泄漏。

解决方案是使用`weak`或`unowned`修饰符打破至少一个强引用关系。两种修饰符的区别在于：

- `unowned`：假定引用对象始终存在，属性必须为非可选类型
- `weak`：允许引用对象变为`nil`，属性必须为可选类型

在此场景下，最适合将`Detail`中的`master`属性标记为`unowned`：

```
class Detail {
    unowned var master: Master
    ...
}
```

> [深入理解] 此处考察引用循环的识别与解决能力，需要展示对ARC内存管理机制的理解。使用`unowned`比`weak`更合适，因为`Detail`的生命周期完全受其所属的`Master`控制。

## 21. 下列代码片段会导致编译错误，解释原因及修复方法

```
struct IntStack {
  var items = [Int]()
  func add(x: Int) {
    items.append(x) // 此处编译错误
  }
}

```

对应答案段落：
结构体是值类型，其实例方法默认不可修改属性。需要在方法前添加`mutating`关键字：

```
struct IntStack {
  var items = [Int]()
  mutating func add(x: Int) {
    items.append(x) // 正确
  }
}
```

> [考察内容] 此处需要理解值类型与变异方法的语义差异。Swift通过`mutating`关键字显式声明会修改结构体状态的方法，保证值类型在不可变上下文中的安全性。

## 22. 给定以下代码，分析变量`x`的类型和值

```swift
let d = ["john": 23, "james": 24, "vincent": 34, "louis": 29]
let x = d.sort{ $0.1 < $1.1 }.map{ $0.0 }
```

对应答案段落：
`x`的类型为`[String]`，值为`["john", "james", "louis", "vincent"]`。由于字典排序后转换为元组数组，`map`方法提取排序后的键名数组。

> [代码分析] 原代码中的`sort`实际应为`sorted`（Swift 3+），但假设使用旧版本API则结果有效。关键点在于理解字典排序后转换为(key, value)元组数组，按值升序排列后取出键名。

## 23. 分析以下代码结果类型及其推算过程

```swift
struct Planet {
    let name: String
    let distanceFromSun: Double
}

let planets = [...行星数据...]

let result1 = planets.map { $0.name }
let result2 = planets.reduce(0) { $0 + $1.distanceFromSun }
```

对应答案段落：
`result1`类型为`[String]`，包含所有行星名称数组。`map`方法将每个`Planet`实例转换为`name`属性的字符串，最终生成字符串数组。

`result2`类型为`Double`，值为各行星距离太阳之和（约57.699）`。`reduce`方法以初始值0开始，累加每个元素的`distanceFromSun`属性。

> [原理说明] `map`转换元素类型，`reduce`聚合产生单一值。对于结构体数组的操作本质上都是通过对元素属性的访问进行变换，保留原始数据类型的特征。

## 24. 结构体与类的值传递差异分析

```
struct Tutorial {
  var difficulty: Int = 1
}

var tutorial1 = Tutorial()
var tutorial2 = tutorial1
tutorial2.difficulty = 2
// tutorial1.difficulty 和 tutorial2.difficulty 的值？
// 若Tutorial改为class结果是否不同？
```

对应答案段落：
结构体版本中：`tutorial1.difficulty`保持1，`tutorial2.difficulty`变为2。值类型在赋值时创建副本，修改不影响原对象。

类版本中：两者值均为2。类实例赋值传递引用，`tutorial1`和`tutorial2`指向同一内存地址。

> [核心概念] 通过具体示例对比值类型与引用类型的赋值语义差异。结构体的写时复制（copy-on-write）机制和类的指针传递特征是理解Swift内存模型的基础。

## 25. `var`与`let`声明对引用类型的影响

```swift
import UIKit

var view1 = UIView()
view1.alpha = 0.5

let view2 = UIView()
view2.alpha = 0.5 // 能否编译？
```

对应答案段落：
最后一行可编译通过。`let`仅限制变量不能重新赋值，但允许修改引用类型实例的可变属性。若尝试`view2 = view1`则会报错，因为`view2`为不可变绑定。

> [深入理解] 对于引用类型，`let`约束的是变量指向的实例地址，而非实例本身的可变性。若要完全锁定实例状态，需要结合其他设计模式如不可变类实现。

## 26. 请尽可能简化以下用于数组排序的闭包表达式

```swift
var animals = ["fish", "cat", "chicken", "dog"]
animals.sort { (one: String, two: String) -> Bool in
    return one < two
}
print(animals)
```

1. 删除类型声明：类型推断系统会自动推导参数和返回值类型

```swift
animals.sort { (one, two) in return one < two }
```

2. 改用参数占位符：

```swift
animals.sort { return $0 < $1 }
```

3. 省略 return 关键字（单表达式闭包）:

```swift
animals.sort { $0 < $1 }
```

4. 利用运算符方法简化：

```swift
animals.sort(by: <)
```

> [考察内容] 这里需要展示对 Swift 闭包语法糖的理解，尤其是对类型推断、简写参数和运算符方法这三个递进层次的掌握

## 27. 修改人员地址后出现意外结果的原因分析及解决方案

以下两个 Person 实例共用同一个 Address 实例当地址属性更新时出现交叉影响：

```swift
class Address {
  var fullAddress: String
  var city: String
  // 初始化方法省略...
}

var headquarters = Address(fullAddress: "123 Tutorial Street", city: "Appletown")
var ray = Person(name: "Ray", address: headquarters)
var brian = Person(name: "Brian", address: headquarters)

brian.address.fullAddress = "148 Tutorial Street"
print(ray.address.fullAddress) // 输出148
```

地址更新异常问题在于：Address 使用的类（引用类型）特性使得多个 Person 共享同一个实例。修改地址时会同步影响所有持有该引用的对象。

解决方案两个方向：

1. **副本创建**：修改时生成新地址对象

```swift
brian.address = Address(fullAddress: "148 Tutorial Street", city: "Appletown")
```

2. **类型转换**：将 Address 声明为结构体

```swift
struct Address { ... }
```

> [深入理解] 该问题考察对值类型与引用类型语义差异的认知。结构体在赋值时会自动创建独立副本，从而避免意外共享

## 28. Optional 类型的核心作用及其解决的问题

Optional 允许任何类型的变量表达"值缺失"的语义。在 Objective-C 中，这种能力仅限于对象类型通过 nil 实现，值类型（如 int）缺乏这种表达能力。

带来的三方面改进：

- 统一处理空值：值类型和引用类型都可以使用 Optional
- 安全性提升：强制解包机制减少运行时意外崩溃
- 语义清晰：通过类型系统显式标记可能空值的变量

比如 Int? 与 Int 有明确的类型区分，编译器会对强制解包进行检测。这与 Objective-C 中发送消息给 nil 对象不会报错的隐式处理形成对比

## 29. `UITableViewCell` 构造方法中的 `reuseIdentifier` 作用剖析

`reuseIdentifier` 作为细胞复用标识符，核心价值在于：

1. **细胞复用池管理**：
   - 根据标识符分类存储离屏单元格
   - 滚动时优先从池中获取可用单元格，避免重复创建

2. **性能优化**：
   - 实例化单元格是昂贵操作（内存分配、布局计算）
   - 实测复用机制可提升滚动帧率 30-50%

3. **混合布局支持**：
   - 不同 reuseIdentifier 对应差异化布局
   - 例如交替行样式、广告位插入等情况

使用示例：

```objc
// 注册单元格类型与标识符
[self.tableView registerClass:[MyCell class] forCellReuseIdentifier:@"CellTypeA"];

// 复用获取
MyCell *cell = [tableView dequeueReusableCellWithIdentifier:@"CellTypeA"];
```

## 30. UIView 布局系统的多种实现方式对比

在 iOS 生态中存在四种主流布局方案：

**坐标系布局 (Frame-based)**

```swift
view.frame = CGRect(x: 20, y: 20, width: 100, height: 40)
```

特点：直接操作视图位置和尺寸，适用于简单绝对定位

**自动布局 (Auto Layout)**

```swift
NSLayoutConstraint.activate([
    button.centerXAnchor.constraint(equalTo: view.centerXAnchor),
    button.topAnchor.constraint(equalTo: textField.bottomAnchor, constant: 20)
])
```

核心优势：动态适应不同屏幕尺寸，支持响应式变化

**界面构建器 (Interface Builder)**

- Storyboard/XIB 的拖拽布局实质是 Auto Layout 的可视化封装
- 优势在于实时预览，适合快速原型开发

**声明式布局 (SwiftUI)**

```swift
VStack {
    Text("Title").font(.title)
    HStack(spacing: 20) {
        Image(systemName: "star")
        Text("Rating: 4.8")
    }
}
```

代表未来趋势，跨平台支持但需 iOS 13+

> [技术选型] 当前项目兼容性要求高的场景仍以 Auto Layout 为主，新项目建议逐步迁移到 SwiftUI

## 31. atomic 与 nonatomic 属性的实现差异与适用场景

底层原理对比：

| 特性        | atomic                       | nonatomic          |
| 锁机制       | 自旋锁（iOS 10 后改为互斥锁）         | 无                 |
| getter/setter | 自动加锁保证原子操作               | 普通内存访问           |
| 性能损耗      | 较高（每次存取都有加锁解锁开销）          | 接近原生访问速度        |
| 线程安全      | 单次存取原子性，不保证多操作整体安全 | 完全无保障            |

代码实现差异：

```objc
// atomic 的自动合成类似：
- (NSString *)name {
    @synchronized(self) {
        return _name;
    }
}
- (void)setName:(NSString *)name {
    @synchronized(self) {
        _name = name;
    }
}
```

应用建议：

- UI 相关属性优先使用 nonatomic
- 需要线程安全时配合高层锁机制（如 GCD 队列）使用，而非依赖 atomic
- 历史遗留代码迁移时可保留 atomic，新代码默认使用 nonatomic

## 32. 全局变量初始化和 +initialize 方法的执行时序分析

代码场景：

```objc
// MyClass.h
extern NSString *startTime;

// MyClass.m
NSString *startTime = nil;
+ (void)initialize {
    if (self == [MyClass class]) {
        startTime = [NSDate date].description;
    }
}
```

执行流程解析：

1. 全局变量在镜像加载时初始化为 nil
2. 首次向 MyClass 发送消息时触发 `+initialize`
3. AppDelegate 的 `application:didFinishLaunching...` 本身就会触发其类的 `+initialize`
4. 因此打印时 startTime 已被正确赋值

潜在风险：

- 多个类别中的 `+initialize` 执行顺序不确定
- 线程安全性问题（尽管系统保证只执行一次）

改进方案：

```objc
+ (void)load {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        startTime = [NSDate date].description;
    });
}
```

## 33. 循环引用的典型场景与破解方法

示例模型：

```objc
@interface TTParent : NSObject
@property (strong) NSMutableArray<TTChild *> *children;
@end

@interface TTChild : NSObject
@property (strong) TTParent *parent; // 强引用导致循环
@end
```

内存循环示意图：

```
Parent ➡️ children Array ➡️ Child 
  ▲          ▲
  └── parent ┘
```

解决方案两种范式：

1. **弱引用破环**

```objc
@property (weak) TTParent *parent;
```

特点：自动置 nil 安全但需注意生命周期管理

2. **中间层解耦**

```objc
@interface TTChild : NSObject
@property (unsafe_unretained) TTParent *parent; // 用于性能敏感场景
@end
```

注意：需手动管理，存在野指针风险

## 34. 后台线程更新UI的正确实现方式

错误代码：

```objc
[backgroundQueue addOperationWithBlock:^{
    [NSThread sleepForTimeInterval:10];
    self.alertLabel.text = @"Done"; // 潜在崩溃
}];
```

三个修复方案：

**GCD 派发**

```objc
dispatch_async(dispatch_get_global_queue(QOS_CLASS_UTILITY, 0), ^{
    // 后台操作
    dispatch_async(dispatch_get_main_queue(), ^{
        self.label.text = @"Updated";
    });
});
```

**NSOperation 依赖**

```objc
NSOperation *backgroundOp = [NSBlockOperation blockOperationWithBlock:^{
    // 耗时任务
}];
NSOperation *uiOp = [NSBlockOperation blockOperationWithBlock:^{
    self.button.hidden = YES;
}];
[uiOp addDependency:backgroundOp];
[[NSOperationQueue mainQueue] addOperation:uiOp];
[backgroundQueue addOperation:backgroundOp];
```

**Swift 并发（iOS 13+）**

```swift
Task {
    await someAsyncWork()
    await MainActor.run {
        button.setTitle("Done", for: .normal)
    }
}
```

## 35. Bundle ID 与 App ID 的关联与差异

对比维度表：

| 特征        | Bundle Identifier          | App ID                   |
|---|---|---|
| 组成结构      | 反向DNS字符串（如 com.xx.app）      | TeamID + Bundle Identifier |
| 生效范围      | 开发阶段定义                  | 开发者Portal配置            |
| 通配符支持     | 否                         | 是（如 com.xx.*）           |
| 用途        | 设备安装唯一标识                 | 服务启用凭证（推送、支付等）         |
| 变更影响      | 需要重新发布应用                 | 重新生成Provisioning Profile |

典型错误场景：Xcode 中的 Bundle ID 与 App ID 不匹配导致构建失败

## 36. Objective-C 内存管理中的强弱引用抉择

引用类型决策树：

1. 是否有循环引用风险？
   - 是 → 使用 weak/unowned
   - 否 → strong

2. 是否需要自动置空？
   - 需要 → weak
   - 不需要 → unsafe_unretained

使用范例：

```objc
@interface NetworkManager : NSObject
@property (weak) id<DownloadDelegate> delegate; 
@end

__weak typeof(self) weakSelf = self;
[self.dataTask completion:^{
    [weakSelf.delegate didComplete]; // 安全访问
}];
```

## 37. NSManagedObjectContext 的多线程管理策略

托管上下文的三种并发模式：

**主队列类型**

```objc
let mainContext = NSManagedObjectContext(
    concurrencyType: .mainQueueConcurrencyType
)
```

适用于 UI 相关操作，要求在主线程执行

**私有队列类型**

```objc
let privateContext = NSManagedObjectContext(
    concurrencyType: .privateQueueConcurrencyType
)
privateContext.perform { // 自动后台线程执行
    // 复杂数据操作
}
```

适合批量数据操作，与主线程上下文隔离

**传统类型（已弃用）**

```objc
.legacyQueueConcurrencyType // iOS 9 前使用
```

最佳实践：

- 父子上下文层次结构：子上下文处理临时变更，父上下文持久化存储
- 使用 performBlock 保证线程安全
- 合并策略设置：NSMergeByPropertyObjectTrumpMergePolicy

## 38. iOS并发编程的三层抽象比较

**NSThread**

- 最底层线程 API
- 手动管理生命周期
- 示例：

```objc
NSThread *thread = [[NSThread alloc] initWithTarget:self 
                                          selector:@selector(backgroundWork) 
                                            object:nil];
[thread start];
```

**GCD (Grand Central Dispatch)**

- 系统级线程池管理
- 队列分类：
  - Serial（串行）
  - Concurrent（并行）
  - Main（主线程）

```swift
DispatchQueue.global(qos: .userInitiated).async {
    let result = processData()
    DispatchQueue.main.async {
        updateUI(with: result)
    }
}
```

**NSOperationQueue**

- 基于 GCD 的高级抽象
- 特性：
  - 操作依赖（addDependency）
  - 优先级（queuePriority）
  - 取消机制（cancelAllOperations）

```objc
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
queue.maxConcurrentOperationCount = 2;

NSBlockOperation *downloadOp = [NSBlockOperation blockOperationWithBlock:^{
    // 下载任务...
}];
NSBlockOperation *parseOp = [NSBlockOperation blockOperationWithBlock:^{
    // 解析数据...
}];
[parseOp addDependency:downloadOp];
[queue addOperations:@[downloadOp, parseOp] waitUntilFinished:NO];
```

## 39. Objective-C 中比较 NSString 的运算符陷阱

代码实例：

```objc
NSString *a = @"test";
NSString *b = @"test";
NSLog(@"%d", a == b); // 可能输出1（地址相同）
```

潜在问题：

- == 比较的是对象指针而非内容
- 相同字符串字面量可能指向同一地址（编译器优化）

正确内容比较三法：

```objc
// 精确比较
[a isEqualToString:b]; 

// 包含 Unicode 规范化处理
[a compare:b options:NSCaseInsensitiveSearch];

// 通用对象比较（可能触发类型转换）
[a isEqual:b];
```

特殊情况处理：

```objc
// 可变字符串处理
NSMutableString *mutableA = [a mutableCopy];
BOOL equal = [a isEqualToString:mutableA]; // YES
```

## 40. iOS 应用状态转换图谱

状态迁移路径：

冷启动 → Not running → Inactive → Active

运行中 → Active ↔ Inactive

背景驻留：
Active → Background → Suspended

终止情况：
Suspended → Not running（系统回收内存）
Background → Not running（后台任务超时）

关键回调方法：

```swift
func applicationWillResignActive()  // 进入Inactive
func applicationDidEnterBackground() // 进入Background
func applicationWillEnterForeground() // 即将返回
func applicationDidBecomeActive()    // 完全激活
```

## 41. 应用启动方法的区别与应用场景

对比表格：

| 方法                          | 触发时机                   | 典型用途                               |
|---|---|---|
| `willFinishLaunching`       | 启动初期                 | 三方库初始化、全局配置                      |
| `didFinishLaunching`        | UI准备阶段               | 窗口创建、根视图控制器设置                    |
| `applicationDidBecomeActive` | 首次激活或从后台返回          | 恢复用户会话、刷新数据                       |
| `applicationWillTerminate`  | 应用即将终止               | 保存关键数据、清理临时文件                    |

性能优化建议：

- 耗时初始化操作应异步执行或延后到首屏显示之后
- 使用启动图（Launch Screen）占位降低等待感知

## 42. JSONSerialization 的高级处理选项

扩展使用场景：

**写入配置示例**

```swift
let options: JSONSerialization.WritingOptions = [
    .prettyPrinted, // 可读格式化
    .sortedKeys,    // 排序键值（iOS 11+）
    .withoutEscapingSlashes // 保持URL格式（iOS 13+）
]
let data = try JSONSerialization.data(withJSONObject: dict, 
                                    options: options)
```

**读取配置案例**

```objc
NSDictionary *dict = [NSJSONSerialization JSONObjectWithData:data 
    options:NSJSONReadingMutableContainers | NSJSONReadingAllowFragments
    error:nil];
```

异常处理要点：

- 使用 `JSONObjectWithData:options:error:` 时检查 error 参数
- `allowFragments` 允许解析非容器类型（如单独字符串、数字）
- 写操作中 `fragmentsAllowed`（macOS 12+/iOS 15+）支持根对象为非容器

## 43. Swift 值类型设计哲学与实践优势

差异对比实例：

```swift
// 值类型示例
struct Point { var x, y: Double }
var p1 = Point(x: 1, y: 2)
var p2 = p1
p2.x = 3
print(p1.x) // 1（保持独立）

// 引用类型示例
class Dog { var name = "Buddy" }
let dog1 = Dog()
let dog2 = dog1
dog2.name = "Max"
print(dog1.name) // Max（共享修改）
```

适用场景选择：

- **值类型优选**：简单数据结构、独立状态、多线程安全需求
- **引用类型适用**：复杂对象关系、需要身份标识、共享可变状态

现代 Swift 实践趋势：

- 优先使用结构体，组合成领域模型
- 仅在需要继承或引用语义时采用类
- 结合协议与关联类型实现灵活抽象

## 44. Swift 中的 Optional（可选类型）是什么？其设计目的是什么？

Optional 类型用于表示一个值可能存在或为 `nil` 的情况。通过类型包装提供显式安全性，强制开发者在编译阶段处理可能的空值场景。常见的解包方式包括：

1. **强制解包**：`!` 操作符（存在运行时崩溃风险）
2. **条件解包**：`if let` 语法绑定非空值到局部变量
3. **提前返回**：`guard let` 进行前置条件验证

```swift
var optionalString: String? = "Hello"
if let unwrapped = optionalString {
    print(unwrapped)
}
```

> [考察内容] 此处需要重点说明 Optional 类型通过编译时安全检查机制避免空指针异常的核心理念。可对比其他语言的 `null` 处理方式差异，突出 Swift 类型系统的安全性。

## 45. Swift 的 `guard` 语句适用场景？与 `if let` 的主要区别是什么？

`guard` 主要用于函数/方法的前置条件验证，当条件不满足时必须使用 `return`、`break`、`throw` 等控制流操作退出当前作用域。其核心差异点：

- **变量作用域**：`guard let` 绑定变量在外部作用域仍有效，而 `if let` 限内部块作用域
- **代码流程**：`guard` 强制失败分支优先处理，提升代码可读性

```swift
func process(input: String?) {
    guard let validInput = input else {
        print("Invalid input")
        return
    }
    // validInput 在此处可直接使用
}
```

> [深入理解] 此问题意在考查对 Swift 错误处理范式和代码架构的理解。可结合 API 传参校验等具体场景说明设计优势。

## 46. Swift 的 `weak` 与 `unowned` 在内存管理中如何选择？请说明应用场景

二者均用于打破循环引用：

- **`weak`**：
  - 声明为 Optional 类型
  - 当引用的对象被释放后自动置为 `nil`
  - 适用于视图控制器与子视图间引用，或异步回调可能失效的场景

- **`unowned`**：
  - 非 Optional 类型
  - 假设对象生命周期与持有者一致或更长
  - 典型场景如父子对象间的双向引用

```swift
class Parent {
    var child: Child?
}
class Child {
    unowned let parent: Parent
    init(parent: Parent) {
        self.parent = parent
    }
}
```

> [注意] 当不确定对象生命周期时应优先选择 `weak`。错误使用 `unowned` 可能导致野指针访问崩溃。

## 47. Objective-C 的快速枚举(Fast Enumeration)实现机制？相较传统遍历的优势

通过 `for (Type obj in collection)` 语法实现集合遍历，编译器会优化为直接访问底层存储结构而非调用 `objectAtIndex:` 等方法。其优势包括：

- **性能优化**：减少消息发送次数
- **代码简洁**：避免手动管理索引变量
- **线程安全**：枚举过程中集合不可变性的保证

```objective-c
NSArray *array = @[@"A", @"B"];
for (NSString *str in array) { 
    NSLog(@"%@", str);
}
```

## 48. Objective-C 中 Category（分类）与 Extension（类扩展）的区别及限制

二者都用于扩展类功能，但存在关键差异：

| 特性            | Category               | Extension             |
|---|---|---|
| 源代码需求      | 不需要原始类代码       | 必须拥有源代码       |
| 添加方法        | ✔️                      | ✔️                    |
| 添加实例变量    | ❌（需关联对象间接实现）| 可在头文件中添加属性 |
| 访问权限        | 公开新方法             | 常用于私有属性/方法  |

> [场景示例] 分类常用于为系统类（如 `NSString`）添加工具方法，扩展通常用于 `.m` 文件中隐藏实现细节。

## 49. 在 Objective-C 属性声明中，`copy` 与 `strong`（`retain`）修饰符的选择依据

决策维度基于对象可变性：

- **`strong`** 用于不可变对象，保持引用关系

```objective-c
@property (strong) NSArray *immutableArray;
```

- **`copy`** 确保可变对象属性值的不可变性

```objective-c
@property (copy) NSMutableString *stringCopy;
// 赋值时会执行 mutableCopy 保证独立存储
```

> [关键点] 当属性可能被外部共享的可变对象赋值时，必须使用 `copy` 修饰符防御性维护封装性。例如 `NSString` 属性接收 `NSMutableString` 类型输入的场景。

## 50. 比较NSOperation、GCD与NSThread的区别

> [考察内容] 该问题需要展示对不同线程抽象层次与实际应用场景的理解。回答时应重点突出抽象层级差异与适用场景  

NSOperation是较GCD更高层次的抽象，基于GCD构建但增加更多功能特性：  

1. **依赖管理**：可通过`addDependency`直接设置操作间依赖，而GCD需通过同步或串行队列间接实现  
2. **任务控制**：支持`cancel()`取消操作、设置`maxConcurrentOperationCount`限制并发数，GCD需自行管理  
3. **观察状态**：提供KVO属性如`isReady`, `isExecuting`监控任务生命周期  
4. **优先级设置**：`queuePriority`支持不同操作的调度顺序，但仅在同队列生效  

GCD(C Grand Central Dispatch)特点：  

```objective-c
// 一次性执行（线程安全）
static dispatch_once_t onceToken;
dispatch_once(&onceToken, ^{ /* 单例初始化代码 */ });

// 延迟执行
dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
    NSLog(@"延迟2秒执行");
});
```  

- **底层高效**：直接使用C语言API，运行时开销更小  
- **队列类型**：提供串行、并发、全局队列和主队列  
- **执行方式**：同步(`dispatch_sync`)与异步(`dispatch_async`)  

NSThread 是最基础线程管理方式：  

- 需要手动管理线程生命周期（启动/取消）  
- 需显式处理线程同步问题（如手动加锁）  
- 适合简单后台任务但缺少高级功能  

三者层级关系：NSThread → GCD → NSOperation  

## 51. 多线程编程中常见安全隐患及解决方案

共享资源（同一对象、变量、文件）在并发访问时可能引发**数据竞争**（Data Race），典型场景如：  

- 多个线程同时修改同一变量  
- 读写文件未同步导致内容错误  

解决方式：  

1. **互斥锁**（Mutex Lock）：  

```objective-c
@synchronized(self) {
    // 需要保护的代码区
    self.sharedValue += 1;
}
```  

> [注意] 应避免创建多个无关锁对象，否则会导致无效锁定  

2. **线程同步技术**：  

- **信号量**：`dispatch_semaphore_t`控制资源访问数量  
- **串行队列**：通过将写操作放入专用队列实现同步  

3. **原子属性**：  

```objective-c
@property (atomic) NSInteger count; // 自动加锁的setter方法
```  

> [深入理解] `atomic`只能保证读写原子性，无法解决复合操作（如递增`count++`）的线程安全  

## 52. 三种多线程技术（NSThread/NSOperation/GCD）的使用场景对比

**性能优先级场景选择**：  

1. **GCD**适合简单任务且需要极致性能：  

- 全局队列执行图像批量处理  
- 利用并发队列加速数据预处理  

2. **NSOperation**适合复杂任务流：  

```objective-c
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
queue.maxConcurrentOperationCount = 3; // 限制并发数

NSBlockOperation *downloadOp = [NSBlockOperation blockOperationWithBlock:^{ /* 下载任务 */ }];
NSBlockOperation *parseOp = [NSBlockOperation blockOperationWithBlock:^{ /* 解析数据 */ }];

[parseOp addDependency:downloadOp]; // 设置依赖
[queue addOperations:@[downloadOp, parseOp] waitUntilFinished:NO];
```  

- 需要任务取消、进度跟踪的下载队列管理  
- 存在前后依赖关系的任务链（如先下载后解析）  

3. **NSThread**应用较少，可用于：  

- 需要长期运行的后台监控线程  
- 必须精细控制线程生命周期的特殊场景  

> [决策要点] 推荐优先级：GCD/NSOperation > NSThread。优先使用高级抽象，仅在必要时用底层API

## 53. HTTP与HTTPS有什么区别？HTTP请求方法有哪些？

HTTPS需向CA申请证书（通常涉及费用），而HTTP无需。HTTP使用明文传输且端口为80，HTTPS通过SSL/TLS加密传输且使用443端口。HTTPS通过SSL+HTTP实现加密传输和身份认证，比HTTP更安全。HTTP无状态，HTTPS建立安全会话后仍能保持连接状态。

HTTP请求方法包括：

- OPTIONS：获取服务器支持的请求方法或测试功能
- GET：请求特定资源
- POST：向指定资源提交数据
- PUT：替换指定位置资源
- HEAD：获取响应头信息
- DELETE：删除指定资源  
- TRACE：回显请求报文用于测试
- CONNECT：建立管道代理连接

> [深入理解] 该问题需要区分协议层差异与实现细节。HTTP方法的语义由RFC规范定义，实际开发需遵循REST API设计原则。例如PUT用于幂等更新，POST用于非幂等操作。

## 54. GET与POST请求的核心区别是什么？

参数位置：GET参数显示在URL中，POST参数在请求体中。  
数据量限制：GET受URL长度限制（约1KB），POST支持更大数据传输（可达2MB）。  
缓存机制：GET请求会被浏览器缓存存储历史记录，POST默认不被缓存。  
安全考虑：GET参数暴露在地址栏可能导致敏感信息泄露，POST适合传输私密数据。  
协议规范：POST必须设置Content-Type头，且发送参数时需要调用send方法传递数据。  

> [考察重点] 这里关注的是HTTP协议底层差异而非表象区别。例如GET的幂等性特性导致可缓存，而POST的非幂等性要求更严格的安全处理。

## 55. 描述HTTP协议工作流程

1. **地址解析**：分解URL为协议、主机名、端口和资源路径。例如`http://localhost.com:8080/index.htm`分解为：
   - 协议：HTTP
   - 主机：localhost.com（DNS解析为IP）
   - 端口：8080
   - 路径：/index.htm

2. **封装HTTP数据包**：组合请求头与主体信息

3. **TCP三次握手建立连接**：默认80端口（示例中为8080）

```bash
# TCP连接建立过程（三次握手）
Client -> SYN -> Server
Server -> SYN+ACK -> Client  
Client -> ACK -> Server
```

4. **发送请求报文**：包含请求行、头部和实体，例如：

   ```
   GET /index.htm HTTP/1.1
   Host: localhost.com
   ```

5. **服务器响应**：返回状态行和资源数据，例如：

   ```
   HTTP/1.1 200 OK
   Content-Type: text/html
   <!DOCTYPE html>...
   ```

6. **TCP连接关闭**：默认关闭连接，启用Keep-Alive头后可复用

## 56. Charles如何抓取HTTPS包？基本原理是什么？

核心原理是中间人攻击（MITM）：  

1. 客户端信任Charles的自签名根证书  
2. Charles拦截HTTPS请求，生成伪造证书与客户端建立SSL连接  
3. Charles作为代理与真实服务器建立另个SSL连接  
4. 双向解密并记录明文通信数据  

具体流程：

1. 安装Charles根证书到系统信任列表  
2. 配置设备代理指向Charles  
3. 拦截请求时Charles动态生成证书  
4. 客户端验证Charles伪造证书（需用户手动信任）  
5. 完成TLS协商后，Charles可解密并记录所有HTTPS流量  

> [技术细节] HTTPS的TLS握手过程包含非对称加密交换对称密钥，Charles通过中间人代理实现了密钥的双向获取，但这种操作会破坏端到端加密的安全性。

## 57. TCP三次握手过程及各次握手作用

```bash
# 三次握手流程：
1. CLIENT -> SYN=1, seq=X -> SERVER (SYN_SENT)
2. SERVER -> SYN=1, ACK=1, seq=Y, ack=X+1 -> CLIENT (SYN_RCVD)
3. CLIENT -> ACK=1, seq=X+1, ack=Y+1 -> SERVER (ESTABLISHED)
```

**意义**：

- 第一次：客户端发送初始序列号，确认客户端发送能力  
- 第二次：确认客户端序列号，发送服务器序列号，确认双端收发能力  
- 第三次：确认服务器序列号，防止失效连接请求

## 58. TCP四次挥手过程及TIME_WAIT状态必要性

```bash
# 四次挥手流程：
1. Client -> FIN=1, seq=u -> Server (FIN_WAIT_1)
2. Server -> ACK=1, ack=u+1 -> Client (CLOSE_WAIT)
   Client进入FIN_WAIT_2
3. Server -> FIN=1, seq=w -> Client (LAST_ACK)
4. Client -> ACK=1, ack=w+1 -> Server (TIME_WAIT)
```

**TIME_WAIT状态作用**：

- 确保最后ACK到达服务端（等待2MSL时间）
- 防止旧连接报文干扰新连接（MSL=Max Segment Lifetime）

> [常见问题] 为何需要四次挥手？因为TCP支持半关闭，收到FIN后需要先确认已接收数据，再发送己方FIN。

## 59. 解释TCP、UDP、Socket关系与差异

**协议层差异**：  

- TCP：可靠流式传输，保证顺序/完整性  
- UDP：无连接数据报，高效但可能丢失  

**Socket抽象**：  

```c++
// 典型Socket使用
int sockfd = socket(AF_INET, SOCK_STREAM, 0);  // TCP
int sockfd = socket(AF_INET, SOCK_DGRAM, 0);   // UDP
```

**HTTP与Socket**：

- HTTP基于TCP短连接（无状态请求-响应）
- Socket支持长连接双向通信（可TCP/UDP）
- 真实案例：WebSocket基于TCP实现全双工通信

## 60. 网络深度优化有哪些关键点？

1. **协议层优化**：  
   - 使用QUIC协议（基于UDP）解决TCP队头阻塞  
   - 启用HTTP/2多路复用减少连接数  

2. **缓存策略**：  

   ```swift
   // NSCache示例
   NSCache<NSString, NSData> *cache = [NSCache new];  
   cache.countLimit = 100;
   ```

3. **网络适应**：  
   - 动态调整超时（蜂窝网络>WiFi）  
   - 数据压缩（Protobuf代替JSON）  
   - 失败重试与离线队列  

4. **DNS优化**：  
   - 本地DNS缓存  
   - HTTPDNS绕过运营商污染  

> [进阶方向] 结合CDN加速、预连接、资源预加载等形成完整优化方案。

## 61. 请解释RunLoop（运行循环）、线程和自动释放池（Autorelease Pool）之间的关系，并说明RunLoop的主要作用是什么？

苹果在主线程RunLoop注册了两个Observer（观察者），其回调函数均与自动释放池相关。第一个Observer监听Entry事件（即将进入Loop），通过调用_objc_autoreleasePoolPush()创建自动释放池，order设置为最低（-2147483647）以确保优先执行。第二个Observer监听BeforeWaiting（准备休眠）和Exit（即将退出Loop）事件，分别在休眠前执行_objc_autoreleasePoolPop()和_objc_autoreleasePoolPush()来更新池，在退出时释放池，order设置为最高（2147483647）以确保最后执行。

> [实现原理] 自动释放池本质是由AutoreleasePoolPage构成的双向链表，每个页面占4096字节。当对象调用autorelease时会被压入栈中，AutoreleasePoolPage::pop()会向栈中对象发送release消息。POOL_BOUNDARY作为边界标记，配合哨兵指针（nil）实现精准释放控制。

RunLoop的作用主要有三个方面：实现线程保活避免资源浪费（通过管理休眠/唤醒循环）、处理事件源（包括输入源/定时源/观察者）形成消息处理机制、统一管理自动释放池生命周期。线程与Runloop形成"一一对应不强制绑定"的关系，而自动释放池则依托RunLoop的事件周期实现内存自动管理。

代码示例中展示循环内内存暴增问题的解决方案：

```objective-c
for (int i = 0; i < 1000; i++) {
    @autoreleasepool {
        UIImage* image = [UIImage imageNamed:@"some_image"]; // 返回autorelease对象
    } // 循环内部池释放时机
}
```

此处通过嵌套@autoreleasepool确保每次迭代都及时释放临时对象。若未使用，所有UIImage对象都会累积到主RunLoop的自动释放池，直到事件循环结束才释放。

## 62. 请解释HTTPS的工作原理，说明对称加密与非对称加密的区别及应用场景

> [加密基础] 对称加密使用相同密钥k进行加解密，算法如DES、AES。优势在于运算速度快，但存在密钥传输风险。非对称加密使用公钥e加密、私钥d解密（如RSA），解决密钥分发问题但计算成本较高。HTTPS采用混合加密：非对称协商临时对称密钥，后续使用对称加密传输数据。

SSL握手核心流程：

1. 客户端发送支持的加密套件列表
2. 服务器返回证书（含公钥S_PuKey）并选定算法
3. 客户端验证证书链（通过内置CA公钥校验数字签名）
4. 生成pre-master secret用S_PuKey加密发送
5. 双方通过预主密钥导出会话密钥（session key）
6. 使用对称密钥加密后续通信

> [证书验证] 使用Hash算法（如SHA-256）生成摘要，CA用私钥加密生成数字签名。浏览器使用预置CA公钥解密得到标准摘要，与当前计算值比对验证证书有效性。需注意证书域名匹配性、有效期等安全检查。

HTTP与HTTP/2对比核心改进：

1. 二进制分帧层实现多路复用（Multiplexing），解决队头阻塞
2. Header压缩（HPACK算法）减少冗余
3. 服务器推送（Server Push）预发送关联资源
4. 改进的流量控制机制

## 63. NSURLSession与NSURLConnection的主要区别是什么？请结合场景说明各自的优劣势

核心差异点体现在架构设计和功能扩展性：

1. 任务管理机制：NSURLSession支持三种任务类型（Data/Download/Upload），与NSURLSession实例解耦
2. 后台传输：通过配置NSURLSessionConfiguration实现后台网络任务（backgroundSessionConfiguration）
3. 断点续传：NSURLSessionDownloadTask直接支持临时文件续传
4. 配置粒度：支持每个Session独立配置缓存策略（URLCache）、Cookie存储（HTTPCookieStorage）、证书链验证等

代码示例对比内存管理：

```objective-c
// NSURLConnection加载大文件需自行管理内存缓冲区
NSMutableData *responseData = [NSMutableData new];
// 需要手动处理内存峰值问题

// NSURLSessionDownloadTask直接将数据写入磁盘
NSURLSessionDownloadTask *task = [session downloadTaskWithURL:url 
    completionHandler:^(NSURL *location, NSURLResponse *response, NSError *error) {
    // 自动保存到临时文件，需手动转移至Documents
}];
```

> [生命周期控制] NSURLSession支持cancel、suspend、resume操作，配合delegate可实现精细进度管理。NSURLConnection在发起后即自动运行，停止操作仅能调用cancel。

## 64. 请详细解释dealloc方法的底层实现过程，包括与其相关的内存管理机制

对象释放核心流程：

1. 引用计数归零时触发release操作
2. 通过objc_msgSend调用dealloc方法链（子类->父类->NSObject）
3. 执行objc_object::rootDealloc()清理对象关联资源

关键内存回收步骤：

```cpp
void objc_object::rootDealloc() {
    if (!hasNonpointerCxxDtor && !weakly_referenced && !hasAssociatedObjects && !hasCxxDtor && !hasSidetableRC) {
        free(this); // 快速释放路径
    } else {
        object_dispose(this); // 复杂对象处理
    }
}
```

具体清理函数objc_destructInstance的执行顺序：

1. object_cxxDestruct：调用C++析构函数和OC实例变量的release
2. _object_remove_assocations：解除所有关联对象（objc_setAssociatedObject绑定）
3. clearDeallocating：清除weak引用表（将弱引用指针置nil）和引用计数表

> [线程安全] dealloc执行期间会加锁保证原子性。特别注意：

- 不应在dealloc中执行耗时操作或调用异步方法
- 调用主线程相关API可能导致死锁
- 使用__weak时可能触发次级dealloc调用链

## 65. 请解释HTTP/1.1与HTTP/2的核心差异，并说明这些改进对移动端开发的影响

协议层主要优化点：

1. 二进制分帧（Binary Framing）：头部信息采用HPACK压缩，数据帧支持优先级设定
2. 多路复用（Multiplexing）：单个TCP连接并行处理多个请求，解决队头阻塞
3. 服务端推送（Server Push）：预推送CSS/JS等静态资源，减少RTT次数
4. 头部压缩：静态表（61个常用头字段）与动态表组合优化

移动端开发效益：

```swift
// 请求合并示例（HTTP/1.1需多个连接）
let request1 = URLRequest(url: imageURL1)
let request2 = URLRequest(url: imageURL2)

// HTTP/2可在同一连接上并行传输
let configuration = URLSessionConfiguration.ephemeral
configuration.protocolClasses = [CustomProtocol.self] // 支持HTTP/2
```

> [性能提升] 对比测试显示，在3G网络下HTTP/2实现首屏加载时间减少30%~50%。需要注意：

- 需服务端和CDN支持HTTP/2协议
- 启用前需通过ALPN协商协议版本
- 避免过多的细碎请求降低头部压缩效率

## 66. 互斥锁（Mutex Lock）和自旋锁（Spin Lock）的本质区别

两者最核心的区别在于线程状态的改变：当获取锁失败时，互斥锁会让线程进入睡眠（sleep）状态，解锁时再恢复运行（running），这个过程伴随上下文切换、CPU抢占等系统开销。而自旋锁则会保持running状态，通过循环检测锁标志位来等待锁释放，不会主动让出CPU资源。

> [应用场景] 这种差异导致互斥锁更适合长时间持锁的场景（毫秒级以上），可以避免空转浪费CPU；自旋锁则在极短等待时间（通常微秒级）下更高效

关键差异点：

1. 初始开销：互斥锁上下文切换需要更高初始开销，但后续开销固定；自旋锁初始开销低，但随等待时间线性增长
2. CPU消耗：互斥锁将线程挂起不占用CPU；自旋锁持续占用CPU核心资源
3. 死锁风险：自旋锁使用不当更易出现死锁（尤其是递归调用或系统函数调用时）
4. 适用系统：在单核不可抢占内核中自旋锁会自动降级为空操作，主要用于多核(SMP)可抢占环境

自旋锁典型用法：

```objective-c
spinlock_t lock;
spin_lock_init(&lock);

spin_lock(&lock);
//临界区操作
spin_unlock(&lock);
```

## 67. NSObject 对象的内存布局流程

内存对齐遵循两项核心原则：

1. 结构体内的每个成员相对于结构体首地址的偏移量必须是该成员类型大小的整数倍
2. 结构体的总大小必须是其最大成员类型大小的整数倍

> [计算案例] 例如结构体包含char(1byte)、int(4byte)、double(8byte)：实际内存占用是1+3(padding)+4+8=16bytes，满足8的倍数。其中char后的3字节是填充位

对于NSObject实例：

- 在64位系统中占用8字节（仅包含指向类结构的isa指针）
- 实际通过malloc_size获取时会包含内存对齐的系统级优化，可能返回16字节

## 68. KVO 的底层实现原理

实现过程核心步骤：

1. 动态创建 NSKVONotifying_XXX 子类并修改实例的isa指针
2. 重写被观察属性的setter方法，插入willChangeValueForKey:和didChangeValueForKey:通知
3. 重写class方法隐藏子类存在

> [触发机制] 直接修改成员变量不会触发KVO，必须通过属性访问器或手动调用will/didChange系列方法

手动触发示例：

```objective-c
[object willChangeValueForKey:@"age"];
//直接修改实例变量
_age = 25;
[object didChangeValueForKey:@"age"];
```

## 69. 常用算法的实现与场景分析

冒泡排序（Bubble Sort）：

```objective-c
for (int i = 0; i < array.count; i++) {
    for (int j = 0; j < array.count-1-i; j++) {
        if (array[j] > array[j+1]) {
            [array exchangeObjectAtIndex:j withObjectAtIndex:j+1];
        }
    }
}
//时间复杂度：O(n²)，适合小数据集
```

选择排序（Selection Sort）：

```objective-c
for (int i = 0; i < array.count; i++) {
    for (int j = i+1; j < array.count; j++) {
        if (array[i] > array[j]) {
            [array exchangeObjectAtIndex:i withObjectAtIndex:j];
        }
    }
}
//同样是O(n²)时间复杂度，交换次数少于冒泡排序
```

动态规划案例：N阶楼梯问题

```swift
func stepWays(n: Int) -> Int {
    if n < 3 { return n }
    var dp = [1, 2]
    for i in 2..<n {
        dp.append(dp[i-1] + dp[i-2])
    }
    return dp.last!
}
//使用滚动数组可将空间复杂度优化到O(1)
```

## 70. iOS 对象的继承体系

核心继承链：

1. NSObject作为根类，提供基本内存管理方法（alloc/dealloc）
2. 根类中包含isa指针指向元类（Meta Class）
3. 元类保存类方法信息，其继承关系与实例类平行
![](../0.Image/iOS/1.webp)

> [Runtime特性] 通过object_getClass()追踪isa指针可以发现：  
实例的isa → 类对象  
类对象的isa → 元类  
元类的isa → 根元类

## 71. 通知、代理、KVO 的优劣对比与适用场景

三者的核心差异协议：
| 机制        | 实时性 | 耦合度 | 回调方式     | 跨进程 |
| Delegate  | 高   | 紧   | 方法直接调用  | 否   |
| KVO       | 中   | 松   | 键值观察触发  | 否   |
| Notification | 低 | 无   | 广播通知模式 | 可   |

最佳实践场景：

- Delegate：需要明确API契约的1对1通信（如UITableViewDelegate）
- KVO：模型层数据同步，如ViewModel属性自动更新
- Notification：应用全局事件广播（如键盘显隐事件）

> [实现细节] KVC的查找顺序：

```objective-c
//设值过程：
1. 找set<Key>:方法 
2. 找成员变量：_<key> → _is<Key> → <key> → is<Key>
//取值过程：
1. 找get<Key>/<key>/is<Key> 
2. 同上查找成员变量顺序
```

## 72. Block的原理是什么？它在内存管理上有哪些特点？

> [深入理解] 此处需要展示对Block底层结构和内存管理的理解，尤其要区分不同内存区域的特性和copy操作的影响  

Block本质是指向结构体的指针，属于运行时的高级特性，基于纯C语言实现。通过`clang -rewrite-objc`命令可将OC代码转换为C实现，观察到结构体如`__main_block_impl_0`的生成。  

默认情况下Block存储在栈区（_NSConcreteStackBlock），随时可能被回收。只有通过copy操作才会将其移动到堆区（_NSConcreteMallocBlock）。关键点包括：  

- 对GlobalBlock执行copy无实际意义，因为它存储在全局区  
- 对MallocBlock执行copy会使其引用计数+1  
- 赋值给非__weak变量、显式调用copy方法、作为copy属性时都会触发copy操作  

代码示例展示不同变量捕获对Block内存区域的影响：  

```objective-c
void (^globalBlock)() = ^{ NSLog(@"GlobalBlock"); }; // 不捕获局部变量 → _NSGlobalBlock
__block int a;
void (^heapBlock)() = ^{ a++; }; // 捕获并修改局部变量 → ARC下自动提升为_NSMallocBlock
NSLog(@"%@", [heapBlock copy]); // copy操作后地址不变，仍在堆区
```

## 73. Block内部如何修改外部变量的值？内存地址如何变化？

> [考察内容] 需解释__block的作用机制和不同变量类型的捕获差异  

Block无法直接修改局部变量的栈内存地址，但可通过以下方式实现修改：  

1. 使用__block修饰符：  
   - 将变量从栈复制到堆，使Block内外共享同一内存  
   - 变量大小可能从4字节扩至32字节（因Foundation框架的内存管理）  
2. 直接修改静态变量/全局变量：  
   - 这些变量存储在常量区，Block直接引用原地址  

内存变化示例：  

```objective-c
__block int val = 5; // 原栈地址：0x7fff...
void (^block)() = ^{ val = 10; }; // 执行后val地址变为堆地址：0x600000...
```

此时Block内部持有的是堆上的变量引用，修改会同步到原作用域。

## 74. 在ARC与MRC环境下，Block的内存管理有何差异？

> [注意点] 需区分ARC的自动优化与MRC的手动管理  

核心差异在于：  

- **MRC环境**：  
  - 必须显式使用copy将栈Block拷贝到堆  
  - 赋值给strong属性不会自动copy  
- **ARC环境**：  
  - 系统自动执行必要copy（如赋值给strong变量）  
  - strong与copy修饰符效果相同  

实验验证：  

```objective-c
// MRC下：
void (^stackBlock)() = ^{ NSLog(@"StackBlock"); }; 
NSLog(@"%@", stackBlock); // _NSStackBlock
void (^heapBlock)() = [stackBlock copy]; // 显式copy → _NSMallocBlock

// ARC下：
self.strongBlock = stackBlock; // 自动copy → _NSMallocBlock
```

## 75. __block与__weak修饰符的区别是什么？

区别主要体现在三个方面：  

1. **适用范围**  
   - __block：可修饰对象和基本类型，适用MRC/ARC  
   - __weak：仅ARC下修饰对象  

2. **内存管理**  
   - __block会retain对象（MRC下需手动避免循环引用）  
   - __weak不增加引用计数  

3. **可变性**  

   ```objective-c
   __block NSObject *obj = [NSObject new];
   __weak NSObject *weakObj = obj;
   void (^block)() = ^{
       obj = [NSObject new]; // 允许重新赋值
       // weakObj = [NSObject new]; // 编译错误
   };
   ```

## 76. Swift中的逃逸闭包与非逃逸闭包有什么区别？对内存管理有什么影响？

关键差异点：  
| 特性          | 非逃逸闭包                 | 逃逸闭包                  |
| 生命周期       | 函数执行期间有效               | 可能被外部存储或延迟执行         |
| 内存安全       | 不会引起循环引用               | 可能引发循环引用（需弱引用处理）      |
| 修饰符        | 默认类型，无需特殊标注            | 需@escaping标注          |
| 性能优化       | 编译器可进行栈内存分配等优化          | 需堆内存分配                |

示例说明：  

```swift
// 非逃逸闭包
func nonEscaping(closure: () -> Void) {
    closure() 
}

// 逃逸闭包
var storedClosure: (() -> Void)?
func escaping(closure: @escaping () -> Void) {
    storedClosure = closure
}

class Test {
    var value = 0
    func test() {
        escaping { self.value = 1 } // 必须显式使用self
        nonEscaping { value = 2 } // 可隐式访问属性
    }
}
```

## 77. 为什么NSString属性常用copy修饰？使用strong可能带来什么问题？

> [深入理解] 此处需要解释可变对象与不可变对象的潜在问题  

使用copy的核心理由是保持封装性：  

1. **防御可变对象**：  
   - 当传入NSMutableString时，copy会生成不可变副本  
   - 防止外部修改影响属性值  

2. **强引用问题**：  

   ```objective-c
   @property (strong) NSString *strongStr;
   @property (copy) NSString *copiedStr;

   NSMutableString *mutable = [NSMutableString stringWithString:@"Hello"];
   self.strongStr = mutable; 
   self.copiedStr = mutable;

   [mutable appendString:@" World"];
   // strongStr变为"Hello World"
   // copiedStr保持"Hello"  
   ```

3. **性能权衡**：  
   - 对于绝对不可变的对象，copy只是retain（浅拷贝）  
   - 对可变对象执行深拷贝  

代码验证：  

```objective-c
NSString *origin = @"Original";
NSMutableString *mutable = origin.mutableCopy;

// 使用strong修饰：
self.strongProperty = mutable;
[mutable appendString:@" changed"]; 
NSLog(@"%@", self.strongProperty); // 输出"Original changed"

// 使用copy修饰：
self.copyProperty = mutable; 
[mutable appendString:@" again"];
NSLog(@"%@", self.copyProperty); // 保持"Original" 
```

## 78. 关于iOS中copy修饰符的规则，具体有哪些需要注意的要点？

对于不可变对象（如NSString、NSArray、NSDictionary、NSSet）执行copy操作时，实际是浅复制。而对其余对象的复制操作都会产生深拷贝，特别要注意以"Mutable"开头的类进行复制时一定是深拷贝。

> [实际用法]

- 当修饰不可变属性时推荐使用copy，这样对新值的赋值操作将自动生成不可变副本：

```objective-c
@property (copy) NSString *nameStr; 
// 对传入的NSString/NSMutableString都会生成不可变的NSCFString副本
```

- 自定义对象要实现copy能力必须覆写NSCopying协议中的`copyWithZone:`方法
- 对可变属性的修饰应避免使用copy，否则会产生意料之外的不可变副本

## 79. iOS内存管理的核心原则是什么？哪些场景必须手动释放对象？如何结合property特性避免泄漏？

内存管理的黄金准则可总结为三个"匹配"原则：每个alloc/new/copy/mutableCopy调用都要配对release，每个retain要配对release，每个strong/copy属性的setter方法需要及时释放旧值。

必须手动释放的对象产生场景包括：

1. 通过alloc/new/copy/mutableCopy方法创建的对象（遵循工厂方法的命名规范）
2. 显式调用retain增加引用计数的对象
3. NSObject类的类方法生成的对象（例如`[NSArray array]`会自动入池但需要遵循自动释放池规则）

> [property陷阱]
使用copy/retain修饰属性时，需特别注意在dealloc中主动释放：

```objective-c
@property (copy) NSArray *data;
// dealloc实现中要释放
- (void)dealloc {
    [_data release];
    [super dealloc];
}
```

当访问已释放对象时会产生"野指针"，系统会抛出EXC_BAD_ACCESS错误。这种情况常见于assign修饰的指针未及时置nil的场景。

## 80. iOS中创建线程的主要方式有哪些？如何确保代码在主线程执行？列举延时执行的常用方法

多线程实现的三大体系：

1. **_NSThread_**：轻量级直接线程控制
2. **_Operation Queue_**：基于任务队列的抽象层
3. **_Grand Central Dispatch_**：现代的线程管理范式

确保主线程执行的核心方法：

```objective-c
// 方式一：NSObject的便捷方法
[self performSelectorOnMainThread:@selector(updateUI) withObject:nil waitUntilDone:YES];

// 方式二：指定主线程队列
dispatch_async(dispatch_get_main_queue(), ^{
    [self.tableView reloadData];
});
```

延时执行的典型方案：

```objective-c
// perform方式（需注意runloop状态）
[self performSelector:@selector(refresh) withObject:nil afterDelay:2.0];

// Timer方式（子线程需手动开启runloop）
[NSTimer scheduledTimerWithTimeInterval:1.0 target:self selector:@selector(checkStatus) userInfo:nil repeats:NO];
```

在后台线程使用timer时要注意显式启动runloop：

```objective-c
dispatch_async(dispatch_get_global_queue(0, 0), ^{
    [[NSRunLoop currentRunLoop] addTimer:self.timer forMode:NSDefaultRunLoopMode];
    [[NSRunLoop currentRunLoop] run];
});
```

## 81. 分析UITableView出现卡顿的根源原因及优化策略？

卡顿的本质在于帧生成的超时。以60FPS为例，每16.7ms需要完成一帧的生成（包含CPU布局计算与GPU渲染），任何环节超时都会导致丢帧。

**_核心痛点解析：_**  
图像流水线包括五个关键阶段：

1. **Layout**：视图层级计算与布局约束处理
2. **Display**：drawRect方法触发的软件绘制
3. **Prepare**：图像解码与格式转换
4. **Commit**：图层树提交到渲染服务
5. **Render**：GPU的顶点处理/光栅化/材质合成

> [优化手段]
**_CPU侧优化：_**

- 预计算和缓存行高与布局信息
- 将耗时操作（JSON解析/图片解码）移至后台队列
- 精简Cell的subviews层级结构
- 使用异步绘制技术（CATiledLayer）

**_GPU侧优化：_**

- 规避离屏渲染（圆角处理建议使用CAShapeLayer配合贝塞尔曲线）
- 减少图层混合（善用opaque属性与不透明素材）
- 控制纹理尺寸（推荐使用适合屏幕scale的图片资源）

## 82. iOS离屏渲染的产生原理与栅格化的实际作用？

**_离屏渲染（Offscreen Rendering）_**
是GPU在非屏幕缓冲区进行绘图的过程，典型场景包括：

```objective-c
view.layer.cornerRadius = 5.0;
view.layer.masksToBounds = YES; // 触发离屏渲染!
```

其性能损耗主要来自两个方面：额外缓冲区的创建维护和上下文切换开销。常见于阴影、蒙层、圆角+裁切等视觉效果。

**_栅格化（ShouldRasterize）_**
通过缓存渲染结果来优化重复绘制的场景，适合静态内容：

```objective-c
cell.layer.shouldRasterize = YES;
cell.layer.rasterizationScale = [UIScreen mainScreen].scale;
```

需要注意若图层内容频繁变化，反而会因缓存失效造成性能反效果。适用于复杂但静态的滚动列表项优化。

## 83. 如何有效监控UITableView滚动时的卡顿现象？

主流监控方案有两种实现路径：

**_方案一：主线程卡顿检测_**
通过监听Runloop状态转换来实现：

```objective-c
CFRunLoopObserverRef observer = CFRunLoopObserverCreateWithHandler(kCFAllocatorDefault, kCFRunLoopAllActivities, YES, 0, ^(CFRunLoopObserverRef observer, CFRrunLoopActivity activity) {
    if (activity == kCFRunLoopBeforeSources || activity == kCFRunLoopAfterWaiting) {
        // 记录时间戳比较时间差
    }
});
CFRunLoopAddObserver(CFRunLoopGetMain(), observer, kCFRunLoopCommonModes);
```

**_方案二：FPS实时统计_**
通过CADisplayLink计算帧率：

```objective-c
CADisplayLink *link = [CADisplayLink displayLinkWithTarget:self selector:@selector(tick:)];
[link addToRunLoop:[NSRunLoop mainRunLoop] forMode:NSRunLoopCommonModes];

void tick:(CADisplayLink *)link {
    _count++;
    if (_lastTime == 0) {
        _lastTime = link.timestamp;
        return;
    }
    NSTimeInterval delta = link.timestamp - _lastTime;
    if (delta < 1) return;
    float fps = _count / delta;
    NSLog(@"FPS:%.0f",fps);
    _count = 0;
    _lastTime = link.timestamp;
}
```

## 84. UIView 和 CALayer 的区别与联系

> [考察内容] 该问题主要考查对iOS视图系统底层实现的理解，特别是UIView与CALayer的分工协作机制。需要展示对Core Animation框架的基本认知。

UIView的layer属性在其官方注释中明确指出其本质：`@property(nonatomic,readonly,strong) CALayer *layer` 表明UIView始终关联一个非空的CALayer实例，并作为该layer的委托对象。具体区分点如下：

1. **事件处理能力**：UIView作为UIResponder的子类，可直接处理触摸事件（touch events），而CALayer仅负责渲染，无法响应交互
2. **层级关系**：
   - UIView通过管理内部的CALayer实现视觉呈现（size、style等属性本质都是操作layer）
   - Layer多出锚点（anchorPoint）属性，这是坐标系差异的关键
3. **渲染机制**：视图显示时，UIView通过实现`CALayerDelegate`协议参与layer的显示过程（`display`方法触发内容生成）
4. **动画支持**：
   - CALayer属性修改默认触发隐式动画
   - UIView通过重写`actionForLayer:forKey:`控制layer的动画行为，实现显式动画的管理

```swift
// 示例：UIView与CALayer属性关联（frame属性实际上是访问layer）
let view = UIView()
view.frame = CGRect(x: 0, y: 0, width: 100, height: 100) 
// 底层等同于操作 view.layer.frame
```

## 85. iOS数据持久化的主要方式与安全考量

主要数据存储方案包括：

1. **NSUserDefaults**：轻量级键值存储，适用于配置项（如用户偏好设置）
2. **Property Lists**：XML/二进制格式序列化，支持基本数据类型
3. **NSKeyedArchiver**：对象归档，需遵循`NSCoding`协议
4. **数据库方案**：SQLite直接操作、ORM框架（FMDB）、CoreData或WCDB等
5. **Keychain Services**：系统级安全存储（密钥、凭证等）

> [深入理解] UserDefaults实质是在沙盒内维护plist文件，存在数据泄露风险：

```swift
// 注意：UserDefaults同步操作建议用于关键数据
let defaults = UserDefaults.standard
defaults.set("sensitiveData", forKey: "secretKey")
defaults.synchronize() // 强制立即写入
```

**安全最佳实践**：

- 敏感数据必须加密存储（如AES-GCM算法）
- 避免在plist中明文存储凭证
- 优先使用Keychain存储生物特征等隐私信息
- SQLite/CoreData数据库文件进行加密处理

## 86. instance/class/meta-class三者的内存结构差异

> [关键概念] 基于Objective-C运行时对象模型，各类型特点：

1. **Instance对象**：
   - 存储isa指针（指向类对象）及实例变量
   - 每次alloc产生独立内存空间

   ```objc
   // 获取实例内存占用（考虑内存对齐）
   class_getInstanceSize([NSObject class]) // 返回8字节（64位系统）
   ```

2. **Class对象**：
   - 单例形式存在（每个类唯一）
   - 包含isa（指向元类）、superclass指针、属性列表、方法列表等
3. **Meta-class对象**：
   - 结构与class相同，但存储类方法信息
   - 其isa指针最终指向根元类，构成闭环

## 87. App瘦身与启动优化的核心策略

**瘦身方案**：

- 删除冗余资源（[fui工具](https://github.com/dblock/fui)扫描无用图片/类）
- 使用ImageOptim等工具压缩图片资源
- 审核Podfile依赖，移除未使用的库
- 调整架构支持（如去掉armv7s以减小二进制体积）

**启动时序优化**：

```objective-c
// 典型启动阶段：
main() -> 加载动态库 -> 初始化Runtime -> 执行+load方法 -> main函数体
```

1. 减少动态库数量（合并必要功能）
2. 优化`application:willFinishLaunchingWithOptions:`中的初始化
   - 将非关键任务延迟执行
   - 异步处理网络预请求
3. 视图层级优化（按需加载根视图内容）

**性能监测工具**：

- Instruments组件的Time Profiler/Leaks分析性能瓶颈
- Xcode Organizer分析启动时间指标

## 88. iOS系统架构层级划分

系统架构分为四层：

1. **Core OS层**：内核管理（BSD Socket/Disk Access等）
2. **Core Services层**：基础框架（CFNetwork/SQLite等）
3. **Media层**：音视频处理（Core Audio/OpenGL ES）
4. **Cocoa Touch层**：UIKit等应用层框架

## 89. iOS控件主要事件响应类型

核心事件处理机制：

1. **触控事件**（UITouch-based）：`touchesBegan`系列方法
2. **值变更事件**（Value-changed）：如UISlider滑动
3. **编辑事件**（Editing-related）：UITextField输入状态变化

```swift
// 示例：按钮同时响应多种事件类型
button.addTarget(self, action: #selector(handleTouchDown), for: .touchDown)
button.addTarget(self, action: #selector(handleValueChanged), for: .valueChanged)
```

## 90. 请解释分类(category)、继承(inheritance)和扩展(extension)的区别，并回答以下子问题：Objective-C是否支持多重继承？如何实现多个接口？重写类时使用继承好还是分类好？为什么？

Objective-C的类不支持多重继承（只能单继承），多继承效果可以通过protocol（协议）和委托代理模式模拟实现。协议允许实现多个接口来达到类似多重继承的效果。

> [深入理解] 实现多重继承的三种方式：

1. 消息转发机制：覆盖`methodSignatureForSelector:`和`forwardInvocation:`方法，将方法调用转发到其他对象
2. 组合模式：在类中持有多个其他类的实例
3. 协议实现：定义多个protocol并实现

**分类与继承的区别**：

- 分类是在现有类的基础上动态添加方法（可添加属性但需借助关联对象），不能修改原始类的实现。同名方法会覆盖原类方法（优先级：分类 > 本类 > 父类）
- 继承创建新子类，可完全重写/扩充父类功能，能添加新属性和方法，支持多态特性

**扩展的特殊性**：

- 匿名分类（没有独立实现文件）
- 只能为已有类添加私有方法和属性
- 声明周期与主类绑定（需在.m文件中实现）
- 不支持运行时动态添加成员变量

> [替换方案] 优先使用分类而非扩展，因为分类更灵活。分类适合在不修改原类的情况下添加工具方法，而继承适合需要完全改变类行为或创建新类型层次结构的情况。

## 91. 解释frame和bounds的区别，修改bounds的size是否会影响frame？

**核心区别**：

1. **坐标系基准**：
   - frame以父视图坐标系为基准
   - bounds以自身坐标系为基准
2. **行为差异**：
   - 修改frame.origin会改变视图位置
   - 修改bounds.origin会改变子视图的布局原点

**尺寸变化影响逻辑**：

```objc
// 原始frame = (x,y,100,100), bounds = (0,0,100,100)
view.bounds = CGRectMake(0, 0, 200, 200); 
// 变化后：
// bounds.size改变会使视图的绘制内容缩放（类似transform）
// frame会自动调整为 (x,y,200,200)
```

> [注意事项] 改变bounds.size实际上同时影响frame.size，但这种改变会导致视图内容的缩放效果，与直接修改frame.size的单纯尺寸变化不同。实际开发中应避免直接修改bounds.size，推荐使用transform进行缩放操作。

## 92. id声明的对象有什么特性？

声明为`id`类型的对象具有以下特点：

- 动态类型特征（运行时决定实际类型）
- 可指向任意Objective-C对象（类似void*但带对象信息）
- 关闭编译时类型检查（需谨慎使用）
- 天然兼容泛型场景
- 自动处理对象引用计数（与非NSObject类型指针不同）

## 93. atomic与nonatomic属性的本质区别是什么？

| 特性        | atomic                     | nonatomic          |
| 线程安全     | 通过锁机制保证原子性           | 无保护机制           |
| 性能影响     | 较大（每次访问加锁解锁）         | 高效                |
| 默认值      | Mac OS程序默认              | iOS程序默认          |
| 适用场景     | 多线程共享资源访问             | 单线程/已同步资源访问   |
| setter实现 | 完整属性保护                  | 直接赋值             |

> [调试技巧] 使用`xcrun -sdk iphonesimulator clang -rewrite-objc`命令可查看生成的原子操作代码实现。

## 94. 线程与进程的主要区别有哪些？

**关键差异**：

1. 内存空间：
   - 进程：独立地址空间（4GB虚拟内存保护）
   - 线程：共享进程内存（直接访问全局变量）
2. 创建消耗：
   - 进程：资源消耗大（需分配独立内存）
   - 线程：创建更快（共享现存资源）
3. 通信方式：
   - 进程：IPC（管道/消息队列等）
   - 线程：直接读写共享内存
4. 容错性：
   - 进程崩溃不影响其他进程
   - 线程崩溃可能导致整个进程终止

## 95. 原生与H5交互的常用方案有哪些？

**主流实现方案**：

```javascript
// JavaScriptCore基础交互示例
JSContext *context = [webView valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
context[@"nativeMethod"] = ^(JSValue *param) {
    // 处理H5调用
    return @"result";
};

// WKWebView消息传递方案
WKScriptMessageHandler *handler = [[WKScriptMessageHandler alloc] init];
[userContentController addScriptMessageHandler:handler name:@"bridge"];
```

**性能对比**：
| 方案                | 优点                          | 缺点                  |
| WebViewJavaScriptBridge | 成熟稳定                      | 需要引入第三方库          |
| JavaScriptCore      | 官方方案、直接对象映射              | 内存管理复杂            |
| WKWebView           | 高性能、内置优化                 | iOS8+支持            |
| URL Scheme          | 简单通用                      | 数据传输能力有限          |

## 96. Swift中struct和class应如何选择？

**选择标准**：

```swift
// 值类型示例
struct Point {
    var x: Double
    var y: Double
}

// 引用类型示例
class Node {
    var value: Int
    var next: Node?
}
```

**决策矩阵**：

- 选择struct的情况：
  - 需要值语义（复制传递）
  - 轻量级数据模型（坐标、尺寸等）
  - 不需要继承
- 选择class的情况：
  - 需要引用语义（共享实例）
  - 需要继承/多态
  - 需要deinit释放资源
  - 与Objective-C API交互

## 97. NSCache与NSDictionary的核心区别是什么？YYModel为何选用NSCache？

**核心差异点**：

- 内存管理：
  - NSCache自动释放内存不足时的缓存
  - NSDictionary需手动清理
- 线程安全：
  - NSCache线程安全
  - NSDictionary需加锁保护
- 密钥处理：
  - NSCache不强拷贝键对象
  - NSDictionary拷贝key

**YYModel选择NSCache的原因**：

1. 自动内存管理适配模型解析需求
2. 高频访问需要线程安全保证
3. 保留原始key对象避免拷贝开销
4. 弱引用策略优化内存使用

## 98. HashMap的实现原理及其与链表的区别？

**数据结构组合**：

```java
// Java伪代码展示HashMap结构
class HashMap {
    Entry[] table; // 数组存储桶
    
    static class Entry {
        final Object key;
        Object value;
        Entry next; // 链表结构
    }
}
```

**哈希桶机制**：

1. 哈希函数：key.hashCode() % capacity
2. 冲突解决：链表法（Java8引入红黑树优化）
3. 扩容机制：负载因子(默认0.75)触发rehash

**与链表的性能对比**：
| 操作       | 哈希表(平均) | 链表   |
| 查找       | O(1)    | O(n) |
| 插入       | O(1)    | O(1) |
| 删除       | O(1)    | O(n) |
| 空间效率    | 较好      | 优秀   |

## 99. dispatch_once 内部执行流程是怎样的？存在哪些需要注意的问题？

dispatch_once 通过 onceToken 的状态决定代码执行逻辑：

1. **初始化状态 (onceToken=0)**：执行 block 并更新 token 值  
2. **已执行完成状态 (onceToken=-1)**：跳过 block 直接返回  
3. **执行中状态 (其他数值)**：阻塞等待直到前一个 block 完成  

> [深入理解] 例如线程 A 首次调用时 token 为 0，开始执行 block 并将 token 更新为随机大数值（如 140734537148864）；此时线程 B 调用会因 token 非零进入等待。线程 A 结束时将 token 标记为-1，后续所有线程直接跳过 block。  

需注意：

1. **避免复杂操作**：仅建议在 block 内进行**单例对象创建**等轻量操作  
2. **销毁单例处理**：若需重置单例需两步操作：① 将单例置 nil ② 重置 onceToken=0  
3. **异常处理**：dispatch_once 内部无异常捕获机制，block 崩溃会导致后续永远无法执行

## 100. 请解释 UIView、UIWindow、UIViewController、UINavigationController 的层级关系，并设计一个始终置顶的全局悬浮窗？

- 组件关系梳理

1. **UIWindow**  

- UIView 的子类  
- 应用的**首个视图容器**，持有 rootViewController  
- 自动将 rootViewController.view 加入自身层级  

2. **UIView**  

- 负责绘制矩形区域与事件响应  
- 通过 `.window` 可反向查找所属窗口  

3. **UIViewController**  

- 管理 view 的生命周期和逻辑  
- 通过 view 属性关联视图层级  

4. **UINavigationController**  

- **容器型控制器**，维护 UIViewController 栈  
- 导航栏（UINavigationBar）属于当前 VC 而非导航控制器  

- 悬浮窗实现方案  

1. **独立窗口方案**  

```objc
// Swift 伪代码示例
let floatWindow = UIWindow(frame: UIScreen.main.bounds)
floatWindow.windowLevel = .alert + 1 // 设置层级高于默认窗口
floatWindow.rootViewController = customFloatVC
floatWindow.isHidden = false
```

2. **层级监控逻辑**  

- 实时检测 keyWindow 变化时调用 `bringSubviewToFront`  
- 配合 KVO 监听 `[UIApplication sharedApplication].windows` 变化  

> [设计要点] 需覆盖截屏/录屏等系统操作，通过 `windowLevel` (如 .statusBar + 1) 保证绝对置顶，但要避免遮挡系统功能组件

## 101. iOS 卡顿监测方案中如何捕获线程堆栈信息？其核心原理是什么？

- 数据采集流程  

1. **Mach 线程遍历**  
通过 `task_threads()` 获取所有线程的 mach_port 列表  

2. **寄存器状态捕获**  

```c
_STRUCT_MCONTEXT machineContext;
mach_msg_type_number_t state_count = BS_THREAD_STATE_COUNT;
kern_return_t kr = thread_get_state(thread, BS_THREAD_STATE, (thread_state_t)&machineContext->__ss, &state_count);
```

3. **栈帧回溯原理**  

- **Stack Pointer (SP)**: 指向当前栈顶  
- **Frame Pointer (FP)**: 存有上一栈帧 FP 地址  
- **符号化处理**: 通过动态链接器解析地址到函数名  

- 关键技术点  
>
> [核心概念] 每个线程拥有独立调用栈，栈帧通过 FP 指针串联形成链式结构。使用 `backtrace_symbols()` 可将地址转为可读符号  

典型实现方案：  

1. 主线程心跳包机制检测 RunLoop 状态  
2. 子线程超时判定生成堆栈快照  
3. 汇编层级获取 SP/FP 寄存器值完成栈展开

## 102. Quartz 2D 绘图系统的三大核心概念及其作用？

1. **图形上下文 (Graphics Context)**  

- 绘图输出目标（位图/PDF/图层等）  
- 保存绘制状态（颜色、线宽、变换矩阵）  

2. **路径 (Path)**  

- 使用 `CGPath` 描述矢量图形轮廓  
- 包含 Bezier 曲线、几何图形等元素  

3. **状态栈 (State Stack)**  

- 通过 `CGContextSaveGState()` / `CGContextRestoreGState()` 管理图层属性  
- 保存与恢复填充色、裁剪区域等配置

## 103. iOS 系统提供的音频播放方案有哪些？各自适用场景是？

| 方案                 | 特性                                                                 | 适用场景             |
| System Sound Services| 最低延迟（<30s）、简单提示音                                        | 按钮点击反馈、警报   |
| AVAudioPlayer         | 支持格式广、控制播放进度                                            | 背景音乐、音频播放器 |
| Audio Queue Services | 底层 API，支持音频处理（变速/混音）                                  | 语音识别、录音应用   |
| OpenAL               | 跨平台、3D 音效                                                    | 游戏开发环境         |

> [补充框架] AudioToolbox (编解码)、AVFoundation (媒体处理)、Core Audio (底层音频管道)

## 104. 如何追踪并解决 iOS App 的线上崩溃问题？

- 崩溃收集方案  

1. **基础统计平台集成**  

- 友盟/腾讯 Bugly 等自动捕获异常  
- 上传符号表 (dSYM) 用于堆栈解析  

2. **高级诊断工具**  

- Xcode Organizer 查看设备日志  
- Instruments 复现特定崩溃场景  

- 崩溃解决流程  

1. **日志符号化**  

```bash
atos -arch arm64 -o AppName.app.dSYM -l 0x100000000 0x0000000100d54320
```

2. **高发问题归类**  

- 内存问题（野指针/内存溢出）  
- 主线程阻塞（UI 响应超时）  
- 异步任务竞态条件  

3. **热修复策略**  

- JSPatch (已弃用) / React Native 动态补丁  
- 灰度发布验证修复效果

## 105. iOS 事件响应链的工作机制是怎样的？点击屏幕时的传递路径如何？

- 核心概念划分  

- **传递链 (Delivery Chain)**：系统 -> UIApplication -> UIWindow -> 子视图（hitTest 决策）  
- **响应链 (Responder Chain)**：初始视图 -> 父视图 -> ... -> UIViewController -> UIWindow -> UIApplication  

- 点击事件处理流程  

1. **Hit-Testing 阶段**  

```objc
// 伪代码逻辑
func hitTest(_ point: CGPoint, with event: UIEvent?) -> UIView? {
    if !isUserInteractionEnabled || isHidden || alpha <= 0.01 { return nil }
    if self.point(inside: point, with: event) {
        for subview in subviews.reversed() {
            let convertedPoint = convert(point, to: subview)
            if let candidate = subview.hitTest(convertedPoint, with: event) {
                return candidate
            }
        }
        return self
    }
    return nil
}
```

2. **响应者确认**  
若 hit-test view 未处理事件，沿响应链向上一级传递，直到有对象响应或到达 Application  

> [典型案例] UIControl 对象（如 UIButton）若未实现处理，事件会向上传递到其父视图直至控制器

## 106. 如何在 iOS 应用中实现退入后台后接收到支付通知时主动播放到账金额？

可通过 ServiceExtension 扩展处理推送内容：

1. **iOS 10+ 方案**：在 `UNNotificationServiceExtension` 中使用语音合成或音频拼接

   - **AVSpeechSynthesis**：直接将推送文本转语音播放

   ```objective-c
   AVSpeechSynthesizer *synthesizer = [[AVSpeechSynthesizer alloc] init];
   AVSpeechUtterance *utterance = [AVSpeechUtterance speechUtteranceWithString:@"到账100元"];
   [synthesizer speakUtterance:utterance];
   ```

   - **音频拼接**：根据数字匹配预存音频片段，通过 `AudioServicesCreateSystemSoundID` 播放组合音频

   > [系统适配策略] iOS 10 以下版本需采用变通方案：推送时根据设备版本定制内容
   - 对 iOS 10+ 设备推送纯数字内容并禁用默认提示音
   - 对旧版系统推送固定提示语音（如："支付宝，您有一笔到账，请及时查看"）
   - 需在服务端维护设备版本信息库进行智能推送分发

## 107. 分析 static、const 和 sizeof 三个关键字的作用与差异

- static 关键字

- **局部变量**：将变量存储方式改为静态存储区，生命周期延长至程序结束
- **全局变量/函数**：限制作用域为当前文件，实现模块隔离

```objective-c
// 文件内全局可访问的静态变量
static int globalCounter = 0;

// 仅在当前文件可调用的函数
static void internalUtility() { /*...*/ }
```

- const 关键字

- **常量声明**：创建只读变量（指针常量与常量指针）

```objective-c
// 指针常量（地址不可变）
NSString *const strPtr = @"ImmutablePointer";

// 常量指针（值不可变）
const NSString *strVal = @"ImmutableValue";
```

> [深入理解] NSString 的特殊性：因 NSString 本身为不可变类，实际开发中更常见使用 `NSString *const` 形式

- sizeof 运算符

- **编译时计算**：获取数据类型/对象在内存中的字节大小
- **特殊性质**：
  - 唯一以单词形式存在的运算符
  - 可对变量/类型直接使用（非函数调用形式）

```c
// 获取数组长度技巧
int arr[] = {1,2,3};
size_t count = sizeof(arr)/sizeof(arr[0]); // 3
```

## 108. 对比 iOS 开发中的 MVC、MVP、MVVM 架构模式

- MVC 模式

- **标准结构**：
  - Model：数据模型
  - View：界面元素
  - Controller：业务逻辑
- **iOS 实现特点**：
  - View 与 Model 通过观察者模式间接通信
  - ViewController 承担控制与协调职责
  - 典型问题：Massive View Controller

> [架构演进] MVC → MVP → MVVM 的核心优化方向是降低耦合度

- MVP 模式

- **流程改造**：
  - Presenter 作为中间层接管业务逻辑
  - View 通过协议定义接口
  - 完全隔离 Model 与 View
- **优势**：
  - 方便单元测试
  - View 可复用性增强
  - 适合复杂页面逻辑解耦

- MVVM 模式

- **核心机制**：数据绑定（Data Binding）
  - ViewModel 暴露可观察属性
  - 自动同步 View 与 Model 状态
- **iOS 实现**：
  - 结合 KVO 或第三方框架（如 RxSwift）
  - 典型应用：MVVM + ReactiveCocoa

```swift
// ViewModel 样例
class UserViewModel {
    @Published var username: String = ""
}
```

> [架构选择] 简单场景适合 MVC，复杂业务推荐 MVVM，需要严格解耦时考虑 MVP

## 109. 在 64位/32位架构下，long 和 char* 类型的字节占用差异

- 数据对比表

| 类型          | 32位系统 | 64位系统 |
| char          | 1字节   | 1字节   |
| char*         | 4字节   | 8字节   |
| long          | 4字节   | 8字节   |
| long long     | 8字节   | 8字节   |
| NSInteger     | 4字节   | 8字节   |

- 关键差异

1. **指针类型**：地址空间扩展使指针大小翻倍
2. **long 类型**：随架构变化而改变
3. **类型安全**：

```objective-c
// 错误示例：32位下可能溢出
int total = 4294967296; 

// 正确做法：使用 NSInteger
NSInteger safeTotal = 4294967296;
```

- NSInteger 实现机制

```cpp
#if __LP64__ || TARGET_OS_EMBEDDED || TARGET_OS_IPHONE || TARGET_OS_WIN32
typedef long NSInteger;
#else
typedef int NSInteger;
#endif
```

> [移植注意] 跨平台开发时需特别注意类型尺寸差异，避免数值溢出和内存对齐问题

## 110. NSTimer的循环引用问题是如何产生的？请结合代码示例说明常见内存泄漏场景

当创建定时器时使用了`target:self`，例如`self.timer = [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(showMsg) repeats:YES]`，此时NSTimer会强引用`self`对象。而self（通常是控制器）又通过强引用属性`timer`保持着定时器对象，形成双向强引用闭环。这样当页面需要销毁时，由于循环引用导致控制器的`dealloc`方法无法执行，timer无法被释放。

> [深入理解] 此处应该明确强调问题核心在于"NSTimer和控制器之间的相互强引用导致引用计数无法降为零"。即使尝试修改属性修饰符为`weak`，但定时器的`invalidate`依赖控制器销毁，而控制器销毁又需要先释放定时器，形成"鸡生蛋蛋生鸡"的问题。

## 111. 使用__weak修饰self能否解决NSTimer的循环引用？为什么？

这种方法无效。虽然声明了`__weak typeof(self) weakSelf = self`并传递给定时器的target参数，但NSTimer底层的实现机制会强制对target产生强引用。这是因为`scheduledTimerWithTimeInterval:target:selector:userInfo:repeats:`方法的实现逻辑不会考虑传入target的内存管理修饰符，无论传入weakSelf还是self，最终都会被定时器强持有。

> [考察内容] 此题主要检测开发者对NSTimer工作机制的理解程度。需要特别指出：weak关键字在block中可以改变引用关系，但这里的target参数不涉及block语义。同时需要对比block和普通方法参数的区别："block在捕获外部变量时会根据修饰符决定引用强弱，而NSTimer的target参数处理机制完全不关心传入的指针修饰符"。

## 112. 如何通过消息转发机制实现NSTimer循环引用的解决方案？

核心思路是引入中间代理对象weakProxy：

1. 创建proxy类弱引用原始target（通常的控制器）
2. 将proxy设置为定时器的target
3. 通过`forwardingTargetForSelector:`方法将消息转发给真实target

代码实现示例：

```objc
// FFProxy.h
@interface FFProxy : NSObject
+ (instancetype)proxyWithTarget:(id)target;
@end

// FFProxy.m
@interface FFProxy()
@property (nonatomic, weak) id target;
@end

@implementation FFProxy

+ (instancetype)proxyWithTarget:(id)target {
    FFProxy *proxy = [FFProxy new];
    proxy.target = target;
    return proxy;
}

- (id)forwardingTargetForSelector:(SEL)aSelector {
    return self.target; // 将消息转发给原始target
}
@end

// 使用示例
FFProxy *proxy = [FFProxy proxyWithTarget:self];
self.timer = [NSTimer scheduledTimerWithTimeInterval:1 
                                              target:proxy 
                                            selector:@selector(showMsg) 
                                            userInfo:nil 
                                             repeats:YES];
```

形成引用链：controller强持有timer → timer强持有proxy → proxy弱持有controller → 打破循环引用链。当controller销毁时会自动触发timer的释放流程。

> [技术细节] `forwardingTargetForSelector:`是消息转发机制的第一阶段，若当前对象（proxy）无法响应消息，可以将消息转发给其他对象处理。此处确保所有selector都由原始target响应，使proxy成为透明的传输层。

## 113. iOS中常见的应用导航模式有哪些？请举例说明具体实现方式

主要分为三种基础导航模式：

1. 平铺导航（Flat Navigation）：适用于屏幕内容无层次关系，通过分页控件展示。例如天气应用的当前视图展示，核心实现通常是UIScrollView + UIPageControl。每个子页面可能对应独立的视图控制器，通过父容器管理。

2. 标签导航（Tab Navigation）：模块功能相互独立，通过底部标签切换。使用UITabBarController实现，例如微信的消息/通讯录/发现/我模块。每个标签对应一个导航控制器或普通视图控制器，通过设置`viewControllers`属性配置。

3. 树形导航（Hierarchical Navigation）：适用于多层级数据展示，使用UINavigationController组织视图控制器的堆栈结构。典型如邮件应用：收件箱列表 -> 邮件详情 -> 附件预览的多级层次。通过push/pop操作切换视图。

现代iOS开发中还常见：

- 分页导航（Page-based）：类似于电子书翻页效果，使用UIPageViewController实现
- 组合导航（Hybrid Navigation）：例如Tab + Navigation的混合使用（几乎90%以上的App使用这种模式）

> [实现示例] 树形导航的典型代码结构：

```objc
// AppDelegate.m
UIViewController *rootVC = [[HomeViewController alloc] init];
UINavigationController *navController = [[UINavigationController alloc] 
                                         initWithRootViewController:rootVC];
self.window.rootViewController = navController;

// 在HomeViewController中跳转详情
DetailViewController *detailVC = [DetailViewController new];
[self.navigationController pushViewController:detailVC animated:YES];
```

## 114. 堆（Heap）与栈（Stack）在内存管理上有哪些核心区别？

核心区别体现在五个维度：

1. 内存分配方式：
   - 堆采用动态分配（dynamic allocation）方式，开发者主动通过alloc/malloc等方法申请内存
   - 栈主要采用自动分配方式，存储局部变量和函数调用上下文，由编译器管理内存分配回收

2. 生命周期管理：
   - 堆内存需要开发者手动管理，ObjC中通过引用计数机制自动管理，但仍可能因循环引用导致内存泄漏
   - 栈内存生命周期与作用域绑定，函数返回后自动释放，变量出作用域后立即销毁

3. 空间大小限制：
   - 堆大小受设备可用物理内存限制，理论上可以申请大块内存（实际受系统限制）
   - 栈空间有限（iOS主线程栈默认1MB，子线程512KB），深层递归可能引发栈溢出

4. 访问效率对比：
   - 栈通过指针简单移动即可分配内存，访问速度更快
   - 堆需要维护复杂的内存分配表和引用计数，访问效率较低

5. 碎片问题：
   - 栈的先进后出模式不会有内存碎片
   - 频繁的堆内存分配释放会产生碎片，需要垃圾回收或ARC机制优化

代码示例说明存储差异：

```objc
// 栈分配（静态分配）
- (void)exampleMethod {
    int localInt = 42;            // 栈存储基本类型
    NSObject *localObj = nil;     // 指针变量本身在栈，对象在堆
}

// 堆分配（动态分配）
NSObject *heapObj = [[NSObject alloc] init]; // alloc在堆中分配内存
```

> [深入分析] 在Objective-C中，虽然对象全部存储在堆中，但指向对象的指针变量本身可能存储在栈或堆中。这取决于指针的声明位置——函数内部的局部指针变量存储在栈，而作为实例变量的指针则存储在对象所在的堆内存中。
