# Solidity

[TOC]



## 1. 私有、内部、公共和外部函数之间的区别

在Solidity中，函数的可见性（Visibility）定义了谁可以访问该函数。主要有四种可见性修饰符：`private`（私有）、`internal`（内部）、`public`（公共）和`external`（外部）。它们的区别如下：

- **private（私有）**

  - 只能在定义该函数的合约内部调用，不能被外部调用。

  - 继承该合约的子合约无法访问`private`函数。

    ```solidity
    contract MyContract {
    	uint private data;
    	function privateFunction() private {
    		// 只能在这个合约内部调用    
    	}
    }
    ```

- **internal（内部）**

  - 可以在定义该函数的合约内部调用，也不能外部调用。

  - 继承该合约的子合约可以访问`internal`函数。

  - `internal`是默认的函数可见性修饰符。

    ```solidity
    contract MyContract {
    	uint internal data;
    	function internalFunction() internal {
      // 可以在这个合约和子合约中调用    
      }
    }
    ```

- **public（公共）**

  - 可以在合约内部、继承合约的子合约中、以及外部（例如通过交易或外部合约调用）访问。

  - 从合约外部调用public函数时，Solidity会生成一个与该public函数相对应的外部接口，使其能够被外部调用。

  - 编译器会自动为public状态变量创建getter函数。

    ```solidity
    contract MyContract {    
    	uint public data; 
    	// 自动生成一个 getter 函数    
    	function publicFunction() public {        
    		// 任何人都可以调用    
    	}
    }
    ```

- **external（外部）**

  - 只能从合约外部调用。不能从合约的内部调用该函数（但可以使用`this.functionName()`调用）。

  - 比`public`函数更加节省gas，因为参数不需要从内存复制到`calldata`。

    ```solidity
    contract MyContract {   
    	uint public data;    
    	function externalFunction() external {   
      // 只能通过外部调用   
      }
    }
    ```


 **总结**

- `private`：只能在合约内部调用，子合约不能访问。

- `internal`：只能在合约内部或继承的子合约中调用。

- `public`：可以被任何人、包括合约内外部调用。

- `external`：只能从合约外部调用，不能在合约内部直接调用（除非通过 `this` 关键字）。



## 2. 为什么`external`比`public`函数更加节省gas？

让我们看一个示例：

```solidity
contract GasComparison {    
// Public function    
	function publicFunction(uint[] memory data) public pure returns (uint) {        
		return data.length;    
	}    
// External function    
	function externalFunction(uint[] calldata data) external pure returns (uint) {        
		return data.length;   
	}
}
// 调用示例
contract Caller {    
	GasComparison public gc = new GasComparison(); 
	
  function callPublic(uint[] memory data) public {  
  	gc.publicFunction(data);  
    }    
    
  function callExternal(uint[] memory data) public {    
 		gc.externalFunction(data);  
  }
}
```

这涉及Solidity中的内存管理和gas优化：

1. 内存(memory) vs 调用数据(calldata):
   - `memory`是函数执行期间存储数据的临时区域。
   - `calldata`是只读的输入数据位置，不会被复制。

2. public函数的行为:
   - 参数会复制到内存中，因此消耗更多gas。

3. external函数的行为:
   - 直接从`calldata`读取数据，避免了复制步骤，节省gas。

4. Gas节省:
   - 复制大型数组或结构体到内存是昂贵的操作，`external`函数通过读取`calldata`节省了gas。

5. 使用场景:
   - `external`函数适合处理大型数据输入。

## 3. 什么时候使用`memory`修饰符，什么时候使用`calldata`修饰符？

在Solidity中，`memory`和`calldata`是两种用于指定数据存储位置的关键字，它们在函数参数和局部变量中的使用有所不同。理解何时使用`memory`和`calldata`对优化gas消耗和确保正确的数据处理非常重要。

**`memory` 修饰符**

- 临时数据存储：`memory`表示数据只在函数执行期间临时存储。函数执行完毕后，这些数据会被自动销毁。

- 可读可写：`memory`中的数据是可变的（即可以修改）。如果你需要在函数中对数据进行修改，通常使用`memory`。

- 常用于内部处理：当你在函数内部处理数据并且需要修改这些数据（例如复制、排序、变换），你会将数据存储在`memory`中。

