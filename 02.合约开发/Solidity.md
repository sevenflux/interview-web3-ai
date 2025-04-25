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

 **总结**

- `private`：只能在合约内部调用，子合约不能访问。

- `internal`：只能在合约内部或继承的子合约中调用。

- `public`：可以被任何人、包括合约内外部调用。

- `external`：只能从合约外部调用，不能在合约内部直接调用（除非通过 `this` 关键字）。



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









