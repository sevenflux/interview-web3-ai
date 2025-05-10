# Node.js

[TOC]





## 1. 以下代码输出的结果是什么？



```javascript
try {
  throw new Error('1')
} catch (error) {
  console.log('error')
}

// 输出结果：error
// 说明: `throw` 抛出的异常会被 `catch` 捕获，因此控制台输出 `"error"`。
```



```javascript
try {
  setTimeout(function() {
    console.log('b');
  }, 0);
} catch (error) {
  console.log('error');
}
console.log('out try catch');

// 输出结果: out try catch
// 说明： `setTimeout` 是异步函数，其回调不会在 `try...catch` 中立即执行，因此不会被捕获。控制台同步输出 `"out try catch"`，而 `"b"` 会在事件循环后异步输出，但题目只要求同步输出。
```



```javascript
try {
  new Promise(() => {
    throw new Error('new promise throw error');
  });
} catch (error) {
  console.log('error');
}

// 输出结果: 无输出或警告
// 说明： `Promise` 构造器内部抛出的异常是异步处理的，`catch` 无法捕获，因此控制台不会输出 `"error"`，也不会抛出同步异常。你原答案错误。
```



```javascript
console.log('Start');
setTimeout(() => {
  console.log('Timeout');
}, 0);
Promise.resolve().then(() => {
  console.log('Promise resolved');
});
console.log('End');
// 输出结果: 
// Start
// End
// Promise resolved
// Timeout
// 说明： 执行顺序为：同步代码 → 微任务（Promise）→ 宏任务（setTimeout）。
```



## 2. 事件循环机制的作用是什么？

事件循环机制用于协调 JavaScript 的同步与异步执行流程，决定异步任务（如定时器、Promise、I/O 等）的回调何时被放回主线程执行队列。

事件循环机制用于管理异步API的回调函数什么时候回到主线程中执行。

Node.js的事件循环基于 libuv，实现异步 I/O 模型。同步API在主线程中执行，异步API在底层的C++维护的线程中执行，异步API的回调函数也会在主线程中执行。

在Javascript应用运行时，众多异步API的回调函数什么时候能回到主线程中调用呢？这就是事件环环机制做的事情，管理异步API的回调函数什么时候回到主线程中执行。

------

## 3. EventLoop 的六个阶段分别是什么？

1. **Timers**：执行定时器（`setTimeout`、`setInterval`）的回调。

2. **Pending Callbacks**：处理上一轮循环中延迟执行的系统回调。

3. **Idle, Prepare**：仅供内部使用。

4. **Poll**：处理 I/O 事件的回调（如文件读取），并决定是否继续等待或进入下一个阶段。

   - 在这个阶段需要特别注意，如果事件队列中有回调函数，则执行它们直到清空队列，否则事件循环将在此阶段停留一段时间以等待新的回调函数进入。

     但是对于这个等待并不是一定的，而是取决于以下两个条件：

     - 如果setlmmediate队列（check阶段）中存在要执行的调函数。这种情况就不会等待。
     - timers队列中存在要执行的回调函数，在这种情况下也不会等待。事件循环将移至check阶段，然后移至Closingcallbacks阶段，并最终从timers阶段进入下一次循环。

5. **Check**：执行 `setImmediate` 的回调。

6. **Close Callbacks**：处理如 `socket.on('close')` 的关闭回调。

------

## 4. Node.js 中的宏任务与微任务分别有哪些？

**宏任务（Macro Task）包括：**

- `setTimeout`
- `setInterval`
- `setImmediate`
- `I/O 回调`

**微任务（Micro Task）包括：**

- `Promise.then/catch/finally`
- `process.nextTick`

------

## 5. 简单说明微任务和宏任务的执行顺序？

在node中，微任务的回调函数被放置在微任务队列中，宏任务的回调函数被放置在宏任务队列中。

微任务优先级高于宏任务。当微任务事件队列中存在可以执行的回调函数时，事件循环在执行完当前阶段的回调函数后会暂停进入事件循环的下一个阶段，而会立即进入微任务的事件队列中开始执行回调函数，当微任务队列中的回调函数执行完成后，事件循环才会进入到下一个段开始执行回调函数。

对于微任务我们还有个点需要特别注意。那就是虽然nextTick同属于微任务，但是它的优先级是高于其它微任务，在执行微任务时，只有nextlick中的所有回调函数执行完成后才会开始执行其它微任务。

总的来说就是当主线程同步代码执行完毕后会优先清空微任务(如果微任务继续产生微任务则会再次清空)，然后再到下个事件循环阶段。并且微任务的执行是穿插在事件循环六个阶段中间的，也就是每次事件循环进入下个阶段前会判断微任务队列是否为空，为空才会进入下个阶段，否则先清空微任务队列。



