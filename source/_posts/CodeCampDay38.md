---
title: 代码随想录算法训练营第三十八天| 动态规划基本概念、斐波那契数、爬楼梯、使用最小花费爬楼梯
date: 2024-01-05 18:08:58
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_02.png
---

# 代码随想录算法训练营第三十八天

## 动态规划基本概念

### 基本概念

动态规划算法基于一个事实：某些问题的最优解包含其子问题的最优解。动态规划算法通常用于解决具有重叠子问题和最优子结构特性的问题。

1. **重叠子问题**：问题的递归算法会反复求解相同的子问题，而不是生成新的子问题。
2. **最优子结构**：问题的最优解包含其子问题的最优解。这意味着可以通过组合子问题的最优解来构造原问题的最优解。

### 解题步骤
1. **定义状态**：找出问题的状态，并定义状态空间。
2. **确定状态转移方程**：找出状态空间之间的关系，建立状态转移方程。
3. **确定边界条件**：确定初始状态（边界）的值。
4. **计算最优解**：根据状态转移方程，自底向上地计算出整个状态空间。
5. **构造最优解（可选）**：如果问题需要输出解的具体方案，需要从计算的状态空间中构造解。

## 斐波那契数

### 题目描述

> 斐波那契数，通常用 `F(n)` 表示，形成的序列称为斐波那契数列。该数列由 `0` 和 `1` 开始，后面的每一项数字都是前面两项数字的和。也就是：
> 
> ```
> F(0) = 0, F(1) = 1
> F(n) = F(n - 1) + F(n - 2), 其中 n > 1
> ```

### 思路

#### 动态规划

动态规划是解决斐波那契数列的一个有效方法。基本思路是从最小的数开始，逐步计算直到所需的 `F(n)`。

#### 递归

虽然递归方法直观简单，但对于较大的 n，会导致重复计算和较高的时间复杂度。

### 实现

#### 动态规划

```typescript
function fib(n: number): number {
  if (n <= 1) return n;
  let dp = [0, 1];
  for (let i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
  }
  return dp[n];
}
```

#### 递归

```typescript
function fibRecursion(n: number): number {
  if (n <= 1) return n;
  return fibRecursion(n - 1) + fibRecursion(n - 2);
}
```

## 爬楼梯

### 题目描述

> 假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。
> 
> 每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

### 思路

这个问题实质上是一个斐波那契数列问题，因为每一步爬楼梯的方法数量取决于前两步的方法数量之和。

1. **定义状态**：令 `dp[i]` 表示到达第 i 阶楼梯的方法总数。
2. **状态转移方程**：可以得到 `dp[i] = dp[i-1] + dp[i-2]`。
3. **初始化**：初始化 `dp[1] = 1` 和 `dp[2] = 2`（只有 1 阶或 2 阶楼梯时的方法数量）。
4. **遍历**：计算从 `3` 到 `n` 的所有 `dp[i]`。

### 实现

```typescript
function climbStairs(n: number): number {
  if (n <= 2) return n;
  let first = 1, second = 2;
  for (let i = 3; i <= n; i++) {
    let third = first + second;
    first = second;
    second = third;
  }
  return second;
}
```

## 使用最小花费爬楼梯

### 题目描述

> 数组的每个索引作为一个阶梯，第 `i` 个阶梯对应着一个非负数的体力花费值 `cost[i]`（索引从 `0` 开始）。
> 
> 每当你爬上一个阶梯你都要花费对应的体力花费值，然后你可以选择继续爬一个阶梯或者爬两个阶梯。

### 思路

动态规划方法适用于此问题。定义一个数组 `dp`，其中 `dp[i]` 表示达到第 `i` 阶楼梯所需的最小花费。

1. **状态转移方程**：对于第 `i` 阶楼梯，有 `dp[i] = min(dp[i-1], dp[i-2]) + cost[i]`。
2. **初始化**：初始化 `dp[0] = cost[0]` 和 `dp[1] = cost[1]`。
3. **计算 `dp`**：从第 2 阶开始计算到 `n`（楼梯顶）。
4. **最小花费**：最小花费是 `min(dp[n-1], dp[n-2])`。

### 实现

```typescript
function minCostClimbingStairs(cost: number[]): number {
  let n = cost.length;
  let dp = new Array(n);
  dp[0] = cost[0];
  dp[1] = cost[1];

  for (let i = 2; i < n; i++) {
    dp[i] = Math.min(dp[i - 1], dp[i - 2]) + cost[i];
  }

  return Math.min(dp[n - 1], dp[n - 2]);
}
```
