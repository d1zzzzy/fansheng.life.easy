---
title: 代码随想录算法训练营第二十天| 最大二叉树、合并二叉树、二叉搜索树中的搜索、验证二叉搜索树
date: 2023-12-18 16:45:36
tags:
  - Algorithm
  - BinaryTree
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_02.png
---

# 代码随想录算法训练营第二十天

## 最大二叉树

### 题目描述

> 给定一个不含重复元素的整数数组 nums 。一个以此数组直接递归构建的 最大二叉树 定义如下：
>
> 1. 二叉树的根是数组 nums 中的最大元素。
> 2. 左子树是通过数组中 最大值左边部分 递归构造出的最大二叉树。
> 3. 右子树是通过数组中 最大值右边部分 递归构造出的最大二叉树。
> 4. 返回有给定数组 nums 构建的 最大二叉树 。

### 思路

通常，最大二叉树是指具有以下特性的二叉树：

1. **每个节点的值大于其子节点的值**：在最大二叉树中，每个节点的值都是其左右子树中所有节点值的最大值。

2. **构建方式**：最大二叉树可以通过特定的方式构建，例如，给定一个没有重复元素的数组，最大二叉树可以通过以下步骤构建：
	+ 数组中的最大值成为根节点。
	+ 根节点左侧的数组元素构成左子树。
	+ 根节点右侧的数组元素构成右子树。
	+ 递归地对左右子数组应用上述步骤。

```typescript
function constructMaximumBinaryTree(nums: number[]): TreeNode | null {
  if (nums.length === 0) return null;

  const maxIndex = nums.indexOf(Math.max(...nums));
  const root: TreeNode = {
    value: nums[maxIndex],
    left: constructMaximumBinaryTree(nums.slice(0, maxIndex)),
    right: constructMaximumBinaryTree(nums.slice(maxIndex + 1)),
  };

  return root;
}
```

## 合并二叉树

### 题目描述

> 给你两棵二叉树的根节点 root1 和 root2 。请你将 root2 合并到 root1 中间。

### 思路

+ 如果 t1 和 t2 都是 null，函数返回 null。
+ 计算 t1 和 t2 当前节点的值（如果节点不存在，则值为 0），并创建一个新的节点，其值为 t1 和 t2 节点值之和。
+ 递归地合并 t1 和 t2 的左子树和右子树。

```typescript
function mergeTrees(t1: TreeNode | null, t2: TreeNode | null): TreeNode | null {
  if (!t1 && !t2) {
    return null;
  }

  const val1 = t1 ? t1.val : 0;
  const val2 = t2 ? t2.val : 0;

  const mergedNode = { val: val1 + val2, left: null, right: null };

  mergedNode.left = mergeTrees(t1 ? t1.left : null, t2 ? t2.left : null);
  mergedNode.right = mergeTrees(t1 ? t1.right : null, t2 ? t2.right : null);

  return mergedNode;
}
```

## 二叉搜索树中的搜索

### 题目描述

> 给定二叉搜索树（BST）的根节点和一个值。你需要在BST中找到节点值等于给定值的节点。返回以该节点为根的子树。如果节点不存在，则返回 NULL。

### 思路

1. 如果 root 为 null，说明树为空或已经搜索到叶子节点，返回 null。
2. 如果 root.val 等于 val，说明找到了目标节点，返回该节点。
3. 如果 val 小于 root.val，继续在左子树中搜索。
4. 如果 val 大于 root.val，继续在右子树中搜索。

```typescript
function searchBST(root: TreeNode | null, val: number): TreeNode | null {
  if (!root) {
    return null;
  }
  if (root.val === val) {
    return root;
  }
  if (val < root.val) {
    return searchBST(root.left, val);
  } else {
    return searchBST(root.right, val);
  }
}
```

## 验证二叉搜索树

### 题目描述

> 给定一个二叉树，判断其是否是一个有效的二叉搜索树。

### 思路

+ 如果 `root` 为 `null`，说明树为空，或者已经递归到叶子节点，返回 `true`。
+ 检查 `root.val` 是否大于 `min.val`（如果 `min` 不为空）和是否小于 max.val（如果 max 不为空）。如果不满足这些条件，则树不是二叉搜索树。
+ 递归地对左子树调用 isValidBST，将当前节点作为 max，对右子树调用 isValidBST，将当前节点作为 min。

```typescript
function isValidBST(root: TreeNode | null, min: TreeNode | null = null, max: TreeNode | null = null): boolean {
  if (!root) {
    return true;
  }

  if (min && root.val <= min.val) {
    return false;
  }

  if (max && root.val >= max.val) {
    return false;
  }

  return isValidBST(root.left, min, root) && isValidBST(root.right, root, max);
}
```