- **使用场景**

  1. 需要修改参数数据：如果你需要在函数内部修改传入的参数数据，使用`memory`。

  2. 创建 局部变量：当你需要创建一个临时的数组、结构体或其他复杂数据类型用于内部计算时，使用`memory`。

     ```solidity
     function modifyData(uint[] memory data) public { 
     	// 这里 data 是可变的，可以被修改   
     	data[0] = 42;
     }
     ```

 **`calldata` 修饰符**

- 只读 数据存储：`calldata`表示数据直接从调用者的输入中读取（即从事务的输入数据中读取），它是只读的，无法修改。

- 节省gas：因为`calldata`中的数据不需要在内存中进行复制，且是只读的，所以在处理较大数据结构（如数组）时，使用`calldata`可以节省gas。

- 只能用于外部函数的参数：`calldata`只能用于外部函数（`external`函数）的参数，因为它直接映射到事务的输入数据。

  - 使用场景

  1. 外部函数的 只读 参数：如果函数参数只会被读取，而不会被修改，且这个函数是`external`的，使用`calldata`。

  2. 优化gas消耗：当处理大数据结构时，使用`calldata`可以避免不必要的内存复制，从而节省gas。

     ```solidity
     function processData(uint[] calldata data) external { 
     	// 这里 data 是只读的，无法修改   
       uint value = data[0];
     }
     ```

 **`memory` vs `calldata` 总结**

使用`memory`：

- 当你需要在函数内部修改传入的数据。

- 当你需要在内部创建和使用复杂的数据结构（如数组、结构体）时。

- 当函数是`public`或`internal`，而非`external`。

使用`calldata`：

- 当数据不需要被修改，仅用于读取。

- 当你希望在外部函数（`external`）中处理大数据结构且需要优化gas消耗时。

**举例对比**

```solidity
contract Example {
	// 使用 memory：数据可以被修改
	function modifyMemory(uint[] memory data) public {
    data[0] = 1; 
    // 修改了传入的数据
	}
	// 使用 calldata：数据不可修改，但节省gas
	function readCalldata(uint[] calldata data) external {
		uint value = data[0]; 
		// 只能读取，不能修改
	}
}
```



通过合理使用`memory`和`calldata`，你可以在优化gas消耗的同时确保函数的正确性和效率。



## 4. Solidity智能合约的pure与view使用原理及场景

**pure 与 view 原理**

**pure**: 不读取更不修改区块上的变量，使用本机的CPU资源计算我们的函数。所以不消耗任何的资源这是很容易的理解的。

**view**: 但是 view 既然要读取区块链上的值，为什么也不用消耗 gas 呢？

其实很简单，因为作为一个全节点来说，会同步保存所有的信息，保存在本地中。

那么我们要查看区块链上的资源，同样可以直接在一个全节点之上查询数据即可。

我不需要全世界的节点都知道。都去同时的处理这笔事务。我也不需要将调用这笔函数的信息记录在区块链上。

所以 view 仍然不消耗 gas。

**使用场景**

**view:** 可以自由调用，因为它只是“查看”区块链的状态而不改变它

**pure:** 也可以自由调用，既不读取也不写入区块链

要将固定长度的字节数组转换为动态长度的字节数组，需要首先创建动态数组，并挨个赋值。

```solidity
pragma solidity ^0.4.23;
contract  fixTodynamic{     
	bytes6 name =  0x6a6f6e736f6e;   
  function  Todynamic() view public returns(bytes){
  //return bytes(name);        
  bytes memory newName = new bytes(name.length);
  //for循环挨个赋值      
  for(uint i = 0;i<name.length;i++){      
  	newName[i] =  name[i];    
  }      
  return newName;   
  }
}
```



## 5. 简单说明智能合约的构造函数和初始化函数的特性与区别

- 构造函数: 在合约部署时调用，仅用于此时初始化状态变量。

- 初始化函数: 手动调用一次，用于初始化合约。



## 6. solidity staticcall 的原理与作用

Solidity的staticcall方法是一种用于在以太坊虚拟机（EVM）上执行不改变状态的外部智能合约的方法。下面是staticcall方法的底层代码实现：

