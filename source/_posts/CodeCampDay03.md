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
