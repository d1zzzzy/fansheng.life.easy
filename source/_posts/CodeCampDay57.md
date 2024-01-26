---
title: 代码随想录算法训练营第五十七天| 回文子串、最长回文子序列
date: 2024-01-26 17:28:18
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_03.png
---

# 代码随想录算法训练营第五十七天

## 回文子串

### 题目描述

> 给定一个字符串，你的任务是计算这个字符串中有多少个回文子串。

### 题目分析

"回文子串"问题要求计算一个给定字符串中有多少个不同的非空子串是回文。回文是指一个字符串正读和反读都相同的字符串。

### 解决方案

使用动态规划来解决此问题。我们可以创建一个布尔型二维数组 `dp`，其中 `dp[i][j]` 表示字符串中从第 `i` 个字符到第 `j` 个字符的子串是否是回文。

### 代码实现

```typescript
function countSubstrings(s: string): number {
  const n = s.length;
  const dp: boolean[][] = Array.from({ length: n }, () => Array(n).fill(false));
  let count = 0;

  for (let len = 1; len <= n; len++) {
    for (let start = 0; start < n; start++) {
      let end = start + len - 1;
      if (end >= n) break;

      dp[start][end] = (len === 1 || len === 2 || dp[start + 1][end - 1]) && s[start] === s[end];

      if (dp[start][end]) count++;
    }
  }

  return count;
}
```

### 代码解释

+ 遍历所有可能的子串长度 `len` 和起始位置 `start`。
+ 对于每个子串，计算其结束位置 `end`。
+ `dp[start][end]` 为 `true` 当且仅当该子串是回文，即两端字符相等，并且去掉两端字符后仍是回文（对于长度为 `1` 或 `2` 的特殊情况，只需检查两端字符是否相等）。
+ 如果 `dp[start][end]` 为 `true`，则回文子串数量 `count` 加一。

## 最长回文子序列

### 题目描述

> 给定一个字符串 `s`，找到其中最长的回文子序列，并返回该序列的长度。可以假设 `s` 的最大长度为 `1000`。

### 题目分析

"最长回文子序列"问题要求找出给定字符串中最长的回文子序列的长度。与回文子串不同，子序列不需要在原字符串中连续，但其字符在原字符串中的相对顺序必须保持不变。

### 解决方案

此问题可以通过动态规划来解决。我们将创建一个二维数组 `dp`，其中 `dp[i][j]` 表示字符串中从第 `i` 个字符到第 `j` 个字符的最长回文子序列的长度。

### 代码实现

```typescript
function longestPalindromeSubseq(s: string): number {
  const n = s.length;
  const dp: number[][] = Array.from({ length: n }, () => Array(n).fill(0));

  for (let i = n - 1; i >= 0; i--) {
    dp[i][i] = 1; // 单个字符是长度为1的回文子序列
    for (let j = i + 1; j < n; j++) {
      if (s[i] === s[j]) {
        dp[i][j] = dp[i + 1][j - 1] + 2;
      } else {
        dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
      }
    }
  }

  return dp[0][n - 1];
}
```

### 代码解释

+ 遍历字符串的每个字符作为子序列的起始点。
+ 初始化 `dp[i][i]` 为 `1`，因为单个字符总是回文子序列。
+ 如果 `s[i]` 和 `s[j]` 相等，则 `dp[i][j]` 是其内部子序列长度加 `2`。
+ 如果 `s[i]` 和 `s[j]` 不相等，则 `dp[i][j]` 是 `dp[i + 1][j]` 和 `dp[i][j - 1]` 中的较大者。
+ `dp[0][n - 1]` 存储整个字符串的最长回文子序列长度。
