---
title: 代码随想录算法训练营第五十五天| 判断子序列、不同的子序列
date: 2024-01-26 17:03:57
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第五十五天

## 判断子序列

### 题目描述

> 给定字符串 s 和 t ，判断 s 是否为 t 的子序列。

### 双指针

#### 思路

可以使用双指针方法来解决这个问题。同时遍历两个字符串，并检查第一个字符串的每个字符是否按顺序出现在第二个字符串中。

#### 代码实现

```typescript
function isSubsequence(s: string, t: string): boolean {
  let sIndex = 0, tIndex = 0;

  while (sIndex < s.length && tIndex < t.length) {
    if (s[sIndex] === t[tIndex]) {
      sIndex++;
    }
    tIndex++;
  }

  return sIndex === s.length;
}
```
#### 代码解释

+ 设置两个指针 `sIndex` 和 `tIndex` 分别指向两个字符串 `s` 和 `t` 的起始位置。
+ 遍历字符串 `t`。如果在 `t` 中找到了与 `s` 中当前字符相同的字符，则移动 `sIndex` 指针。
+ 继续在 `t` 中移动 `tIndex` 指针，直到遍历完整个字符串或找到完整的子序列。
+ 最后，如果 `sIndex` 等于 `s` 的长度，说明 `s` 是 `t` 的子序列。

### 动态规划

#### 思路

+ 创建一个二维布尔型数组 `dp`，其中 `dp[i][j]` 表示子字符串 `s[0...i-1]` 是否为 `t[0...j-1]` 的子序列。
+ 遍历两个字符串，根据当前字符是否匹配更新 `dp` 数组。

#### 代码实现

```typescript
function isSubsequence(s: string, t: string): boolean {
  const m = s.length, n = t.length;
  const dp: boolean[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(false));

  for (let j = 0; j <= n; j++) {
    dp[0][j] = true; // 空字符串总是任何字符串的子序列
  }

  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (s[i - 1] === t[j - 1]) {
        dp[i][j] = dp[i - 1][j - 1];
      } else {
        dp[i][j] = dp[i][j - 1];
      }
    }
  }

  return dp[m][n];
}
```

#### 代码实现

+ 初始化 `dp`，其中 `dp[0][...]` 表示空字符串是任何字符串的子序列，因此初始化为 `true`。
+ 遍历 `s` 和 `t` 的每个字符，更新 `dp[i][j]`： 
  + 如果 `s[i - 1]` 和 `t[j - 1]` 相等，则 `dp[i][j]` 的值取决于 `dp[i - 1][j - 1]`。
  + 如果不相等，则 `dp[i][j]` 的值取决于 `dp[i][j - 1]`，表示即使没有当前的 `t[j - 1]` 字符，`s[0...i-1]` 依然可能是 `t[0...j-2]` 的子序列。
+ 最后，`dp[m][n]` 表示 `s` 是否是 `t` 的子序列。

## 不同的子序列

### 题目描述

> 给定一个字符串 `s` 和一个字符串 `t` ，计算在 `s` 的子序列中 `t` 出现的个数。

### 思路

我们将使用一个二维数组 dp，其中 dp[i][j] 表示考虑原字符串的前 i 个字符和目标字符串的前 j 个字符时的不同子序列数。

### 代码实现

```typescript
function numDistinct(s: string, t: string): number {
  const MOD = 1e9 + 7;
  const m = s.length, n = t.length;
  const dp: number[][] = Array.from({ length: m + 1 }, () => Array(n + 1).fill(0));

  for (let i = 0; i <= m; i++) {
    dp[i][0] = 1;  // 空字符串是任何字符串的子序列
  }

  for (let i = 1; i <= m; i++) {
    for (let j = 1; j <= n; j++) {
      if (s[i - 1] === t[j - 1]) {
        dp[i][j] = (dp[i - 1][j - 1] + dp[i - 1][j]) % MOD;
      } else {
        dp[i][j] = dp[i - 1][j];
      }
    }
  }

  return dp[m][n];
}
```

### 代码解释

+ 初始化 `dp` 数组，`dp[i][0]` 表示空字符串是任何字符串的子序列，因此对所有 `i` 设置为 `1`。
+ 遍历两个字符串的每个字符，更新 `dp[i][j]`： 
  + 如果 `s[i - 1]` 和 `t[j - 1]` 相等，则 `dp[i][j]` 是 `dp[i - 1][j - 1]`（考虑当前字符）和 `dp[i - 1][j]`（不考虑当前字符）的和。
  + 如果不相等，则 `dp[i][j]` 的值等于 `dp[i - 1][j]`，即不考虑当前的 `s[i - 1]` 字符。
+ 返回 `dp[m][n]`，即考虑整个字符串 `s` 和 `t` 时的不同子序列数。

