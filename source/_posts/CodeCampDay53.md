---
title: 代码随想录算法训练营第五十三天| 最长公共子序列、不相交的线、最大子数组和
date: 2024-01-26 16:46:42
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_01.jpg
---

# 代码随想录算法训练营第五十三天

## 最长公共子序列

### 题目描述

> 给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长**公共子序列**的长度。如果不存在**公共子序列**，返回 `0` 。

### 思路

+ 创建一个二维数组 `dp`，其中 `dp[i][j]` 表示 `text1` 的前 `i` 个字符和 `text2` 的前 `j` 个字符的最长公共子序列长度。
+ 通过双层循环遍历两个字符串，比较每一对字符。 
  + 如果字符相等，则当前的最长公共子序列长度为左上方的值加一。
  + 如果字符不等，取左边和上方的最大值作为当前值。
+ `dp[m][n]` 存储的是整个字符串的最长公共子序列长度。

### 代码实现
```typescript
function longestCommonSubsequence(text1: string, text2: string): number {
  const m = text1.length;
  const n = text2.length;
  const dp: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));

  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (text1[i - 1] === text2[j - 1]) {
        // 当字符相等时，子序列长度加一
        dp[i][j] = dp[i - 1][j - 1] + 1;
      } else {
        // 当字符不等时，取上或左的最大值
        dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
      }
    }
  }

  return dp[m][n];
}
```

## 不相交的线

### 题目描述

> 在两条独立的水平线上按给定的顺序写下 `nums1` 和 `nums2` 中的整数。
> 
> 现在，可以绘制一些连接两个数字 `nums1[i]` 和 `nums2[j]` 的直线，这些直线需要同时满足满足：
> 
> `nums1[i] == nums2[j]`
> 
> 且绘制的直线不与任何其他连线（非水平线）相交。
> 
> 请注意，连线即使在端点也不能相交：每个数字只能属于一条连线。

### 思路

+ 创建一个二维数组 `dp`，其中 `dp[i][j]` 表示数组 `A` 的前 `i` 个元素和数组 `B` 的前 `j` 个元素之间最长公共子序列的长度。
+ 遍历两个数组，当 `A[i-1]` 和 `B[j-1]` 相等时，`dp[i][j]` 为 `dp[i-1][j-1] + 1`；否则，取 `dp[i-1][j]` 和 `dp[i][j-1]` 中的较大值。
+ `dp[m][n]` 存储的是两个数组的最长公共子序列长度，即最大不相交线的数量。

### 代码实现

```typescript
function maxUncrossedLines(A: number[], B: number[]): number {
  const m = A.length;
  const n = B.length;
  const dp: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));

  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (A[i - 1] === B[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1] + 1;
      } else {
        dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
      }
    }
  }

  return dp[m][n];
}
```

## 最大子数组和

### 题目描述

> 给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

### 思路

使用动态规划的方法来解决。关键思想是维护一个当前最大子数组和的变量，并更新它以包含每个位置的最大子数组和。

### 代码实现

```typescript
function maxSubArray(nums: number[]): number {
  let maxSoFar = nums[0];
  let currentMax = nums[0];

  for (let i = 1; i < nums.length; i++) {
    currentMax = Math.max(nums[i], currentMax + nums[i]);
    maxSoFar = Math.max(maxSoFar, currentMax);
  }

  return maxSoFar;
}
```

### 代码解释

+ `maxSoFar` 存储到目前为止的最大子数组和。
+ `currentMax` 存储包含当前元素的最大子数组和。
+ 对于每个元素 `nums[i]`，更新 `currentMax` 为 `nums[i]` 或 `currentMax + nums[i]` 中的较大者，这表示我们可以选择开始一个新的子数组或继续当前的子数组。
+ 更新 `maxSoFar` 为 `maxSoFar` 和 `currentMax` 中的较大者。
+ 最终 `maxSoFar` 将包含整个数组的最大子数组和。