```solidity
function staticcall(
  address target,
  bytes memory data
) internal view returns (bool success, bytes memory returnData) {
  assembly {
    let result := staticcall(gas(), target, add(data, 0x20), mload(data), 0, 0)
    let size := returndatasize()
    returnData := mload(0x40) // allocate new memory
    mstore(0x40, add(returnData, and(add(add(size, 0x20), 0x1f), not(0x1f)))) // round up to nearest 32 bytes
    mstore(returnData, size) // set length of returned data
    returndatacopy(add(returnData, 0x20), 0, size)
    success := and(result, not(iszero(size)))
  }
}
```



这个实现使用了EVM汇编代码。首先，staticcall函数将目标地址和要执行的字节码作为输入参数。然后，它通过使用staticcall指令在EVM上执行外部智能合约。这个指令接受五个参数：gas限制、目标地址、输入数据的起始位置、输入数据的长度和输出数据的起始位置。这个函数没有改变调用者的状态。

在assembly块内，staticcall指令的结果保存在result变量中。然后，returndatasize指令用于获取返回数据的长度。接下来，这个实现通过使用EVM汇编代码来分配新内存、设置返回数据的长度和复制返回数据。最后，它将成功的状态和返回数据作为输出返回给调用者。

总体来说，这个底层实现使用了EVM汇编代码，它使用了一些指令来处理外部智能合约的调用和返回数据的处理，以便实现staticcall方法。

Solidity中的staticcall函数是一种特殊类型的函数调用，它允许在以太坊虚拟机上执行一个不修改状态的智能合约函数。它的作用是查询一个智能合约中的数据或计算某个状态而不会改变区块链上的状态。

在staticcall函数中，虚拟机会创建一个新的临时账户并将调用数据传递给该账户。然后，该账户的代码将被执行，但是无法修改状态或者调用具有状态变更的函数。这个过程不会消耗gas，因为没有状态变更。

staticcall函数的返回值是一个布尔值，用来表示调用是否成功。如果成功，将返回该函数的返回值，否则返回一个空的字节数组。

staticcall的常用场景是从智能合约中读取数据。例如，当需要在一个合约中获取另一个合约的某些数据时，可以使用staticcall函数来查询数据，而不必调用具有状态变更的函数，这样可以避免额外的gas费用和状态变更。



## 7. solidity 中 assembly 原理与作用

Solidity是一种高级编程语言，用于编写智能合约。它基于EVM（以太坊虚拟机）的字节码指令集，但是有时候需要进行更底层的操作，这时候就可以使用Solidity中的Assembly语言。

Assembly是一种低级语言，它直接操作EVM指令集。Solidity中的Assembly可以用来执行一些高级操作，比如内联汇编，也可以用来访问合约存储空间和进行低级别的内存操作。此外，Assembly还可以用于优化代码和执行一些高级加密算法等。

Solidity中的Assembly代码可以通过在函数声明中使用"assembly"关键字来定义。Assembly代码使用一种类似于汇编语言的语法，并使用特定的指令集来操作EVM。Assembly语言还可以直接读写内存和存储器，以及进行其他底层操作。

总之，Solidity中的Assembly可以提供更高级别的操作和更佳的性能，但它也需要更多的编写经验和技能，因为它需要直接操作EVM指令集。对于大多数智能合约，Solidity的高级语言已经足够，不需要使用Assembly。



## 8. `require`, `assert`和`revert`的区别

- `require`: 用于输入验证和状态检查，失败时返回剩余 gas 并恢复状态。
- `assert`: 用于检测不变量和内部错误，失败时消耗所有剩余 gas 并恢复状态。
- `revert`: 用于显式抛出异常，支持携带错误信息，失败时返回剩余 gas 并恢复状态。



## 9. 分析下面代码是否存在内存对齐问题



**题一**

V1 版本的合约, 运行很久了

```solidity
contract A {
   uint256 public A;
   uint256 public B;
}
```

V2 版本的合约

```solidity
contract A {
   uint256 public A;
   uint256 public C;   
   uint256 public B;
}
```

V2的改动存在缺陷，C 状态变量占用了 B 的 slot



**题二**

V1 版本的合约, 运行很久了

```solidity
contract A {
 struct AAA {
     uint256 public A;
     uint256 public B;   
  }
  uint256 public Z;
  mapping(uint256=>AAA) ZZZ;
  uint256 public C;
  uint256 public D;
}
```

V2 版本的合约

