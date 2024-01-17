---
title: 代码随想录算法训练营第四十八天| 打家劫舍、打家劫舍 II、打家劫舍 III
date: 2024-01-17 18:00:09
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_03.png
---

# 代码随想录算法训练营第四十八天

## 打家劫舍

### 题目描述

> 给定一个代表每个房屋存放金额的非负整数数组，计算你在**不触动警报装置的情况下**，能够偷窃到的最高金额。
> 
> **注意**：不能连续偷窃两个相邻的房屋。

### 思路

1. **定义状态**：创建一个数组 `dp`，其中 `dp[i]` 表示到第 `i` 个房子为止可以偷窃到的最大金额。
2. **初始化状态**：初始化 `dp[0]` 为第一个房屋的金额（如果只有一个房屋），`dp[1]` 为前两个房屋中金额较大的一个（如果有两个或以上的房屋）。
3. **状态转移**：从第三个房屋开始，对于每个房屋 `i`，计算 `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`。这里 `dp[i-1]` 表示不偷当前房屋时的最大金额，`dp[i-2] + nums[i]` 
   表示偷当前房屋时的最大金额。
4. **求解**：最终答案是 `dp[n-1]`，其中 n 是房屋的总数。

### 代码实现

```typescript
function rob(nums: number[]): number {
  if (nums.length === 0) return 0;
  if (nums.length === 1) return nums[0];
  
  const dp: number[] = [];
  dp[0] = nums[0];
  dp[1] = Math.max(nums[0], nums[1]);

  for (let i = 2; i < nums.length; i++) {
    dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i]);
  }

  return dp[nums.length - 1];
}
```

## 打家劫舍 II

### 题目描述

> 给定一个代表每个房屋存放金额的非负整数数组，计算你在**不触动警报装置的情况下**，能够偷窃到的最高金额。
> 
> **注意**：不能连续偷窃两个相邻的房屋，同时第一个房屋和最后一个房屋是相邻的。

### 思路

1. **定义状态**：和 "打家劫舍" 一样，创建一个数组 `dp`，其中 `dp[i]` 表示到第 `i` 个房子为止可以偷窃到的最大金额。
2. **初始化状态**：同上。
3. **状态转移**：同上。
4. **处理两个子问题**：分别计算 `dp` 对于从第一间房子到倒数第二间房子和从第二间房子到最后一间房子的结果。
5. **求解**：取两个子问题中的最大值。

### 代码实现

```typescript
function rob(nums: number[]): number {
  if (nums.length === 0) return 0;
  if (nums.length === 1) return nums[0];

  function simpleRob(start: number, end: number): number {
    let prevMax = 0, currMax = 0;
    for (let i = start; i <= end; i++) {
      let temp = currMax;
      currMax = Math.max(prevMax + nums[i], currMax);
      prevMax = temp;
    }
    return currMax;
  }

  return Math.max(simpleRob(0, nums.length - 2), simpleRob(1, nums.length - 1));
}
```
## 打家劫舍 III

### 题目描述

> 给定一个二叉树，每个节点都对应着一个整数值，表示**该节点存放的金额**。
> 
> 如果**打劫**了某个节点，就不能打劫它的**子节点**，否则会触发警报。
> 
> 计算在不触发警报的情况下，能够偷窃到的最高金额。

### 思路

这个问题需要用到树形动态规划。对于树的每个节点，有两种情况：

1. 偷窃当前节点：这意味着不能偷窃其子节点。
2. 不偷窃当前节点：可以偷窃其子节点。

对于树中的每个节点，我们需要计算两个值：

+ 偷窃该节点时能得到的最大金额。
+ 不偷窃该节点时能得到的最大金额。

**方法定义**

+ 定义一个辅助函数 `robInternal`，对于每个节点返回一个长度为 `2` 的数组： 
  + 第一个元素表示不偷窃当前节点时的最大金额。
  + 第二个元素表示偷窃当前节点时的最大金额。

### 代码实现

```typescript
function rob(root: TreeNode | null): number {
  const result = robInternal(root);
  return Math.max(result[0], result[1]);
}

function robInternal(node: TreeNode | null): number[] {
  if (!node) return [0, 0];

  const left = robInternal(node.left);
  const right = robInternal(node.right);

  const robThis = node.val + left[0] + right[0];
  const notRobThis = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);

  return [notRobThis, robThis];
}
```
