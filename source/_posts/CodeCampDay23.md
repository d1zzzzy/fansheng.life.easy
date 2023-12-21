---
title: 代码随想录算法训练营第二十三天| 修剪二叉搜索树、将有序数组转换为二叉搜索树、把二叉搜索树转换为累加树
date: 2023-12-21 16:56:59
tags:
  - Algorithm
  - BinaryTree
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第二十三天

## 修剪二叉搜索树

### 题目描述

> 给你二叉搜索树的根节点 root ，同时给定最小边界low 和最大边界 high。通过修剪二叉搜索树，使得所有节点的值在[low, high]中。修剪根节点不会导致其余节点的删除。返回修剪好的二叉搜索树的根节点。

1. 递归遍历树：从根节点开始，递归地处理每个节点。
2. 决定是否保留节点： 
   + 如果节点的值小于 low，则其左子树的所有节点也都小于 low，因此整个左子树都被舍弃，递归处理右子树。 
   + 如果节点的值大于 high，则其右子树的所有节点也都大于 high，因此整个右子树都被舍弃，递归处理左子树。 
   + 如果节点的值在 [low, high] 范围内，递归处理其左右子树，并保留当前节点。

```typescript
function trimBST(root: TreeNode | null, low: number, high: number): TreeNode | null {
  if (root === null) return null;

  // 节点值小于下限
  if (root.val < low) return trimBST(root.right, low, high);

  // 节点值大于上限
  if (root.val > high) return trimBST(root.left, low, high);

  // 节点值在范围内，递归处理左右子树
  root.left = trimBST(root.left, low, high);
  root.right = trimBST(root.right, low, high);
  return root;
}
```

## 将有序数组转换为二叉搜索树

### 题目描述

> 给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 高度平衡 二叉搜索树。高度平衡 二叉树 是一棵满足每个节点的左右两个子树的高度差的绝对值不超过 1 的二叉树。

1. 找到中间元素：选择数组的中间元素作为树的根节点。如果数组长度为偶数，可以选择中间两个元素中的任意一个。
2. 递归构建子树：使用中间元素左侧的元素构建左子树，右侧的元素构建右子树。
3. 递归终止条件：当数组为空或子数组长度为 0 时停止递归。

```typescript
function sortedArrayToBST(nums: number[]): TreeNode | null {
  if (nums.length === 0) return null;

  const mid = Math.floor(nums.length / 2);
  const node = new TreeNode(nums[mid]);
  node.left = sortedArrayToBST(nums.slice(0, mid));
  node.right = sortedArrayToBST(nums.slice(mid + 1));

  return node;
}
```

### 注意事项

+ 在选择中间元素作为根节点时，如果数组长度为偶数，可以选择中间两个元素中的任意一个。这可能会导致不同的平衡二叉树，但都是有效的。
+ 使用数组切片 (slice) 在每次递归时创建新的子数组。虽然这种方法简单明了，但可能导致额外的内存使用。如果对性能有更高要求，可以考虑使用数组索引来避免额外的内存分配。
+ 这种方法能保证生成的二叉树是高度平衡的，适用于需要快速搜索、插入和删除操作的场景

### 优化实现

```typescript
function sortedArrayToBST(nums: number[]): TreeNode | null {
  function convertListToBST(left: number, right: number): TreeNode | null {
    if (left > right) return null;

    // 总是选择中间位置右边的数字作为根节点
    let mid = Math.floor((left + right) / 2) + (left + right) % 2;

    let node = new TreeNode(nums[mid]);
    node.left = convertListToBST(left, mid - 1);
    node.right = convertListToBST(mid + 1, right);
    return node;
  }

  return convertListToBST(0, nums.length - 1);
}
```

## 把二叉搜索树转换为累加树

### 题目描述

> 给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

### 思路

1. 反向中序遍历：从根节点开始，按照“右子树 - 根节点 - 左子树”的顺序遍历树。
2. 累加和更新：维护一个累加和变量，用来存储遍历过程中访问过的所有节点的值之和。更新每个节点的值为当前的累加和。
3. 递归实现：使用递归函数进行反向中序遍历，并在访问每个节点时更新其值。

```typescript
function convertBST(root: TreeNode | null): TreeNode | null {
  let sum = 0;

  function reverseInOrder(node: TreeNode | null) {
    if (node === null) return;

    // 先遍历右子树
    reverseInOrder(node.right);

    // 更新节点的值和累加和
    sum += node.val;
    node.val = sum;

    // 遍历左子树
    reverseInOrder(node.left);
  }

  reverseInOrder(root);
  return root;
}
```

#### 注意事项

1. 确保遵循反向中序遍历的顺序，这对于正确地累加和更新节点的值至关重要。
2. 使用递归实现是简单直观的，但要注意处理好递归的边界条件。
3. 对于非常大的树，递归可能导致栈溢出。在这种情况下，可以考虑使用迭代方法和栈来模拟递归。

### 优化实现

1. 初始化根节点：首先，找到数组的中间元素并以此作为树的根节点。这一步是为了确保最终的树是高度平衡的。

2. 使用栈来模拟递归：在递归版本中，我们会递归地处理左右子树。在迭代版本中，我们使用一个栈来模拟这个过程。栈中的每个元素都包含一个待处理的树节点和该节点对应的子数组的左右边界索引。

3. 处理栈中的元素：每次从栈中弹出一个元素，该元素包含一个节点和该节点对应的子数组边界。然后，根据边界找到子数组的中间元素，并为其创建左右子节点。

4. 迭代构建子树：对于每个新创建的子节点，我们将它及其对应的子数组边界作为新元素推入栈中。这样，我们就能继续为这些子节点构建它们的子树。

5. 重复直至栈为空：继续此过程，直到栈中没有其他元素。此时，所有的子数组都已经被处理，二叉搜索树构建完成。


```typescript
class StackNode {
  node: TreeNode;
  start: number;
  end: number;

  constructor(node: TreeNode, start: number, end: number) {
    this.node = node;
    this.start = start;
    this.end = end;
  }
}

function sortedArrayToBST(nums: number[]): TreeNode | null {
  if (nums.length === 0) return null;

  let mid = Math.floor(nums.length / 2);
  let root = new TreeNode(nums[mid]);
  let stack: StackNode[] = [new StackNode(root, 0, nums.length - 1)];

  while (stack.length > 0) {
    let { node, start, end } = stack.pop()!;
    mid = Math.floor((start + end) / 2);

    if (mid > start) {
      let leftMid = Math.floor((start + mid - 1) / 2);
      node.left = new TreeNode(nums[leftMid]);
      stack.push(new StackNode(node.left, start, mid - 1));
    }

    if (mid < end) {
      let rightMid = Math.floor((mid + 1 + end) / 2);
      node.right = new TreeNode(nums[rightMid]);
      stack.push(new StackNode(node.right, mid + 1, end));
    }
  }

  return root;
}
```