```solidity
contract A {
 struct AAA {
     uint256 public A;
     uint256 public B;
     uint256 public X;
     uint256 public Y;
  }
  uint256 public Z;
  mapping(uint256=>AAA) ZZZ;
  uint256 public C;
  uint256 public D;
}
```

V2的改动没有问题



## 10. 如何在 Solidity 中编写节省gas的高效循环？

1. 避免不必要的循环或无限循环：始终确保循环有明确的终止条件，避免因 gas 消耗过多导致交易失败。
2. 减少重复计算：将循环中重复使用的表达式提取为临时变量，避免每次迭代重复计算，节省计算成本。
3. 避免使用昂贵的运算符：如除法（`/`）和取模（`%`）等操作比加法、减法更耗 Gas，能用位运算或其他替代方法时应优先使用。
4. 尽量避免大型数组操作：遍历大型数组非常昂贵。可以使用 `mapping` 等更高效的数据结构，或在链下处理数据后再上链。
5. 避免嵌套循环：嵌套循环的复杂度会导致 Gas 成本呈指数增长，除非必要，应尽量展平逻辑或拆分成多个函数执行。
6. 使用 `constant` 和 `immutable`：将不会改变的值声明为 `constant` 或 `immutable`，可减少存储读取开销，在循环中尤其有效。



## 11. 描述 Solidity 中三种与 gas 成本相关的存储类型

Solidity 中常见的三种存储类型为 **storage**、**memory** 和 **calldata**，它们各自的 gas 成本和用途如下：

1. **Storage（存储）变量**：
    Storage 是合约的永久存储区域，通常用于保存状态变量。写入 Storage 的操作 gas 成本非常高，因为数据会持久化到区块链。例如，修改一个 `uint256` 状态变量的值可能消耗上千 gas。读取 Storage 虽然相对便宜，但仍比 Memory 和 Calldata 更昂贵。
2. **Memory（内存）变量**：
    Memory 是函数调用期间的临时内存区域，用于临时变量或函数参数的复制。Memory 中的读写操作比 Storage 更便宜，但在函数执行结束后会被释放。Memory 更适用于处理中间计算数据或返回值。
3. **Calldata（调用数据）变量**：
    Calldata 是函数参数的只读存储区域，特别用于外部函数的输入参数。它不消耗额外的复制成本，适合处理外部输入数据。由于是只读的，不能修改，但它的 gas 成本是三者中最低的，适合用于 `view` 或 `pure` 函数参数传递。



## 12. 如果代理合约对 A 合约执行 `delegatecall`，而 A 合约内部执行 `address(this).balance`，那么返回的是代理合约的余额还是 A 合约的余额？


 返回的是**代理合约的余额**。
 在 `delegatecall` 中，被调用的合约（A）会在调用合约（代理）的上下文中执行，这意味着：

- 被调用合约使用的是调用者的存储（storage）、地址（address）和余额（balance）；
- 因此，`address(this)` 在 A 合约中依然表示的是代理合约的地址；
- 所以，`address(this).balance` 实际上是读取代理合约的以太余额。

这种机制是 `delegatecall` 用于构建可升级合约（如 Proxy 模式）时的基础原理。



## 13. **乘以或除以 2 的倍数，在 Solidity 中的 gas 高效替代方法是什么？**


 在 Solidity 中，将一个整数乘以或除以 2 的幂，可以使用**位移运算（shift operations）**来实现，从而获得更高的 gas 效率：

- `x << n` 相当于 `x * 2^n`
- `x >> n` 相当于 `x / 2^n`（对整数向下取整）

例如：

```solidity
uint256 x = 64;
uint256 a = x << 3; // 等同于 64 * 8
uint256 b = x >> 2; // 等同于 64 / 4
```

与乘法或除法相比，位移操作在 EVM 中的执行成本更低。

从 **Solidity 0.8.3** 起，**编译器可能自动将乘除以 2 的幂转换为位移指令**，但这取决于表达式的上下文和常量可解析性。手动使用移位运算仍然可以提高可读性和性能控制力。

**注意事项：**

- 位移操作不会自动进行溢出检查（但 Solidity 0.8 默认对算术运算进行检查），需要注意边界安全。
- 对于负数使用移位需格外小心（只适用于有符号类型 `int`）。
