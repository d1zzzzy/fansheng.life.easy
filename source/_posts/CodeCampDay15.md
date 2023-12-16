---
title: 代码随想录算法训练营第十六天| 找树左下角的值、路径总和、路径总和ii、从中序与后序遍历序列构造二叉树、从前序与中序遍历序列构造二叉树
date: 2023-12-16 15:26:11
tags:
  - Algorithm
  - BinaryTree
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_01.jpg
---

# 代码随想录算法训练营第十六天

## 找树左下角的值

> 题目描述：
> 
> 给定一个二叉树，在树的最后一行找到最左边的值。

### 思路

找到树的左下角的值通常意味着找到二叉树最底层最左边的节点的值。这可以通过层序遍历来实现，层序遍历能够保证遍历顺序从上到下，从左到右。因此，最后一个遍历到的节点即为最底层最左边的节点。

```typescript
function findBottomLeftValue(root: TreeNode | null): number {
	if (!root) return 0;

	let queue: TreeNode[] = [root];
	let bottomLeftValue = 0;

	while (queue.length > 0) {
		let levelSize = queue.length;
		for (let i = 0; i < levelSize; i++) {
			let node = queue.shift()!;
			if (i === 0) bottomLeftValue = node.val; // 记录每层的第一个节点的值

			if (node.left) queue.push(node.left);
			if (node.right) queue.push(node.right);
		}
	}

	return bottomLeftValue;
}
```

## 路径总和

> 题目描述：
> 
> 给你二叉树的根节点 `root` 和一个表示目标和的整数 `targetSum` 。判断该树中是否存在 根节点到叶子节点 的路径，这条路径上所有节点值相加等于目标和 `targetSum` 。如果存在，返回 `true` ；否则，返回 `false` 。

### 思路

路径总和问题是判断在二叉树中是否存在一条从根节点到叶节点的路径，其路径上所有节点的值相加等于给定的目标和。这个问题可以通过递归来解决。

递归方法的基本思想是：从根节点开始，递归检查每个节点，更新目标和，直到到达叶节点。如果在某个叶节点处，更新后的目标和等于该叶节点的值，则路径总和等于给定的目标和。

```typescript
function hasPathSum(root: TreeNode | null, sum: number): boolean {
	if (root === null) {
		return false;
	}
	if (root.left === null && root.right === null) {
		return sum === root.val;
	}
	return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
}
```

## 路径总和ii

> 题目描述：
> 
> 给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。
>
> 叶子节点 是指没有子节点的节点。

### 思路

路径总和 II 问题要求找出所有从根节点到叶节点的路径，这些路径上的节点值之和等于给定的目标和。这个问题可以通过递归的方式解决，类似于路径总和问题，但需要记录符合条件的路径。

递归方法的基本思路是：

1. 从根节点开始，递归地遍历每个节点。
2. 维护一个路径列表，记录从根节点到当前节点的路径。
3. 如果当前节点是叶节点，并且路径上的节点值之和等于目标和，则将该路径添加到结果列表中。
4. 递归地检查左右子节点。

```typescript
function pathSum(root: TreeNode | null, sum: number): number[][] {
	let paths: number[][] = [];
	let path: number[] = [];

	function findPaths(node: TreeNode | null, target: number) {
		if (node === null) return;

		path.push(node.val);
		target -= node.val;

		if (node.left === null && node.right === null && target === 0) {
				paths.push([...path]); // 添加符合条件的路径
		} else {
				findPaths(node.left, target);
				findPaths(node.right, target);
		}
		
		path.pop(); // 回溯
	}

	findPaths(root, sum);
	return paths;
}
```

## 从中序与后序遍历序列构造二叉树

> 题目描述：
> 
> 根据一棵树的中序遍历与后序遍历构造二叉树。

### 思路

从中序与后序遍历序列构造二叉树的问题可以通过递归方法来解决。这个问题的关键在于理解中序遍历和后序遍历的特性：

+ 中序遍历：先遍历左子树，然后是根节点，最后是右子树。
+ 后序遍历：先遍历左子树，然后是右子树，最后是根节点。

后序遍历的最后一个元素总是树的根节点。利用这个信息，我们可以在中序遍历序列中找到根节点，从而确定左子树和右子树的范围。然后，递归地对左子树和右子树执行相同的操作。

**算法步骤**

1. 如果后序遍历序列为空，返回 `null`。
2. 后序遍历的最后一个元素为根节点。
3. 在中序遍历序列中找到根节点的位置，将序列分为左子树和右子树。
4. 递归地构建左子树和右子树

```typescript
function buildTree(inorder: number[], postorder: number[]): TreeNode | null {
	if (postorder.length === 0) return null;

	const rootVal = postorder.pop()!;
	const root = new TreeNode(rootVal);

	const rootIndexInInorder = inorder.indexOf(rootVal);
	root.right = buildTree(inorder.slice(rootIndexInInorder + 1), postorder.slice(rootIndexInInorder));
	root.left = buildTree(inorder.slice(0, rootIndexInInorder), postorder.slice(0, rootIndexInInorder));

	return root;
}
```

## 从前序与中序遍历序列构造二叉树

> 题目描述：
> 
> 根据一棵树的前序遍历与中序遍历构造二叉树。

从前序与中序遍历序列构造二叉树的问题可以通过递归方法解决。关键在于理解前序遍历和中序遍历的特性：

+ 前序遍历：先访问根节点，然后是左子树，最后是右子树。
+ 中序遍历：先访问左子树，然后是根节点，最后是右子树。

前序遍历的第一个元素总是树的根节点。利用这个信息，我们可以在中序遍历序列中找到根节点，从而确定左子树和右子树的范围。接着，递归地对左子树和右子树执行相同的操作。

```typescript
function buildTree(preorder: number[], inorder: number[]): TreeNode | null {
	if (preorder.length === 0) return null;

	const rootVal = preorder[0];
	const root = new TreeNode(rootVal);

	const rootIndexInInorder = inorder.indexOf(rootVal);
	root.left = buildTree(preorder.slice(1, rootIndexInInorder + 1), inorder.slice(0, rootIndexInInorder));
	root.right = buildTree(preorder.slice(rootIndexInInorder + 1), inorder.slice(rootIndexInInorder + 1));

	return root;
}
```
