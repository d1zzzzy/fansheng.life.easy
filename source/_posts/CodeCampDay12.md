---
title: 代码随想录算法训练营第十三天| 层序遍历十题、翻转二叉树、对称二叉树2
date: 2023-12-13 16:47:33
tags:
  - Algorithm
  - BinaryTree
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_05.png
---

# 代码随想录算法训练营第十三天

## 层序遍历十题

### 二叉树的层序遍历

#### 题目描述

> 给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。

#### 思路

1. 创建一个队列，用于存储待访问的节点。
2. 将根节点加入队列。
3. 当队列不为空时： 
  + 从队列中弹出一个节点。 
  + 访问该节点。 
  + 将该节点的非空左右子节点依次加入队列。

#### 代码实现

```typescript
function levelOrder(root: TreeNode | null): number[][] {
  if (!root) return [];

  let result: number[][] = [];
  let queue: TreeNode[] = [root];

  while (queue.length > 0) {
    let levelSize = queue.length;
    let currentLevel: number[] = [];

    for (let i = 0; i < levelSize; i++) {
      let node = queue.shift()!;
      currentLevel.push(node.val);

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }

    result.push(currentLevel);
  }

  return result;
}
```

### 二叉树的层序遍历 II

#### 题目描述

> 给定一个二叉树，返回其节点值自底向上的层序遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

#### 思路

1. 使用一个队列来按层存储树中的节点。
2. 对树进行标准的层序遍历，记录每一层的节点值。
3. 完成遍历后，将存储层次的数组反转，以得到从底层到顶层的顺序。

#### 代码实现

```typescript
function levelOrderBottom(root: TreeNode | null): number[][] {
  if (!root) return [];

  let result: number[][] = [];
  let queue: TreeNode[] = [root];

  while (queue.length > 0) {
    let levelSize = queue.length;
    let currentLevel: number[] = [];

    for (let i = 0; i < levelSize; i++) {
      let node = queue.shift()!;
      currentLevel.push(node.val);

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }

    result.unshift(currentLevel); // 添加到结果数组的前端
  }

  return result;
}
```

### 二叉树的右视图

#### 题目描述

> 给定一个二叉树的 **根节点** `root` ，想象自己站在它的 **右侧** ，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

#### 思路

1. 如果根节点为空，则返回空列表。 
2. 使用队列进行层序遍历。 
3. 对于每一层，将最后一个节点（即最右边的节点）添加到结果列表中。

#### 代码实现

```typescript
function rightSideView(root: TreeNode | null): number[] {
  if (!root) return [];

  let result: number[] = [];
  let queue: TreeNode[] = [root];

  while (queue.length > 0) {
    let levelSize = queue.length;

    for (let i = 0; i < levelSize; i++) {
      let node = queue.shift()!;
      if (i === levelSize - 1) {
        result.push(node.val); // 添加每层的最后一个节点
      }

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
  }

  return result;
}
```

### 二叉树的层平均值

#### 题目描述

> 给定一个非空二叉树, 返回一个由每层节点平均值组成的数组。

#### 思路

1. 如果根节点为空，则返回空列表。
2. 使用队列进行层序遍历。
3. 对于每一层，计算所有节点值的总和及节点数量，然后求平均值。

#### 代码实现

```typescript
function averageOfLevels(root: TreeNode | null): number[] {
  if (!root) return [];

  let averages: number[] = [];
  let queue: TreeNode[] = [root];

  while (queue.length > 0) {
    let levelLength = queue.length;
    let sum = 0;

    for (let i = 0; i < levelLength; i++) {
      let node = queue.shift()!;
      sum += node.val;

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }

    averages.push(sum / levelLength);
  }

  return averages;
}
```

### N 叉树的层序遍历

#### 题目描述

> 给定一个 N 叉树，返回其节点值的 **层序遍历** 。 (即从左到右，逐层遍历)。

#### 思路

