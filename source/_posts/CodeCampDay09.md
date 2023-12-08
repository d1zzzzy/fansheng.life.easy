---
title: 代码随想录算法训练营第九天| 用栈实现队列、用队列实现栈
date: 2023-12-08 17:45:30
tags:
  - Algorithm
  - Stack
  - Queue
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_01.webp
---

# 代码随想录算法训练营第九天

## 栈

> 栈是一种遵循后进先出（LIFO）原则的有序集合。新添加的或待删除的元素都保存在栈的同一端，称作栈顶，另一端就叫栈底。在栈里，新元素都靠近栈顶，旧元素都接近栈底。

## 队列

> 队列是遵循先进先出（FIFO）原则的一组有序的项。队列在尾部添加新元素，并从顶部移除元素。最新添加的元素必须排在队列的末尾。

## 用栈实现队列

> 题目描述：
> 
> 使用栈实现队列的下列操作：
> 
> 请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作`（push、pop、peek、empty）`;

### 思路

要用栈实现队列，你需要两个栈。栈是一种后进先出（LIFO）的数据结构，而队列是一种先进先出（FIFO）的数据结构。使用两个栈，你可以通过反转元素的顺序来模拟队列的行为。一个栈用于入队操作（我们称之为“入栈”），另一个栈用于出队操作（称之为“出栈”）。

### 代码实现

```typescript
class MyQueue {
	private inStack: number[];
	private outStack: number[];

	constructor() {
		this.inStack = [];
		this.outStack = [];
	}

	push(x: number): void {
		this.inStack.push(x);
	}

	pop(): number {
		if (this.outStack.length === 0) {
			while (this.inStack.length !== 0) {
				this.outStack.push(this.inStack.pop()!);
			}
		}
		return this.outStack.pop()!;
	}

	peek(): number {
		if (this.outStack.length === 0) {
			while (this.inStack.length !== 0) {
				this.outStack.push(this.inStack.pop()!);
			}
		}
		return this.outStack[this.outStack.length - 1];
	}

	empty(): boolean {
		return this.inStack.length === 0 && this.outStack.length === 0;
	}
}
```

## 用队列实现栈

> 题目描述：
> 
> 请你仅使用两个队列实现一个后入先出（LIFO）的栈，并支持普通栈的全部四种操作`（push、top、pop 和 empty）`。

### 思路

要用队列实现栈，你需要使用至少两个队列。队列是一种先进先出（FIFO）的数据结构，而栈是一种后进先出（LIFO）的数据结构。使用两个队列，你可以通过交换元素的顺序来模拟栈的行为。一个队列用于存储栈的内容（我们称之为“主队列”），另一个队列用于辅助操作（称之为“辅助队列”）

### 代码实现

```typescript
class MyStack {
  private mainQueue: number[];
  private auxQueue: number[];

  constructor() {
    this.mainQueue = [];
    this.auxQueue = [];
  }

  push(x: number): void {
    this.mainQueue.push(x);
  }

  pop(): number {
    while (this.mainQueue.length > 1) {
      this.auxQueue.push(this.mainQueue.shift()!);
    }
    let poppedItem = this.mainQueue.shift();
    [this.mainQueue, this.auxQueue] = [this.auxQueue, this.mainQueue];
    return poppedItem!;
  }

  top(): number {
    while (this.mainQueue.length > 1) {
      this.auxQueue.push(this.mainQueue.shift()!);
    }
    let topItem = this.mainQueue[0];
    this.auxQueue.push(this.mainQueue.shift()!);
    [this.mainQueue, this.auxQueue] = [this.auxQueue, this.mainQueue];
    return topItem!;
  }

  empty(): boolean {
    return this.mainQueue.length === 0;
  }
}
```

## 总结

### 相同点

+ **线性结构**：栈和队列都是线性数据结构，即它们以序列的形式存储元素。
+ **动态大小**：它们通常能够根据需要动态地增长和缩减大小。
+ **支持基本操作**：两者都支持添加和移除元素的基本操作。

### 不同点

1. 顺序不同：
	+ 栈：遵循后进先出（LIFO, Last-In-First-Out）的原则。最后添加进栈的元素会最先被移除。
	+ 队列：遵循先进先出（FIFO, First-In-First-Out）的原则。最先添加进队列的元素会最先被移除。
2. 用途不同：
	+ 栈：通常用于处理具有最近相关性的场景，如函数调用（调用栈）、撤销操作、括号匹配等。
	+ 队列：常用于处理按顺序处理的任务，如打印任务队列、线程池任务队列、数据缓冲区等。
3. 操作名称不同： 
   + 栈：通常有 push（添加）和 pop（移除）操作。
   + 队列：通常有 enqueue（入队）和 dequeue（出队）操作。
4. 应用场景：
   + 栈：适用于需要后进先出处理的场景，例如递归实现、后缀表达式计算、历史记录等。
   + 队列：适用于需要按顺序处理的场景，例如任务调度、消息处理、数据流管理等。 
5. 操作限制： 
   + 栈：只能在一端（通常称为栈顶）进行添加和移除操作。
   + 队列：在一端添加元素，在另一端移除元素。
