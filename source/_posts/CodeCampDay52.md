---
title: 代码随想录算法训练营第五十二天| 最长递增子序列、最长连续递增序列、最长重复子数组
date: 2024-01-19 10:25:07
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第五十二天

## 最长递增子序列

### 题目描述

> 给定一个无序的整数数组，找到其中最长上升子序列的长度。

### 题目分析
"最长递增子序列"（Longest Increasing Subsequence，简称 LIS）问题要求在一个未排序的数组中找到最长递增子序列的长度。递增子序列不必连续，但顺序必须保持不变。

### 思路
动态规划是解决这个问题的经典方法。我们维护一个数组 `dp`，其中 `dp[i]` 表示以第 `i` 个元素结尾的最长递增子序列的长度。

#### 状态转移方程
- 初始化：`dp[i]` 初始化为 1，因为每个元素自身就是一个长度为 1 的递增子序列。
- 转移：对于每个 `i`（从 0 到 `n-1`），遍历 `j`（从 0 到 `i-1`）。如果 `nums[i] > nums[j]`，则 `dp[i] = max(dp[i], dp[j] + 1)`。

#### 求解
- 最终答案是 `dp` 数组中的最大值，因为最长递增子序列可以在数组的任何位置结束。

### 代码实现
```typescript
function lengthOfLIS(nums: number[]): number {
  const dp = new Array(nums.length).fill(1);
  let maxLength = 1;

  for (let i = 1; i < nums.length; i++) {
    for (let j = 0; j < i; j++) {
      if (nums[i] > nums[j]) {
        dp[i] = Math.max(dp[i], dp[j] + 1);
      }
    }
    maxLength = Math.max(maxLength, dp[i]);
  }

  return maxLength;
}
```

## 最长连续递增序列

### 题目描述

> 给定一个未经排序的整数数组，找到最长且**连续**的的递增序列。

### 题目分析
"最长连续递增序列"问题是寻找给定数组中最长的连续递增子序列的长度。与"最长递增子序列"不同，这里的“连续”意味着子序列中的元素在原数组中是连续的。

### 思路
这个问题可以通过遍历数组并在遍历过程中记录连续递增的子序列的长度来解决。我们不需要使用动态规划，因为问题只关注连续递增的部分。

#### 算法步骤
1. **初始化**：设置两个变量，`maxLength` 用于记录最长连续递增子序列的长度，`currentLength` 用于记录当前连续递增子序列的长度。
2. **遍历数组**：遍历数组，比较相邻元素。
3. **更新长度**：
	- 如果当前元素大于前一个元素，则 `currentLength` 加一。
	- 否则，重置 `currentLength` 为 1。
4. **更新最大长度**：在每次更新 `currentLength` 后，比较并更新 `maxLength`。
5. **返回结果**：遍历完成后，返回 `maxLength`。

### 代码实现
```typescript
function findLengthOfLCIS(nums: number[]): number {
  if (nums.length === 0) return 0;

  let maxLength = 1;
  let currentLength = 1;

  for (let i = 1; i < nums.length; i++) {
    if (nums[i] > nums[i - 1]) {
      currentLength++;
      maxLength = Math.max(maxLength, currentLength);
    } else {
      currentLength = 1;
    }
  }

  return maxLength;
}
```

## 最长重复子数组

### 题目描述

> 给两个整数数组 `A` 和 `B` ，返回两个数组中公共的、长度最长的子数组的长度。

### 题目分析
"最长重复子数组"问题要求找出两个给定数组中存在的最长公共子数组的长度。这个问题可以通过动态规划来解决，与求字符串的最长公共子序列类似，但区别在于我们需要找的是连续的子数组。

### 思路
动态规划的关键在于定义一个二维数组 `dp` 来存储结果，其中 `dp[i][j]` 表示以 `A[i-1]` 和 `B[j-1]` 结尾的最长重复子数组的长度。

#### 状态转移方程
- 初始化：`dp[i][0]` 和 `dp[0][j]` 初始化为 0，因为任何数组与空数组的最长重复子数组长度为 0。
- 转移：如果 `A[i-1] == B[j-1]`，则 `dp[i][j] = dp[i-1][j-1] + 1`；否则 `dp[i][j] = 0`。

#### 求解
- 最终答案是 `dp` 数组中的最大值。

### 代码实现
```typescript
function findLength(A: number[], B: number[]): number {
  const m = A.length;
  const n = B.length;
  const dp = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));
  let maxLength = 0;

  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (A[i - 1] === B[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1] + 1;
        maxLength = Math.max(maxLength, dp[i][j]);
      }
    }
  }

  return maxLength;
}
```
