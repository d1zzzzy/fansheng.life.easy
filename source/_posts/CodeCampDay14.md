---
title: 代码随想录算法训练营第十五天| 平衡二叉树、二叉树的所有路径、左子叶之和
date: 2023-12-15 15:02:52
tags:
  - Algorithm
  - BinaryTree
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第十五天

## 平衡二叉树

> 题目描述：
> 
> 给定一个二叉树，判断它是否是高度平衡的二叉树。

### 思路

1. 递归
2. 迭代

#### 递归

```typescript
function isBalanced(root: TreeNode | null): boolean {
	if (!root) return true;

	return (
		isBalanced(root.left) &&
		isBalanced(root.right) &&
		Math.abs(maxDepth(root.left) - maxDepth(root.right)) <= 1
	);
}

function maxDepth(root: TreeNode | null): number {
	if (!root) return 0;

	return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}
```

#### 迭代

```typescript
function isBalanced(root: TreeNode | null): boolean {
	if (!root) return true;

	let queue: TreeNode[] = [root];

	while (queue.length > 0) {
		let levelSize = queue.length;

		for (let i = 0; i < levelSize; i++) {
			let node = queue.shift()!;

			if (node.left) queue.push(node.left);
			if (node.right) queue.push(node.right);
		}

		if (Math.abs(maxDepth(root.left) - maxDepth(root.right)) > 1) return false;
	}

	return true;
}

function maxDepth(root: TreeNode | null): number {
	if (!root) return 0;

	return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
}
```

## 二叉树的所有路径

> 题目描述：
> 
> 给定一个二叉树，返回所有从根节点到叶子节点的路径。

### 思路

1. 递归
2. 迭代

#### 递归

```typescript
function binaryTreePaths(root: TreeNode | null): string[] {
	let result: string[] = [];

	if (!root) return result;

	if (!root.left && !root.right) {
		result.push(`${root.val}`);
		return result;
	}

	let leftPaths = binaryTreePaths(root.left);
	for (let path of leftPaths) {
		result.push(`${root.val}->${path}`);
	}

	let rightPaths = binaryTreePaths(root.right);
	for (let path of rightPaths) {
		result.push(`${root.val}->${path}`);
	}

	return result;
}
```

#### 迭代

```typescript
function binaryTreePaths(root: TreeNode | null): string[] {
	let result: string[] = [];

	if (!root) return result;

	let queue: [TreeNode, string][] = [[root, `${root.val}`]];

	while (queue.length > 0) {
		let [node, path] = queue.shift()!;

		if (!node.left && !node.right) {
			result.push(path);
		}

		if (node.left) queue.push([node.left, `${path}->${node.left.val}`]);
		if (node.right) queue.push([node.right, `${path}->${node.right.val}`]);
	}

	return result;
}
```

## 左子叶之和

> 题目描述：
> 
> 计算给定二叉树的所有左叶子之和。

### 思路

1. 递归
2. 迭代

#### 递归

```typescript
function sumOfLeftLeaves(root: TreeNode | null): number {
	if (!root) return 0;

	let result = 0;

	if (root.left && !root.left.left && !root.left.right) {
		result += root.left.val;
	}

	result += sumOfLeftLeaves(root.left);
	result += sumOfLeftLeaves(root.right);

	return result;
}
```

#### 迭代

```typescript
function sumOfLeftLeaves(root: TreeNode | null): number {
	if (!root) return 0;

	let result = 0;
	let queue: TreeNode[] = [root];

	while (queue.length > 0) {
		let levelSize = queue.length;

		for (let i = 0; i < levelSize; i++) {
			let node = queue.shift()!;

			if (node.left && !node.left.left && !node.left.right) {
				result += node.left.val;
			}

			if (node.left) queue.push(node.left);
			if (node.right) queue.push(node.right);
		}
	}

	return result;
}
```
