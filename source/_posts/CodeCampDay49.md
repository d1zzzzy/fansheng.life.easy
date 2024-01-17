---
title: 代码随想录算法训练营第四十九天| 买卖股票的最佳时机、买卖股票的最佳时机 II
date: 2024-01-17 18:23:12
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第四十九天

## 买卖股票的最佳时机

### 题目描述

> 给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。
> 
> 你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的 **最大利润** 。

### 思路

1. **定义状态**：记录到目前为止的最低股票价格 `minPrice` 和最大利润 `maxProfit`。
2. **初始化状态**：初始时，设置 `minPrice` 为无穷大，`maxProfit` 为 0。
3. **遍历股票价格**：遍历数组中的每个价格，更新 `minPrice` 和 `maxProfit`。 
   + 更新 `minPrice` 为当前价格和 `minPrice` 的较小者。
   + 更新 `maxProfit` 为当前价格减去 `minPrice` 和 `maxProfit` 的较大者。 
4. **求解**：完成遍历后，`maxProfit` 即为最大利润。

### 代码实现

```typescript
function maxProfit(prices: number[]): number {
  let minPrice = Infinity;
  let maxProfit = 0;

  for (let price of prices) {
    if (price < minPrice) {
      minPrice = price;
    } else if (price - minPrice > maxProfit) {
      maxProfit = price - minPrice;
    }
  }

  return maxProfit;
}
```

## 买卖股票的最佳时机 II

### 题目描述

> 给定一个数组 `prices` ，其中 `prices[i]` 是一支给定股票第 `i` 天的价格。
> 
> 设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

### 思路

1. 持有股票（hold）：对于每一天，如果我们持有股票，那么最大利润是多少？
2. 不持有股票（notHold）：对于每一天，如果我们不持有股票，那么最大利润是多少？

这两种状态会根据是否买入或卖出股票在每一天发生转换。

**状态转换方程**

+ `hold[i] = max(hold[i-1], notHold[i-1] - prices[i])`：第 `i` 天持有股票的最大利润是昨天就持有股票的利润，或者昨天没有但今天买入的利润。
+ `notHold[i] = max(notHold[i-1], hold[i-1] + prices[i])`：第 `i` 天不持有股票的最大利润是昨天就没有股票的利润，或者昨天持有但今天卖出的利润。

### 代码实现

```typescript
function maxProfit(prices: number[]): number {
  const n = prices.length;
  if (n <= 1) return 0;

  let hold = -prices[0], notHold = 0;

  for (let i = 1; i < n; i++) {
    hold = Math.max(hold, notHold - prices[i]);
    notHold = Math.max(notHold, hold + prices[i]);
  }

  return notHold;
}
```
