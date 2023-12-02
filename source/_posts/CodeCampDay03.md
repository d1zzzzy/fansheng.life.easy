---
title: 代码随想录算法训练营第三天| 长度最小的子数组、有序数组的平方、螺旋矩阵II
date: 2023-12-01 15:42:54
tags:
	- Algorithm
	- LinkedList
categories:
	- [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/code-camp-day03.png
---

# 代码随想录算法训练营第三天

## 链表

> 链表是一种常见的基础数据结构，区别于数组，链表的每个元素不是存储在连续的内存空间中，而是通过指针将不连续的内存空间串联起来使用。
> 在 JS 中，链表的实现可以通过对象的引用来实现，也可以通过数组的下标来实现。
> 链表的大多数操作都是通过指针来实现，所以在实现链表的时候，需要注意指针的指向问题。

### 203.移除链表元素

> 题目描述：
> 
> 给你一个链表的头节点 `head` 和一个整数 `val` ，请你删除链表中所有满足 `Node.val == val` 的节点，并返回 新的头节点 。

#### 思路

遍历链表，如果当前节点下一个节点的值为目标值的话，用一个节点保存之后的地址，然后通过指针跳过那个节点。

#### 代码实现

```typescript
function removeElements(head, val) {
  // 新建一个虚拟指针，将该指针指向 head
	// 如此即便 head 为空，我们也可以通过 虚拟指针进行遍历
  const dummyHeader = new ListNode(0, head);

  // 用于遍历指针，初始值指向 头指针
  let pointer = dummyHeader;

  // 循环终止条件
  while(pointer.next) {
    // 如果当前节点的下一个节点为目标值
    if (pointer.next && pointer.next.val === val) {
	    // 修改指针，跳过下一个节点（即达到删除的目的）
      pointer.next = pointer.next.next;
      continue;
    }

    pointer = pointer.next;
  }

  return dummyHeader.next;
};
```
#### 复杂度分析

+ 时间复杂度：O(n)
+ 空间复杂度：O(1)

### 707.设计链表  

> 题目描述：
> 
> 你可以选择使用单链表或者双链表，设计并实现自己的链表。
>
> 单链表中的节点应该具备两个属性：val 和 next 。val 是当前节点的值，next 是指向下一个节点的指针/引用。
>
> 如果是双向链表，则还需要属性 prev 以指示链表中的上一个节点。假设链表中的所有节点下标从 0 开始。
> 
> 实现 MyLinkedList 类：
>
> MyLinkedList() 初始化 MyLinkedList 对象。
> int get(int index) 获取链表中下标为 index 的节点的值。如果下标无效，则返回 -1 。
> void addAtHead(int val) 将一个值为 val 的节点插入到链表中第一个元素之前。在插入完成后，新节点会成为链表的第一个节点。
> void addAtTail(int val) 将一个值为 val 的节点追加到链表中作为链表的最后一个元素。
> void addAtIndex(int index, int val) 将一个值为 val 的节点插入到链表中下标为 index 的节点之前。如果 index 等于链表的长度，那么该节点会被追加到链表的末尾。如果 index 比长度更大，该节点将 不会插入 到链表中。
> void deleteAtIndex(int index) 如果下标有效，则删除链表中下标为 index 的节点。

#### 已知

1. 可以使用单链表或者双链表：使用两种不同的链表后续对链表的操作也不一样
2. 链表的结构

题目目的是去熟悉链表的基础操作。

#### 代码实现

##### 单链表

```typescript
	class Node {
		value: number;
		next: Node | null = null;

		constructor(value: number) {
			this.value = value;
		}
	}
    
	class MyLinkedList {
		head: Node | null = null;
		length: number = 0;

		constructor() {}

		get(index: number): number {
			if (index < 0 || index >= this.length) return -1;

			let pointer = this.head;
			for (let i = 0; i < index; i++) {
				pointer = pointer.next;
			}

			return pointer.value;
		}
        
		// 在头部添加等于把整个链表接在一个新节点后面
		addAtHead(val: number): void {
			const node = new Node(val);
			node.next = this.head;
			this.head = node;
			this.length++;
		}
        
		// 主要在于判断链表有无元素
		addAtTail(val: number): void {
			const node = new Node(val);
			if (!this.head) {
				this.head = node;
			} else {
				let pointer = this.head;
				while (pointer.next) {
					pointer = pointer.next;
				}
				pointer.next = node;
			}
			this.length++;
		}

		// index 有几种情况
		//  1. index 超出链表的长度（异常情况）
		//  2. index <= 0，即在头部添加
		//  3. index 为链表的长度，即在尾部添加
		//  4. index 为中间位置，即在中间添加
		addAtIndex(index: number, val: number): void {
			if (index > this.length) return;
			if (index <= 0) {
				this.addAtHead(val);
				return;
			}
			if (index === this.length) {
				this.addAtTail(val);
				return;
			}

			const node = new Node(val);
			let pointer = this.head;
			for (let i = 0; i < index - 1; i++) {
				pointer = pointer.next;
			}
			node.next = pointer.next;
			pointer.next = node;
			this.length++;
		}
			
		// index 有几种情况，但是和添加不一样
		//  1. index < 0 || >= length 不做任何处理
		//  2. index === 0，即删除头部
		//  3. index 为中间位置，即删除中间位置
		deleteAtIndex(index: number): void {
			if (index < 0 || index >= this.length) return;

			if (index === 0) {
				this.head = this.head.next;
				this.length--;
				return;
			}

			let pointer = this.head;
			for (let i = 0; i < index - 1; i++) {
				pointer = pointer.next;
			}
			pointer.next = pointer.next.next;
			this.length--;
		}
	}
```

##### 双链表

双链表和单链表的区别就是需要考虑两个指针的指向问题，需要思路更清晰一些。

```typescript
	class Node {
		value: number;
		prev: Node | null = null;
		next: Node | null = null;

		constructor(value: number) {
			this.value = value;
		}
	}

	class MyLinkedList {
		head: Node | null = null;
		tail: Node | null = null;
		length: number = 0;

		constructor() {}

		get(index: number): number {
			if (index < 0 || index >= this.length) return -1;

			let pointer = this.head;
			for (let i = 0; i < index; i++) {
				pointer = pointer.next;
			}

			return pointer.value;
		}

		addAtHead(val: number): void {
			const node = new Node(val);
			if (!this.head) {
				this.head = node;
				this.tail = node;
			} else {
				node.next = this.head;
				this.head.prev = node;
				this.head = node;
			}
			this.length++;
		}

		addAtTail(val: number): void {
			const node = new Node(val);
			if (!this.head) {
				this.head = node;
				this.tail = node;
			} else {
				node.prev = this.tail;
				this.tail.next = node;
				this.tail = node;
			}
			this.length++;
		}

		addAtIndex(index: number, val: number): void {
			if (index > this.length) return;
			if (index <= 0) {
				this.addAtHead(val);
				return;
			}
			if (index === this.length) {
				this.addAtTail(val);
				return;
			}

			const node = new Node(val);
			let pointer = this.head;
			for (let i = 0; i < index - 1; i++) {
				pointer = pointer.next;
			}
			node.prev = pointer;
			node.next = pointer.next;
			pointer.next.prev = node;
			pointer.next = node;
			this.length++;
		}

		deleteAtIndex(index: number): void {
			if (index < 0 || index >= this.length) return;

			if (index === 0) {
				this.head = this.head.next;
                
				if (this.head) {
					this.head.prev = null;
				} else {
					this.tail = null;
				}

				this.length--;
				return;
			}

			if (index === this.length - 1) {
				this.tail = this.tail.prev;
				this.tail.next = null;
				this.length--;
				return;
			}

			let pointer = this.head;
			for (let i = 0; i < index - 1; i++) {
				pointer = pointer.next;
			}
			pointer.next = pointer.next.next;
			pointer.next.prev = pointer;
			this.length--;
		}
	}
```

### 206.反转链表 

> 题目描述：
> 
> 给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

#### 思路

1. 新建一个链表，然后遍历原链表，每遍历一个节点，就把该节点插入到新链表的头部
2. 遍历完成之后，新链表就是反转之后的链表
3. 由于新链表的头部是最后一个节点，所以需要新建一个虚拟头部，然后把新链表的头部指向虚拟头部的下一个节点
4. 由于新链表的尾部是原链表的头部，所以需要把原链表的头部指向 null
5. 返回新链表的头部

#### 代码实现

```typescript
function reverseList(head: ListNode | null): ListNode | null {
  if (!head) return null;

  let newList = new ListNode(0, null);

  newList.next = head;

  while(head.next) {
    let temp = head.next;
    head.next = temp.next;
    temp.next = newList.next;
    newList.next = temp;
  }

  return newList.next;
};
```
