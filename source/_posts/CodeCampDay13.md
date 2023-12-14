---
title: 代码随想录算法训练营第十四天| 二叉树的最大深度、 n叉树的最大深度、二叉树的最小深度、完全二叉树的节点个数
date: 2023-12-14 14:43:54
tags:
  - Algorithm
  - BinaryTree
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_02.png
---

# 代码随想录算法训练营第十四天

## 二叉树的最大深度

> 题目描述：
> 
> 给定一个二叉树，找出其最大深度。

### 思路

1. 递归
2. 迭代

#### 递归

```typescript
function maxDepth(root: TreeNode | null): number {
	if (!root) return 0;

	return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}
```

#### 迭代

```typescript
function maxDepth(root: TreeNode | null): number {
	if (!root) return 0;

	let result = 0;
	let queue: TreeNode[] = [root];

	while (queue.length > 0) {
		let levelSize = queue.length;

		for (let i = 0; i < levelSize; i++) {
			let node = queue.shift()!;

			if (node.left) queue.push(node.left);
			if (node.right) queue.push(node.right);
		}

		result++;
	}

	return result;
}
```

## N叉树的最大深度

> 题目描述：
> 
> 给定一个 N 叉树，找到其最大深度。

### 思路

1. 递归
2. 迭代

#### 递归

```typescript
function maxDepth(root: Node | null): number {
	if (!root) return 0;

	let max = 0;

	for (let child of root.children) {
		max = Math.max(max, maxDepth(child));
	}

	return max + 1;
}
```

#### 迭代

```typescript
function maxDepth(root: Node | null): number {
	if (!root) return 0;

	let result = 0;
	let queue: Node[] = [root];

	while (queue.length > 0) {
		let levelSize = queue.length;

		for (let i = 0; i < levelSize; i++) {
			let node = queue.shift()!;

			for (let child of node.children) {
				if (child) queue.push(child);
			}
		}

		result++;
	}

	return result;
}
```

## 二叉树的最小深度

> 题目描述：
> 
> 给定一个二叉树，找出其最小深度。

### 思路

1. 递归
2. 迭代

#### 递归

```typescript
function minDepth(root: TreeNode | null): number {
	if (!root) return 0;

	if (!root.left) return minDepth(root.right) + 1;
	if (!root.right) return minDepth(root.left) + 1;

	return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
}
```

#### 迭代

```typescript
function minDepth(root: TreeNode | null): number {
	if (!root) return 0;

	let result = 0;
	let queue: TreeNode[] = [root];

	while (queue.length > 0) {
		let levelSize = queue.length;

		for (let i = 0; i < levelSize; i++) {
			let node = queue.shift()!;

			if (!node.left && !node.right) return result + 1;

			if (node.left) queue.push(node.left);
			if (node.right) queue.push(node.right);
		}

		result++;
	}

	return result;
}
```

## 完全二叉树的节点个数

> 题目描述：
> 
> 给出一个完全二叉树，求出该树的节点个数。

### 思路

1. 递归
2. 迭代

#### 递归

```typescript
function countNodes(root: TreeNode | null): number {
	if (!root) return 0;

	return countNodes(root.left) + countNodes(root.right) + 1;
}
```

#### 迭代

```typescript
function countNodes(root: TreeNode | null): number {
	if (!root) return 0;

	let result = 0;
	let queue: TreeNode[] = [root];

	while (queue.length > 0) {
		let levelSize = queue.length;

		for (let i = 0; i < levelSize; i++) {
			let node = queue.shift()!;

			if (node.left) queue.push(node.left);
			if (node.right) queue.push(node.right);
		}

		result += levelSize;
	}

	return result;
}
```
