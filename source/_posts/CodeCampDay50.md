---
title: 代码随想录算法训练营第五十天| 买卖股票的最佳时机 III、买卖股票的最佳时机 IV
date: 2024-01-17 18:37:03
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_05.png
---

# 代码随想录算法训练营第五十天

## 买卖股票的最佳时机 III

### 题目描述

> 给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定的股票在第 `i` 天的价格。
> 
> 设计一个算法来计算你所能获取的最大利润。你最多可以完成 **两笔** 交易。

### 思路

1. **定义状态**： 
   + `dp[i][k][0]` 表示第 `i` 天结束时，最多进行 `k` 笔交易且不持有股票时的最大利润。
   + `dp[i][k][1]` 表示第 `i` 天结束时，最多进行 `k` 笔交易且持有股票时的最大利润。 
2. 状态转移方程： 
   + `dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])`：第 `i` 天不持有股票的最大利润来自于前一天也不持有或者前一天持有但今天卖出。 
   + `dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])`：第 `i` 天持有股票的最大利润来自于前一天持有或者前一天不持有但今天买入。
3. 初始化状态： 
   + 第 0 天不可能持有股票，所以 `dp[0][k][1]` 应初始化为负无穷大。 
   + 第 0 天也不可能有利润，所以 `dp[0][k][0]` 应初始化为 0。 
4. 求解： 
   + 遍历每一天和交易次数，应用状态转移方程更新状态。

### 代码实现

```typescript
function maxProfit(prices: number[]): number {
  if (!prices.length) return 0;

  const dp = Array.from({ length: prices.length }, () =>
    Array.from({ length: 3 }, () => Array(2).fill(0))
  );

  for (let k = 1; k <= 2; k++) {
    dp[0][k][1] = -prices[0];
  }

  for (let i = 1; i < prices.length; i++) {
    for (let k = 1; k <= 2; k++) {
      dp[i][k][0] = Math.max(dp[i - 1][k][0], dp[i - 1][k][1] + prices[i]);
      dp[i][k][1] = Math.max(dp[i - 1][k][1], dp[i - 1][k - 1][0] - prices[i]);
    }
  }

  return dp[prices.length - 1][2][0];
}
```

## 买卖股票的最佳时机 IV

### 题目描述

> 给定一个整数数组 `prices` ，它的第 `i` 个元素 `prices[i]` 是一支给定的股票在第 `i` 天的价格。
> 
> 设计一个算法来计算你所能获取的最大利润。你最多可以完成 **k 笔** 交易。

### 思路

1. 定义状态：
	+ `dp[i][j][0]` 表示第 `i` 天结束时，进行了 `j` 次交易且当前不持有股票的最大利润。
	+ `dp[i][j][1]` 表示第 `i` 天结束时，进行了 `j` 次交易且当前持有股票的最大利润。
2. 状态转移方程： 
   + `dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j][1] + prices[i])`：第 i 天不持有股票的最大利润来自于前一天也不持有或者前一天持有但今天卖出。
   + `dp[i][j][1] = max(dp[i-1][j][1], dp[i-1][j-1][0] - prices[i])`：第 i 天持有股票的最大利润来自于前一天持有或者前一天不持有但今天买入。
3. 初始化状态：
   + 第 0 天不可能持有股票，所以 `dp[0][j][1]` 应初始化为负无穷大。
   + 第 0 天也不可能有利润，所以 `dp[0][j][0]` 应初始化为 0。
4. 求解： 
   + 遍历每一天和交易次数，应用状态转移方程更新状态。

### 代码实现

```typescript
function maxProfit(k: number, prices: number[]): number {
  if (!prices.length) return 0;

  const n = prices.length;
  if (k > n / 2) {
    // 如果 k 大于 n/2，问题变为不限交易次数
    let profit = 0;
    for (let i = 1; i < n; i++) {
      if (prices[i] > prices[i - 1]) {
        profit += prices[i] - prices[i - 1];
      }
    }
    return profit;
  }

  const dp = Array.from({ length: n }, () =>
    Array.from({ length: k + 1 }, () => Array(2).fill(0))
  );

  for (let j = 0; j <= k; j++) {
    dp[0][j][1] = -prices[0];
  }

  for (let i = 1; i < n; i++) {
    for (let j = 1; j <= k; j++) {
      dp[i][j][0] = Math.max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i]);
      dp[i][j][1] = Math.max(dp[i - 1][j][1], dp[i - 1][j - 1][0] - prices[i]);
    }
  }

  return dp[n - 1][k][0];
}
```