1. 如果根节点为空，则返回空列表。
2. 初始化一个队列，将根节点加入队列。
3. 当队列不为空时： 
   + 记录当前队列的长度，这个长度代表当前层的节点数。
   + 依次从队列中取出这一层的所有节点，并将它们的子节点加入队列。
   + 将这一层的节点值存储在一个列表中。

#### 代码实现

```typescript
function levelOrder(root: Node | null): number[][] {
  if (!root) return [];

  let result: number[][] = [];
  let queue: Node[] = [root];

  while (queue.length > 0) {
    let levelLength = queue.length;
    let currentLevel: number[] = [];

    for (let i = 0; i < levelLength; i++) {
      let node = queue.shift()!;
      currentLevel.push(node.val);

      // 将当前节点的所有子节点加入队列
      for (let child of node.children) {
        if (child) queue.push(child);
      }
    }

    result.push(currentLevel);
  }

  return result;
}
```

### 填充每个节点的下一个右侧节点指针

#### 题目描述

> 给定一个 **完美二叉树** ，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

#### 思路

1. 使用队列进行层序遍历。
2. 对于每一层的节点，将每个节点的 next 指针指向队列中的下一个节点，最后一个节点的 next 指向 null

#### 代码实现

```typescript
function connect(root: Node | null): Node | null {
  if (!root) return null;

  let queue: Node[] = [root];

  while (queue.length > 0) {
    let levelLength = queue.length;

    for (let i = 0; i < levelLength; i++) {
      let node = queue.shift()!;
      // 不是每层的最后一个节点
      if (i < levelLength - 1) {
        node.next = queue[0];
      }

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
  }

  return root;
}
```

### 填充每个节点的下一个右侧节点指针II

#### 题目描述

> 给定一个二叉树：
>
> ```c++
> struct Node {
>   int val;
>   Node *left;
>   Node *right;
>   Node *next;
> }
> ```
> 
> 填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL 。

#### 思路

1. 使用队列进行层序遍历。
2. 对于每一层的节点，将每个节点的 next 指针指向队列中的下一个节点，最后一个节点的 next 指向 null。

```typescript
function connect(root: Node | null): Node | null {
  if (!root) return null;

  let queue: Node[] = [root];

  while (queue.length > 0) {
    let levelLength = queue.length;

    for (let i = 0; i < levelLength; i++) {
      let node = queue.shift()!;
      // 不是每层的最后一个节点
      if (i < levelLength - 1) {
        node.next = queue[0];
      }

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }
  }

  return root;
}
```

### 在每个树行中找最大值

#### 思路

1. 使用队列进行层序遍历。
2. 对于每一层的节点，将每个节点的值与当前层的最大值进行比较，如果大于当前层的最大值，则更新最大值。
3. 将当前层的最大值添加到结果列表中。
4. 返回结果列表。

#### 代码实现

```typescript
function largestValues(root: TreeNode | null): number[] {
  const queue: TreeNode[] = [];

  if(root) queue.push(root);

  const result: number[] = [];

  while(queue.length) {
    const size = queue.length;
    let max: number = -Number.MAX_SAFE_INTEGER;

    for (let i = 0; i < size; i++) {
      const node = queue.shift();

      max = Math.max(max, node.val);

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }

    result.push(max);
  }

  return result;
};
```

### 二叉树的最大深度

#### 思路

一个节点的最大深度是它的左右子节点的最大深度加一。

1. 如果节点为空，返回深度 0。
2. 计算左子树的深度。
3. 计算右子树的深度。
4. 返回左右子树深度的最大值加一。

#### 代码实现

```typescript
function maxDepth(root: TreeNode | null): number {
  if (root === null) {
      return 0;
  }

  const leftDepth = maxDepth(root.left);
  const rightDepth = maxDepth(root.right);

  return Math.max(leftDepth, rightDepth) + 1;
}
```

### 二叉树的最小深度

#### 题目描述

> 最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

#### 思路

##### 递归方法

