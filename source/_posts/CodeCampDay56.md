---
title: 代码随想录算法训练营第五十六天| 两个字符串的删除操作、编辑距离
date: 2024-01-26 17:20:41
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_01.jpg
---

# 代码随想录算法训练营第五十六天

## 两个字符串的删除操作

### 题目描述

> 给定两个单词 `word1` 和 `word2`，找到使得 `word1` 和 `word2` 相同所需的最小步数，每步可以删除任意一个字符串中的一个字符。

### 题目分析

"两个字符串的删除操作"问题要求找出使两个字符串相等所需的最少删除次数。这可以转化为求两个字符串的最长公共子序列（`LCS`），然后根据`LCS`来确定需要删除的字符数。

### 解决方案

+ 首先，求出两个字符串的最长公共子序列的长度。
+ 然后，总删除次数是两个字符串的长度之和减去两倍的最长公共子序列长度（因为最长公共子序列中的字符不需要删除）。

### 代码实现

```typescript
function minDistance(word1: string, word2: string): number {
  const lcsLength = longestCommonSubsequence(word1, word2);
  return word1.length + word2.length - 2 * lcsLength;
}

function longestCommonSubsequence(text1: string, text2: string): number {
  const m = text1.length;
  const n = text2.length;
  const dp: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));

  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (text1[i - 1] === text2[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1] + 1;
      } else {
        dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
      }
    }
  }

  return dp[m][n];
}
```

### 代码解释

+ `longestCommonSubsequence` 函数用来计算两个字符串的最长公共子序列长度。
+ `minDistance` 函数先计算出最长公共子序列长度，然后根据该长度和原字符串长度计算出最少的删除次数。

## 编辑距离

### 题目描述

> 给定两个单词 `word1` 和 `word2`，计算出将 `word1` 转换成 `word2` 所使用的最少操作数。

### 题目分析

"编辑距离"问题（也称为 `Levenshtein` 距离）要求确定将一个字符串转换为另一个字符串所需的最少编辑操作数。允许的编辑操作包括插入、删除和替换字符。

### 解决方案

使用动态规划方法来解决此问题。我们将创建一个二维数组 `dp`，其中 `dp[i][j]` 表示将字符串 `word1` 的前 `i` 个字符转换为字符串 `word2` 的前 `j` 个字符所需的最少操作数。

### 代码实现

```typescript
function minDistance(word1: string, word2: string): number {
  const m = word1.length;
  const n = word2.length;
  const dp: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));

  for (let i = 0; i <= m; i++) {
    for (let j = 0; j <= n; j++) {
      if (i === 0) {
        dp[i][j] = j;  // 如果 word1 为空，则需要 j 次插入操作
      } else if (j === 0) {
        dp[i][j] = i;  // 如果 word2 为空，则需要 i 次删除操作
      } else if (word1[i - 1] === word2[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1];  // 如果字符相同，无需操作
      } else {
        dp[i][j] = 1 + Math.min(dp[i - 1][j],    // 删除
                                dp[i][j - 1],    // 插入
                                dp[i - 1][j - 1] // 替换
                                );
      }
    }
  }

  return dp[m][n];
}
```

### 代码解释

+ 初始化 `dp` 数组，处理边界情况（一个字符串为空的情况）。
+ 对于非边界情况，如果当前字符相同，则直接继承之前的编辑距离；如果不同，则考虑三种操作（删除、插入、替换），选择最小的编辑距离加一。
+ `dp[m][n]` 存储的是将整个 `word1` 转换为整个 `word2` 的最小编辑距离。
