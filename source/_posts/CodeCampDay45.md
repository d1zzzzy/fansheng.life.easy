---
title: 代码随想录算法训练营第四十五天| 爬楼梯、零钱兑换、完全平方数
date: 2024-01-17 16:41:16
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_02.png
---

# 代码随想录算法训练营第四十五天

## 爬楼梯

### 题目描述

> 假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
> 
> 每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

### 思路

1. **定义状态**：创建一个数组 `dp`，其中 `dp[i]` 表示到达第 `i` 阶楼梯的方法数。
2. **初始化状态**：`dp[1]` 初始化为 1（只有一种方式爬到第一阶），`dp[2]` 初始化为 2（两种方式爬到第二阶：一次爬两阶或分两次各爬一阶）。
3. **状态转移**：从第 3 阶开始，每一阶的方法数是前两阶方法数的和，因为你可以从前一阶爬一步上来，或者从前两阶爬两步上来。因此，`dp[i] = dp[i - 1] + dp[i - 2]`。
4. **求解**：最终答案是 `dp[n]`，其中 n 是楼梯的总阶数。

### 代码实现

```typescript
function climbStairs(n: number): number {
  if (n === 1) return 1;
  if (n === 2) return 2;

  let dp1 = 1;
  let dp2 = 2;
  let dp = 0;

  for (let i = 3; i <= n; i++) {
    dp = dp1 + dp2;
    dp1 = dp2;
    dp2 = dp;
  }

  return dp;
}
```

## 零钱兑换

### 题目描述

> 给定不同面额的硬币 `coins` 和一个总金额 `amount`。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。

### 思路

1. **定义状态**：创建一个数组 `dp`，其中 `dp[i]` 表示凑成金额 `i` 所需的最少硬币数量。
2. **初始化状态**：初始化 `dp[0]` 为 `0`，因为凑成金额 `0` 不需要任何硬币。其余的 `dp[i]` 初始化为一个较大的数，比如 `amount + 1`，表示初始时无法凑成该金额。
3. **状态转移**：对于每个金额 `i` 和每种硬币的面额 `coin`，如果 `i - coin` 大于等于 `0`，则尝试用这种面额的硬币来凑，更新 `dp[i] = min(dp[i], dp[i - coin] + 1)`。
4. **求解**：最终答案是 `dp[amount]`。如果 `dp[amount]` 大于 `amount`，说明无法凑成该金额，返回 `-1`。

### 代码实现

```typescript
function coinChange(coins: number[], amount: number): number {
  const max = amount + 1;
  const dp: number[] = new Array(max).fill(max);
  dp[0] = 0;

  for (let i = 1; i <= amount; i++) {
    for (const coin of coins) {
      if (coin <= i) {
        dp[i] = Math.min(dp[i], dp[i - coin] + 1);
      }
    }
  }

  return dp[amount] > amount ? -1 : dp[amount];
}
```

## 完全平方数

### 题目描述

> 给定正整数 `n`，找到若干个完全平方数（比如 `1, 4, 9, 16, ...`）使得它们的和等于 `n`。你需要让组成和的完全平方数的个数最少。

### 思路

1. **定义状态**：创建一个数组 `dp`，其中 `dp[i]` 表示组成整数 `i` 所需的最少平方数的数量。
2. **初始化状态**：初始化 `dp[0]` 为 0，因为组成数字 0 不需要任何平方数。其余的 `dp[i]` 初始化为一个较大的数，例如 `i`，表示初始时的最大平方数数量。
3. **状态转移**：对于每个小于等于 `i` 的平方数 `j*j`，更新 `dp[i] = min(dp[i], dp[i - j*j] + 1)`。
4. **求解**：最终答案是 `dp[n]`，其中 `n` 是给定的正整数。

### 代码实现

```typescript
function numSquares(n: number): number {
  const dp: number[] = new Array(n + 1).fill(Infinity);
  dp[0] = 0;

  for (let i = 1; i <= n; i++) {
    for (let j = 1; j * j <= i; j++) {
      dp[i] = Math.min(dp[i], dp[i - j * j] + 1);
    }
  }

  return dp[n];
}
```
