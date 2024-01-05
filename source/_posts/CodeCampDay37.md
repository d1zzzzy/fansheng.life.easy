---
title: 代码随想录算法训练营第三十七天| 单调递增的数字、监控二叉树
date: 2024-01-05 16:40:55
tags:
  - Algorithm
  - Greedy
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_05.png
---

# 代码随想录算法训练营第三十七天

## 单调递增的数字

### 题目描述

> 给定一个整数 `n` ，返回 小于或等于 `n` 的最大数字，且数字呈 *单调递增* 。

### 思路

1. **转换成字符数组**：将数字转换为字符数组，方便操作。
2. **找到第一个递减点**：从左到右遍历字符数组，找到第一个不满足单调递增的位置。
3. **调整数字**：将找到的位置的数字减一，然后将这个位置右边的所有数字设置为 9。
4. **转换回整数**：将修改后的字符数组转换回整数。

### 实现

```typescript
function monotoneIncreasingDigits(N: number): number {
  let chars = N.toString().split('');
  let mark = chars.length;

  for (let i = chars.length - 1; i > 0; i--) {
    if (chars[i] < chars[i - 1]) {
      mark = i;
      chars[i - 1] = (parseInt(chars[i - 1]) - 1).toString();
    }
  }

  for (let i = mark; i < chars.length; i++) {
    chars[i] = '9';
  }

  return parseInt(chars.join(''));
}
```

### 注意事项

+ 需要从右向左遍历数字，以便在找到第一个递减点时能够正确地调整数字。
+ 在调整数字后，需要将所有右侧数字设置为 9，这是因为我们希望得到的是小于或等于 N 的最大数字。 
+ 在实现中要注意数字和字符串之间的转换。

## 监控二叉树

### 题目描述

> 给定一个二叉树，我们在树的节点上安装摄像头。
> 
> 节点上的每个摄影头都可以监视其父对象、自身及其直接子对象。
> 
> 计算监控树的所有节点所需的最小摄像头数量。

### 思路

这个问题可以通过递归（自底向上）来解决，主要思路是对每个节点进行状态的定义：

+ 状态 0：节点未被监控
+ 状态 1：节点被监控，但没有摄像头
+ 状态 2：节点放置了摄像头 

对于每个节点，有以下几种情况：

1. 如果一个节点的子节点未被监控，那么这个节点需要放置摄像头。
2. 如果一个节点的子节点放置了摄像头，那么这个节点被监控，但不需要放置摄像头。
3. 如果一个节点的子节点都被监控了，但都没有放置摄像头，那么这个节点未被监控。

### 实现

```typescript
function minCameraCover(root: TreeNode | null): number {
  let result = 0;

  function dfs(node: TreeNode | null): number {
    if (!node) return 1;
    let left = dfs(node.left);
    let right = dfs(node.right);

    if (left === 0 || right === 0) {
      result++;
      return 2;
    }
    return left === 2 || right === 2 ? 1 : 0;
  }

  if (dfs(root) === 0) result++;
  return result;
}
```
