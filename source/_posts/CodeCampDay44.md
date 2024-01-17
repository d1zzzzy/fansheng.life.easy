---
title: 代码随想录算法训练营第四十四天| 零钱兑换 II、组合总和 Ⅳ
date: 2024-01-17 14:24:22
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第四十四天

## 零钱兑换 II

### 题目描述

> 给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。

### 思路

1. **定义状态**：创建一个数组 `dp`，其中 dp[i] 表示凑成金额 `i` 的方法数。
2. **初始化状态**：初始化 `dp[0]` 为 1，因为凑成金额 0 的方法只有一种，即不使用任何硬币。
3. **状态转移**：对于每种硬币，更新 `dp` 数组。对于金额 `i`，如果 `i` 大于或等于当前硬币的面额，则 `dp[i]` 增加 `dp[i - coin]`，因为 `dp[i - coin]` 表示使用当前硬币时的组合方法数。
4. **求解**：最终答案是 `dp[amount]`。

### 代码实现

```typescript
function change(amount: number, coins: number[]): number {
  const dp: number[] = new Array(amount + 1).fill(0);
  dp[0] = 1;

  for (const coin of coins) {
    for (let i = coin; i <= amount; i++) {
      dp[i] += dp[i - coin];
    }
  }

  return dp[amount];
}
```

## 组合总和 Ⅳ

### 题目描述

> 给你一个由不同整数组成的数组 `nums` ，和一个目标整数 `target` 。请你从 `nums` 中找出并返回总和为 `target` 的元素组合的个数。

### 思路

1. **定义状态**：创建一个数组 `dp`，其中 `dp[i]` 表示凑成总和为 `i` 的排列数。
2. **初始化状态**：初始化 `dp[0]` 为 `1`，因为凑成总和为 0 的方式只有一种，即不使用任何数字。
3. **状态转移**：对于 `dp[i]`（对于每个小于或等于目标数的 `i`），遍历数组中的每个数字。如果当前数字小于或等于 `i`，那么 `dp[i]` 应增加 `dp[i - num]`，因为 `dp[i - num]` 
   表示减去当前数字后剩余总和的排列数。
4. **求解**：最终答案是 `dp[target]`。

### 代码实现

```typescript
function combinationSum4(nums: number[], target: number): number {
  const dp: number[] = new Array(target + 1).fill(0);
  dp[0] = 1;

  for (let i = 1; i <= target; i++) {
    for (const num of nums) {
      if (num <= i) {
        dp[i] += dp[i - num];
      }
    }
  }

  return dp[target];
}
```
