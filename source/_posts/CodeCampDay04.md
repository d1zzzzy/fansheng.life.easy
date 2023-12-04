---
title: 代码随想录算法训练营第亖天| 两两交换链表中的节点、删除链表的倒数第N个节点、链表相交、环形链表II
date: 2023-12-04 09:28:48
tags:
  - Algorithm
  - LinkedList
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/code-camp-day04.png
---

# 代码随想录算法训练营第四天

## 两两交换链表中的节点

> 题目描述：
> 
> 给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

### 思路

#### 解法一：递归

递归的思路很简单，就是将链表分成两部分，一部分是需要交换的节点，另一部分是剩余的节点。

```typescript
function swapPairs(head: ListNode | null): ListNode | null {
	// 如果当前节点为空，或者当前节点的下一个节点为空，直接返回当前节点
	if (!head || !head.next) {
		return head;
	}

	// 保存下一个节点
	const next = head.next;

	// 递归调用，将剩余的节点进行两两交换
	head.next = swapPairs(next.next);

	// 交换节点
	next.next = head;

	// 返回交换后的节点
	return next;
}
```

#### 解法二：迭代

迭代的思路也很简单，就是将链表分成两部分，一部分是需要交换的节点，另一部分是剩余的节点。

```typescript
function swapPairs(head: ListNode | null): ListNode | null {
	// 新建一个虚拟指针，将该指针指向 head
	// 如此即便 head 为空，我们也可以通过 虚拟指针进行遍历
	const dummyHeader = new ListNode(0, head);

	// 用于遍历指针，初始值指向 头指针
	let pointer = dummyHeader;

	// 循环终止条件
	while (pointer.next && pointer.next.next) {
		// 保存下一个节点
		const next = pointer.next.next;

		// 交换节点
		pointer.next.next = next.next;
		next.next = pointer.next;
		pointer.next = next;

		// 移动指针
		pointer = pointer.next.next;
	}

	return dummyHeader.next;
}
```

#### 复杂度分析

+ 递归
	+ 时间复杂度：O(n)
	+ 空间复杂度：O(n)
+ 迭代
  + 时间复杂度：O(n)
  + 空间复杂度：O(1)

## 删除链表的倒数第N个节点

> 题目描述：
> 
> 给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

### 思路

#### 解法一：双指针

双指针的思路很简单，就是让快指针先走 `n` 步，然后快慢指针一起走，当快指针走到链表末尾的时候，慢指针指向的就是倒数第 `n` 个节点。

```typescript
function removeNthFromEnd(head: ListNode | null, n: number): ListNode | null {
	// 新建一个虚拟指针，将该指针指向 head
	// 如此即便 head 为空，我们也可以通过 虚拟指针进行遍历
	const dummyHeader = new ListNode(0, head);

	// 用于遍历指针，初始值指向 头指针
	let pointer = dummyHeader;

	// 快指针
	let fast = dummyHeader;

	// 快指针先走 n 步
	while (n--) {
		fast = fast.next;
	}

	// 快慢指针一起走
	while (fast.next) {
		pointer = pointer.next;
		fast = fast.next;
	}

	// 删除节点
	pointer.next = pointer.next.next;

	return dummyHeader.next;
}
```

#### 解法二：栈

栈的思路也很简单，就是将链表的所有节点入栈，然后出栈 `n` 个节点，此时栈顶的节点就是倒数第 `n` 个节点，然后将栈顶的节点删除即可。

```typescript
function removeNthFromEnd(head: ListNode | null, n: number): ListNode | null {
	// 新建一个虚拟指针，将该指针指向 head
	// 如此即便 head 为空，我们也可以通过 虚拟指针进行遍历
	const dummyHeader = new ListNode(0, head);

	// 用于遍历指针，初始值指向 头指针
	let pointer = dummyHeader;

	// 栈
	const stack = [];

	// 遍历链表，将所有节点入栈
	while (pointer) {
		stack.push(pointer);
		pointer = pointer.next;
	}

	// 出栈 n 个节点
	while (n--) {
		stack.pop();
	}

	// 获取栈顶的节点
	const top = stack[stack.length - 1];

	// 删除节点
	top.next = top.next.next;

	return dummyHeader.next;
}
```

#### 复杂度分析

+ 双指针
	+ 时间复杂度：O(n)
	+ 空间复杂度：O(1)
+ 栈
	+ 时间复杂度：O(n)
	+ 空间复杂度：O(n)

## 链表相交

> 题目描述：
> 
> 给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 `null` 。

### 思路

#### 解法一：双指针

双指针的思路很简单，就是让两个指针分别指向两个链表的头节点，然后让两个指针同时向后移动，当其中一个指针移动到链表末尾的时候，将其指向另一个链表的头节点，然后继续向后移动，当两个指针相遇的时候，就是相交的节点。

```typescript
function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
	// 用于遍历指针，初始值指向 头指针
	let pointerA = headA;
	let pointerB = headB;

	// 循环终止条件
	while (pointerA !== pointerB) {
		// 如果指针 A 指向了链表 A 的末尾，则将其指向链表 B 的头节点
		pointerA = pointerA ? pointerA.next : headB;

		// 如果指针 B 指向了链表 B 的末尾，则将其指向链表 A 的头节点
		pointerB = pointerB ? pointerB.next : headA;
	}

	return pointerA;
}
```

