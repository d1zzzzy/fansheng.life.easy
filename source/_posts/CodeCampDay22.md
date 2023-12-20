---
title: 代码随想录算法训练营第二十二天| 二叉搜索树的最近公共祖先、二叉搜索树中的插入操作、删除二叉搜索树中的节点
date: 2023-12-20 11:11:42
tags:
  - Algorithm
  - BinaryTree
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_03.png
---

# 代码随想录算法训练营第二十二天

## 二叉搜索树的最近公共祖先

### 题目描述

> 给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

### 思路

在二叉搜索树（BST）中寻找两个节点的最近公共祖先（LCA）可以利用其特有的性质：左子树上所有节点的值都小于根节点的值，右子树上所有节点的值都大于根节点的值。这使得寻找最近公共祖先变得相对简单，因为可以根据节点值的大小关系来决定遍历的方向

1. 从根节点开始遍历树。
2. 如果两个节点 p 和 q 的值都大于当前节点的值，则说明 p 和 q 应该在当前节点的右子树中，因此向右子树移动。
3. 如果两个节点 p 和 q 的值都小于当前节点的值，则说明 p 和 q 应该在当前节点的左子树中，因此向左子树移动。
4. 如果一个节点的值大于当前节点的值，另一个节点的值小于当前节点的值，或者其中一个节点的值等于当前节点的值，则当前节点就是 p 和 q 的最近公共祖先。

```typescript
function lowestCommonAncestor(root: TreeNode | null, p: TreeNode, q: TreeNode): TreeNode | null {
  while (root !== null) {
    if (p.val > root.val && q.val > root.val) {
      root = root.right; // 向右子树移动
    } else if (p.val < root.val && q.val < root.val) {
      root = root.left; // 向左子树移动
    } else {
      return root; // 找到最近公共祖先
    }
  }
  return null;
}
```

#### 注意事项

1. 由于二叉搜索树的特性，这个方法比普通二叉树的最近公共祖先问题更加高效。
2. 算法中没有使用递归，而是通过循环遍历树，这使得空间复杂度为 `O(1)`。
3. 如果树中没有 `p` 或 `q`，该方法同样有效。

## 二叉搜索树中的插入操作

### 题目描述

> 给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。返回插入后二叉搜索树的根节点。保证原始二叉搜索树中不存在新值。

### 思路

在二叉搜索树（BST）中执行插入操作通常涉及寻找正确的插入位置，以保持二叉搜索树的性质：对于树中的每个节点，其左子树中的所有节点的值都小于节点的值，而其右子树中的所有节点的值都大于节点的值。插入操作可以通过递归或迭代方法实现。

#### 递归

递归方法的基本思路是：

1. 如果树为空，新节点将成为树的根节点。
2. 否则，比较要插入的节点值与当前节点值：
3. 如果要插入的节点值小于当前节点值，则递归地插入到左子树中。
4. 如果要插入的节点值大于当前节点值，则递归地插入到右子树中

```typescript
function insertIntoBST(root: TreeNode | null, val: number): TreeNode | null {
  if (root === null) {
    return new TreeNode(val);
  }

  if (val < root.val) {
    root.left = insertIntoBST(root.left, val);
  } else {
    root.right = insertIntoBST(root.right, val);
  }

  return root;
}
```

#### 迭代

迭代方法使用循环来找到正确的插入位置：

1. 从根节点开始，遍历树以找到插入点。
2. 根据要插入的节点值与当前节点值的比较结果，决定是向左还是向右移动。
3. 找到正确的插入位置后，创建新节点并插入。

```typescript
function insertIntoBST(root: TreeNode | null, val: number): TreeNode | null {
  if (root === null) {
    return new TreeNode(val);
  }

  let currentNode = root;
  while (true) {
    if (val < currentNode.val) {
      if (currentNode.left === null) {
        currentNode.left = new TreeNode(val);
        break;
      } else {
        currentNode = currentNode.left;
      }
    } else {
      if (currentNode.right === null) {
        currentNode.right = new TreeNode(val);
        break;
      } else {
        currentNode = currentNode.right;
      }
    }
  }

  return root;
}
```

##### 注意事项

1. 在插入新节点后，不需要重新平衡树，因为BST的插入操作不会破坏其性质。
2. 递归方法更简洁但可能涉及深度递归，而迭代方法可能更易于理解且不涉及栈溢出的风险。
3. 插入操作的时间复杂度是 O(h)，其中 h 是树的高度。对于平衡的二叉搜索树，h = log(n)，其中 n 是树中节点的数量。

## 删除二叉搜索树中的节点

### 题目描述

> 给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

### 思路

在二叉搜索树（BST）中删除节点是一个稍微复杂的操作，因为在删除节点后，还需要保持树的二叉搜索属性。删除操作可以分为几种情况：

1. 删除的节点是叶节点：直接删除该节点。
2. 删除的节点只有一个子节点：删除该节点，并用其子节点替换它。
3. 删除的节点有两个子节点： 
   + 找到该节点的中序后继节点（即右子树中的最小节点）或中序前驱节点（即左子树中的最大节点）。 
   + 用后继节点或前驱节点的值替换要删除的节点的值。 
   + 删除后继节点或前驱节点。

#### 递归

1. 如果根节点为空，返回 null。
2. 如果要删除的节点值小于根节点值，则递归地删除左子树中的节点。
3. 如果要删除的节点值大于根节点值，则递归地删除右子树中的节点。
4. 如果找到了要删除的节点，根据上述情况处理。

```typescript
function deleteNode(root: TreeNode | null, key: number): TreeNode | null {
  if (root === null) return null;

  if (key < root.val) {
    root.left = deleteNode(root.left, key);
  } else if (key > root.val) {
    root.right = deleteNode(root.right, key);
  } else {
    if (root.left === null) {
      return root.right;
    } else if (root.right === null) {
      return root.left;
    }

    // 找到右子树的最小节点
    const minNode = getMin(root.right);
    root.val = minNode.val; // 替换值
    root.right = deleteNode(root.right, root.val); // 删除右子树中的最小节点
  }

  return root;
}

function getMin(node: TreeNode): TreeNode {
  while (node.left !== null) {
    node = node.left;
  }
  return node;
}
```

##### 注意事项

+ 删除操作需要考虑多种情况，特别是当删除的节点有两个子节点时。
+ 在查找后继节点时，我们通常寻找右子树中的最小节点。
+ 在某些情况下，删除操作可能会破坏树的平衡，需要额外的操作来重新平衡树，例如在 AVL 树或红黑树中。
+ 删除节点的时间复杂度是 `O(h)`，其中 `h` 是树的高度。