------

## 6. ES6 有哪些新特性？

- 块级作用域声明（`let`、`const`）
- 箭头函数
- 模板字符串
- 解构赋值
- 默认参数、剩余参数、扩展运算符
- `for...of` 循环
- 类（`class`）与继承
- 模块化（`import` / `export`）
- `Promise` 异步编程
- `Map` / `Set` 数据结构
- `Generator` 函数

------

## 7. JavaScript 类继承的方法有哪些？

1. **原型链继承**

```javascript
function Animal() {}
Animal.prototype.say = function() { console.log('animal'); };

function Dog() {}
Dog.prototype = new Animal();
Dog.prototype.constructor = Dog;
```

2. **构造函数继承**

```javascript
function Animal(name) {
  this.name = name;
}
function Dog(name) {
  Animal.call(this, name);
}
```

3. **属性复制**

```javascript
function Animal() {
	this.name = 'animal';
}
Animal.prototype.sayName = function() {
	alert(this.name);
};

function Person() {}

for(prop in Animal.prototype) {
	Person.prototype[prop] = Animal.prototype[prop];
} // 复制动物的所有属性到人量边
Person.prototype.constructor = 'Person'; // 更新构造函数为人
```

4. **组合继承**

```javascript
function Animal(name) {
  this.name = name;
}
Animal.prototype.say = function() { console.log(this.name); };

function Dog(name) {
  Animal.call(this, name);
}
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
```

5. **ES6 `extends`**

```javascript
class Animal {
  constructor(name) { this.name = name; }
}
class Dog extends Animal {
  constructor(name) { super(name); }
}
```

------

## 8. JS 中 `this` 的指向是什么？

 `this` 的指向取决于函数的调用方式：

- 全局环境中：指向 `global`（Node）或 `window`（浏览器）
- 对象方法调用：指向该对象
- 构造函数：指向新实例
- 箭头函数：不绑定自己的 `this`，取决于定义时的上下文
- 显式绑定：使用 `call`、`apply`、`bind` 指定

------

## 9. `apply`、`call` 和 `bind` 有什么区别？

- `apply(thisArg, [args])`：以数组方式传参并立即调用
- `call(thisArg, arg1, arg2, ...)`：逐个参数传参并立即调用
- `bind(thisArg, ...)`：返回绑定后函数，需手动调用

**示例：**

```javascript
function greet(greeting) {
  console.log(greeting + ', ' + this.name);
}
const obj = { name: 'Alice' };

greet.call(obj, 'Hi');         // Hi, Alice
greet.apply(obj, ['Hello']);   // Hello, Alice
const bound = greet.bind(obj);
bound('Hey');                  // Hey, Alice
```

------

## 10. `caller`、`callee` 和 `arguments` 是什么？

- `arguments`：类数组对象，包含函数参数
- `callee`：`arguments` 的属性，指向当前函数（严格模式下禁用）
- `caller`：指向调用当前函数的函数（非标准、已废弃）

**示例：**

```javascript
function foo() {
  console.log(arguments.callee); // 指向 foo 本身（非严格模式）
}
```

------

## 11. Node.js 的架构是什么？

 Node.js 的架构主要分为三层：

1. **应用层**：JavaScript 编写的应用代码
2. **Node.js 核心模块层**：
   - JavaScript 实现的模块（如 fs、http）
   - C++ 扩展与绑定层（连接 JavaScript 与底层库）
3. **底层库与系统调用层**：
   - `libuv`：事件循环和线程池
   - `OpenSSL`、`zlib` 等库
   - 操作系统内核接口

------

## 12. ES6 中 `Map` 和 `Set` 有什么区别？

| 特性     | Map                   | Set              |
| -------- | --------------------- | ---------------- |
| 存储结构 | 键值对                | 单一值           |
| 键名类型 | 可为任意类型          | 无键名，仅值唯一 |
| 是否重复 | 可重复键（后者覆盖）  | 值唯一           |
| 遍历方式 | `forEach`, `for...of` | 同上             |



------

## 13. 简述 `Set`、`WeakSet`、`Map`、`WeakMap` 的作用和区别

**Set**：

- 唯一值集合
- 可遍历
- 适合数组去重

**WeakSet**：

- 仅接受对象
- 不可遍历
- 对象是弱引用，避免内存泄漏

**Map**：

- 键值对集合
- 任意类型作键
- 可遍历，支持转换为数组

**WeakMap**：

- 仅对象作键（不可枚举）
- 键是弱引用，适用于私有数据存储