#### 解法二：栈

栈的思路也很简单，就是将两个链表的所有节点分别入栈，然后同时出栈，当两个栈顶的节点不相等的时候，上一个栈顶的节点就是相交的节点。

```typescript
function getIntersectionNode(headA: ListNode | null, headB: ListNode | null): ListNode | null {
  let stackA: ListNode[] = [];
  let stackB: ListNode[] = [];

  // 填充栈A
  let currentA = headA;
  while (currentA != null) {
    stackA.push(currentA);
    currentA = currentA.next;
  }

  // 填充栈B
  let currentB = headB;
  while (currentB != null) {
    stackB.push(currentB);
    currentB = currentB.next;
  }

  let intersection: ListNode | null = null;

  // 查找相交节点
  while (stackA.length > 0 && stackB.length > 0) {
    let topA = stackA.pop();
    let topB = stackB.pop();

    if (topA === topB) {
      intersection = topA;
    } else {
      break;
    }
  }

  return intersection;
}
```

#### 复杂度分析

+ 双指针
	+ 时间复杂度：O(m + n)
	+ 空间复杂度：O(1)
+ 栈
	+ 时间复杂度：O(m + n)
	+ 空间复杂度：O(m + n)


## 环形链表II

> 题目描述：
> 
> 给定一个链表，返回链表开始入环的第一个节点。如果链表无环，则返回 `null` 。

### 思路

#### 解法一：哈希表

哈希表的思路很简单，就是遍历链表，将每个节点存入哈希表，如果遍历到的节点已经存在于哈希表中，则该节点就是环形链表的入口节点。

```typescript
function detectCycle(head: ListNode | null): ListNode | null {
	const set = new Set();

	while (head) {
		if (set.has(head)) {
			return head;
		}

		set.add(head);
		head = head.next;
	}

	return null;
}
```

#### 解法二：双指针

双指针的思路很简单，就是让两个指针分别指向链表的头节点，然后让两个指针同时向后移动，当两个指针相遇的时候，就是环形链表的入口节点。

```typescript
function detectCycle(head: ListNode | null): ListNode | null {
	// 用于遍历指针，初始值指向 头指针
	let pointerA = head;
	let pointerB = head;

	// 循环终止条件
	while (pointerA && pointerB) {
		// 如果指针 A 指向了链表 A 的末尾，则将其指向链表 B 的头节点
		pointerA = pointerA.next;

		// 如果指针 B 指向了链表 B 的末尾，则将其指向链表 A 的头节点
		pointerB = pointerB.next?.next;

		// 如果两个指针相遇，则说明链表有环
		if (pointerA === pointerB) {
			// 将其中一个指针指向头节点
			pointerA = head;

			// 两个指针同时向后移动，当两个指针相遇的时候，就是环形链表的入口节点
			while (pointerA !== pointerB) {
				pointerA = pointerA.next;
				pointerB = pointerB.next;
			}

			return pointerA;
		}
	}

	return null;
}
```

#### 复杂度分析

+ 哈希表
	+ 时间复杂度：O(n)
	+ 空间复杂度：O(n)
+ 双指针
	+ 时间复杂度：O(n)
	+ 空间复杂度：O(1)

## 链表问题总结

### 反转链表：

+ 使用迭代或递归方法来反转链表。
+ 在迭代方法中，使用三个指针：当前节点、上一个节点和下一个节点。

### 检测链表中的环：

+ 使用快慢指针（双指针法）。快指针每次移动两步，慢指针每次移动一步。如果快慢指针相遇，则链表中存在环。

### 链表的中间节点：

+ 使用快慢指针。快指针每次移动两步，慢指针每次移动一步。当快指针到达链表末尾时，慢指针所在的位置即为中间节点。

### 删除链表中的节点：

+ 需要处理两种情况：删除中间的节点和删除尾节点。对于中间节点，可以直接将其前一个节点的 next 指向要删除节点的 next 节点。删除尾节点需要遍历整个链表。

### 链表中倒数第k个节点：

+ 使用双指针法。首先，将一个指针向前移动 k 步，然后同时移动两个指针，直到前面的指针到达链表末尾。

### 合并两个有序链表：

+ 使用递归或迭代方法合并两个有序链表。比较两个链表的头节点，选择较小的节点作为合并后链表的头节点。

### 链表相交问题：

+ 使用哈希表记录其中一个链表的节点，然后遍历另一个链表检查节点是否出现在哈希表中。或者使用双指针法，分别遍历两个链表，通过调整起始位置来找到交点。

### 环形链表的入环节点：

+ 首先使用快慢指针法确定链表中存在环。然后，将一个指针重新定位到链表头部，并以相同速度移动两个指针，直至它们相遇，相遇点即为入环点。

### 奇偶链表：

+ 创建两个指针分别表示奇数节点和偶数节点，然后根据奇偶性分别连接节点，最后将偶数链表连接在奇数链表的尾部。

### 复制带随机指针的链表：

+ 为原始链表的每个节点创建一个新节点，将新节点插入到原节点和下一个原节点之间。然后，为复制的节点分配随机指针，最后分离原链表和复制的链表。
