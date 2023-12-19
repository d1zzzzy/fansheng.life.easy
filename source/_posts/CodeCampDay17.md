---
title: 代码随想录算法训练营第二十一天| 二叉搜索树的最小绝对差、二叉搜索树中的众数、二叉树的最近公共祖先
date: 2023-12-19 18:06:26
tags:
  - Algorithm
  - BinaryTree
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第二十一天

## 二叉搜索树的最小绝对差

### 题目描述

> 给你一棵所有节点为非负值的二叉搜索树，请你计算树中任意两节点的差的绝对值的最小值。

### 思路

1. 使用中序遍历递归地遍历树。
2. 在遍历过程中，比较当前节点和前一个节点的值，计算绝对差。
3. 更新最小绝对差。

```typescript
function getMinimumDifference(root: TreeNode | null): number {
  let prev: TreeNode | null = null;
  let minDiff = Infinity;

  function inorder(node: TreeNode | null) {
    if (node === null) return;

    inorder(node.left);

    if (prev) {
      minDiff = Math.min(minDiff, Math.abs(node.val - prev.val));
    }
    prev = node;

    inorder(node.right);
  }

  inorder(root);
  return minDiff;
}
```

## 二叉搜索树中的众数

### 题目描述

> 给定一个有相同值的二叉搜索树（BST），找出 BST 中的所有众数（出现频率最高的元素）。

### 思路

1. **初始化**：创建变量来存储当前元素的值、当前元素的计数、最大计数和众数列表。
2. **中序遍历**：递归地遍历树，按升序处理节点。
3. **更新计数和众数**：如果当前节点的值等于前一个节点的值，增加计数；否则，重置计数。根据计数更新众数列表。

```typescript
function findMode(root: TreeNode | null): number[] {
  let currentVal: number | undefined;
  let currentCount = 0;
  let maxCount = 0;
  let modes: number[] = [];

  function updateModes(val: number) {
    if (val === currentVal) {
      currentCount++;
    } else {
      currentVal = val;
      currentCount = 1;
    }

    if (currentCount > maxCount) {
      maxCount = currentCount;
      modes = [val];
    } else if (currentCount === maxCount) {
      modes.push(val);
    }
  }

  function inorder(node: TreeNode | null) {
    if (node === null) return;

    inorder(node.left);
    updateModes(node.val);
    inorder(node.right);
  }

  inorder(root);
  return modes;
}
```

## 二叉树的最近公共祖先

### 题目描述

> 给定一个二叉树，找到该树中两个指定节点的最近公共祖先。

### 思路

+ 递归遍历：从根节点开始，递归地遍历二叉树。
+ 递归终止条件：如果当前节点为空、等于 `p` 或等于 `q`，则返回当前节点。
+ 递归左右子树：分别在左右子树中递归寻找 `p` 和 `q`。
+ 判断和返回： 
  1. 如果左右子树递归都返回非空值，则说明 `p` 和 `q` 分别在当前节点的两侧，当前节点即为最近公共祖先。 
  2. 如果只有一个子树递归返回非空值，则说明 `p` 和 `q` 都在这个子树上，返回这个子树的递归结果。 
  3. 如果两个子树递归都返回空值，则返回空。

```typescript
function lowestCommonAncestor(root: TreeNode | null, p: TreeNode, q: TreeNode): TreeNode | null {
  if (root === null || root === p || root === q) {
    return root;
  }

  let left = lowestCommonAncestor(root.left, p, q);
  let right = lowestCommonAncestor(root.right, p, q);

  if (left !== null && right !== null) {
    return root;
  }

  return left !== null ? left : right;
}
```

## 二叉搜索树总结

具有以下独特的性质和特性：

1. **节点值的有序性**：
	- 每个节点的值都大于其左子树上所有节点的值。
	- 每个节点的值都小于其右子树上所有节点的值。
	- 这意味着对二叉搜索树进行中序遍历将产生一个升序的节点值序列。

2. **查找效率**：
	- 由于其有序性，二叉搜索树在查找特定值时可以提供平均为 O(log n) 的时间复杂度，其中 n 是树中节点的数量。
	- 在最坏情况下（树呈线性结构时），查找操作的时间复杂度会退化到 O(n)。

3. **插入和删除操作**：
	- 插入操作涉及搜索正确的插入位置，以保持树的有序性，然后添加新节点。
	- 删除操作较为复杂，分为几种情况：删除的节点是叶节点、只有一个子节点或有两个子节点。在删除有两个子节点的节点时，通常需要找到其前驱或后继节点来替代被删除的节点。

4. **平衡性**：
	- 二叉搜索树不一定是平衡的。如果插入或删除操作使得树变得不平衡，查找效率可能下降。
	- 平衡二叉搜索树（如 AVL 树或红黑树）通过在每次插入和删除操作后重新平衡树来解决这个问题，从而保证了最坏情况下仍能提供较好的性能。

5. **递归结构**：
	- 二叉搜索树的每个子树本身也是二叉搜索树。
	- 这一性质常被用于递归算法中，例如在查找、插入和删除操作中。

6. **应用场景**：
	- 由于其有序性和高效的查找、插入和删除操作，二叉搜索树在许多场合都非常有用，特别是在需要频繁执行这些操作的场景中，如数据库索引和文件系统。