1. 如果节点为空，返回 0。
2. 如果左子树为空，返回右子树的最小深度加 1。
3. 如果右子树为空，返回左子树的最小深度加 1。
4. 如果左右子树都不为空，返回左右子树的最小深度的最小值加 1。

#### 代码实现

```typescript
function minDepth(root: TreeNode | null): number {
  if (root === null) {
    return 0;
  }
  if (root.left === null) {
    return minDepth(root.right) + 1;
  }
  if (root.right === null) {
    return minDepth(root.left) + 1;
  }
  return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
}
```

## 翻转二叉树

### 题目描述

> 翻转一棵二叉树。

### 思路

递归地交换每个节点的左右子树。对于每个节点，我们先交换其左右子节点，然后分别递归地对左子树和右子树执行同样的操作。

### 代码实现

```typescript
function invertTree(root: TreeNode | null): TreeNode | null {
  if (root === null) {
    return null;
  }

  // 交换左右子树
  [root.left, root.right] = [root.right, root.left];

  // 递归地翻转子树
  invertTree(root.left);
  invertTree(root.right);

  return root;
}
```

## 对称二叉树 2

### 题目描述

> 给定一个二叉树，检查它是否是镜像对称的。

### 思路

**递归方法**

递归方法的基本思想是比较树的左子树和右子树。对于树来说，要成为对称的，它必须满足以下条件：

1. 它的左子树和右子树的根节点具有相同的值。
2. 左子树的左子树和右子树的右子树是对称的。
3. 左子树的右子树和右子树的左子树是对称的。

### 代码实现

```typescript
function isSymmetric(root: TreeNode | null): boolean {
  if (root === null) return true;
  return isMirror(root.left, root.right);
}

function isMirror(t1: TreeNode | null, t2: TreeNode | null): boolean {
  if (t1 === null && t2 === null) return true;
  if (t1 === null || t2 === null) return false;
  return (t1.val === t2.val) && isMirror(t1.right, t2.left) && isMirror(t1.left, t2.right);
}
```

## 关于二叉树

二叉树遍历是遍历树结构的一种方法，涉及到访问树中的每个节点一次且仅一次。根据节点访问的顺序不同，二叉树遍历主要有以下几种类型：

### 1. 前序遍历（Pre-order Traversal）

前序遍历的顺序是先访问根节点，然后递归地进行左子树的前序遍历，最后递归地进行右子树的前序遍历。

**思路及解法**：
- 递归法：直接按照定义进行递归。
- 迭代法：使用栈来模拟递归过程。

### 2. 中序遍历（In-order Traversal）

中序遍历的顺序是先递归地进行左子树的中序遍历，然后访问根节点，最后递归地进行右子树的中序遍历。

**思路及解法**：
- 递归法：按照定义递归左子树，访问节点，递归右子树。
- 迭代法：使用栈来遍历左子树，访问节点，再遍历右子树。

### 3. 后序遍历（Post-order Traversal）

后序遍历的顺序是先递归地进行左子树的后序遍历，然后递归地进行右子树的后序遍历，最后访问根节点。

**思路及解法**：
- 递归法：按照定义递归左子树和右子树，然后访问节点。
- 迭代法：使用栈来实现，注意访问顺序。

### 4. 层序遍历（Level-order Traversal）

层序遍历是按照树的层级从上到下访问每个节点。

**思路及解法**：
- 使用队列：将根节点放入队列，然后不断迭代，每次从队列中取出一个节点访问，并将其子节点按顺序放入队列。

### 总结

- 递归法更直观，直接按照遍历的定义进行实现。
- 迭代法通常需要使用栈或队列作为辅助数据结构。
- 各种遍历的选择取决于具体需要。例如，中序遍历常用于二叉搜索树，可以得到有序的数据；层序遍历则用于分层处理节点。

在实际应用中，选择合适的遍历方式对于解决问题是关键。每种遍历方式都有其特定的应用场景，理解这些遍历方式及其实现对于深入理解树结构和相关算法非常有帮助。
