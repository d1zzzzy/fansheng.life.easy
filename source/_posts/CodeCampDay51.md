---
title: 代码随想录算法训练营第五十一天| 最佳买卖股票时机含冷冻期、买卖股票的最佳时机含手续费
date: 2024-01-19 10:24:06
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_02.png
---

# 代码随想录算法训练营第五十一天

## 最佳买卖股票时机含冷冻期

### 题目描述

> 给定一个整数数组，其中第 i 个元素代表了第 i 天的股票价格 。​
> 
> 设计一个算法计算出**最大利润**。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）：
> 
> + 你**不能**同时参与多笔交易（即，你必须在再次购买前出售掉之前的股票）。
> + 卖出股票后，你**无法**在第二天买入股票（即，冷冻期为 1 天）。

### 思路

动态规划是解决这个问题的关键。我们需要维护三种状态：

1. **持有股票（hold）**：在第 `i` 天结束时持有股票时的最大利润。
2. **不持有股票，处于冷冻期（frozen）**：在第 `i` 天结束时，处于冷冻期的最大利润。
3. **不持有股票，不处于冷冻期（unfrozen）**：在第 `i` 天结束时，不处于冷冻期的最大利润。

#### 状态转换方程
- `hold[i]` = max(`hold[i-1]`, `unfrozen[i-1] - prices[i]`)：第 `i` 天持有股票可以由前一天持有股票或前一天未持有但今天买入（非冷冻期）得来。
- `frozen[i]` = `hold[i-1] + prices[i]`：第 `i` 天处于冷冻期意味着前一天卖出了股票。
- `unfrozen[i]` = max(`unfrozen[i-1]`, `frozen[i-1]`)：第 `i` 天不持有股票且不处于冷冻期可以由前一天同样不持有股票（无论是否冷冻）得来。

### 代码实现

```typescript
function maxProfit(prices: number[]): number {
  const n = prices.length;
  if (n === 0) return 0;

  const hold = new Array(n).fill(0);
  const frozen = new Array(n).fill(0);
  const unfrozen = new Array(n).fill(0);

  hold[0] = -prices[0];

  for (let i = 1; i < n; i++) {
    hold[i] = Math.max(hold[i - 1], unfrozen[i - 1] - prices[i]);
    frozen[i] = hold[i - 1] + prices[i];
    unfrozen[i] = Math.max(unfrozen[i - 1], frozen[i - 1]);
  }

  return Math.max(frozen[n - 1], unfrozen[n - 1]);
}

// 示例
const prices = [1, 2, 3, 0, 2];
console.log(maxProfit(prices)); // 输出结果
```

## 买卖股票的最佳时机含手续费

### 题目描述

> 给定一个整数数组 `prices`，其中第 `i` 个元素代表了第 `i` 天的股票价格 ；非负整数 `fee` 代表了交易股票的手续费用。
>
> 你可以无限次地完成交易，但是你**每次交易都需要付手续费**。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

### 思路
这个问题可以使用动态规划来解决。我们需要维护两种状态：

1. **持有股票（hold）**：在第 `i` 天结束时持有股票时的最大利润。
2. **不持有股票（notHold）**：在第 `i` 天结束时不持有股票时的最大利润。

#### 状态转换方程
- `hold[i]` = max(`hold[i-1]`, `notHold[i-1] - prices[i]`)：第 `i` 天持有股票可以由前一天持有股票或前一天未持有但今天买入得来。
- `notHold[i]` = max(`notHold[i-1]`, `hold[i-1] + prices[i] - fee`)：第 `i` 天不持有股票可以由前一天不持有股票或前一天持有股票但今天卖出（扣除手续费）得来。

### 代码实现

```typescript
function maxProfit(prices: number[], fee: number): number {
  const n = prices.length;
  let hold = -prices[0];
  let notHold = 0;

  for (let i = 1; i < n; i++) {
    hold = Math.max(hold, notHold - prices[i]);
    notHold = Math.max(notHold, hold + prices[i] - fee);
  }

  return notHold;
}

// 示例
const prices = [1, 3, 2, 8, 4, 9];
const fee = 2;
console.log(maxProfit(prices, fee)); // 输出结果
```
