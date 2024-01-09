---
title: 代码随想录算法训练营第四十二天| 分割等和子集
date: 2024-01-09 23:31:35
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_03.png
---

# 代码随想录算法训练营第四十二天

## 分割等和子集

### 题目描述

> 给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。
> 
> **注意**：每个数组中的元素不会超过 100，数组的大小不会超过 200。

### 思路

该问题可转化为一个背包问题：是否能从数组中选出一些数字，使得这些数字的总和等于整个数组元素和的一半。

1. 计算数组总和：首先计算数组 `nums` 的总和 `sum`。如果 `sum` 是奇数，直接返回 `false`。
2. 背包问题动态规划： 
   + 创建一个布尔数组 `dp`，大小为` sum / 2 + 1`，其中 `dp[i]` 表示数组是否可以取出一些数字，使得它们的总和为 i。
   + 初始化 `dp[0] = true`（没有数字时总和为 0 是可能的）。
   + 遍历 `nums`，更新 `dp` 数组。
3. 状态转移方程： 
   + 对于每个数字 `num`，反向遍历 `dp` 从 `sum / 2` 到 `num`，更新 `dp[j] = dp[j] || dp[j - num]`。

### 实现

```typescript
function canPartition(nums: number[]): boolean {
	let sum = nums.reduce((a, b) => a + b, 0);
	if (sum % 2 !== 0) return false;
	
	let target = sum / 2;
	let dp = new Array(target + 1).fill(false);
	dp[0] = true;

	for (let num of nums) {
		for (let i = target; i >= num; i--) {
			dp[i] = dp[i] || dp[i - num];
		}
	}

	return dp[target];
}
```

## 背包问题

背包问题是动态规划中的一个经典问题，有多种变体，包括 0-1 背包、完全背包、多重背包等。这些问题通常涉及在给定重量限制的情况下，从一组物品中选择一部分，以达到某种目标（如最大价值）。

### 0-1 背包问题
**问题描述**：给定一组物品，每种物品都有自己的重量和价值，在限定的总重量内选择物品，使得选中的物品总价值最高。

**解题思路**：
1. **状态定义**：`dp[i][w]` 表示对于前 `i` 个物品，当前背包重量为 `w` 时的最大价值。
2. **状态转移**：`dp[i][w] = max(dp[i-1][w], dp[i-1][w-weight[i]] + value[i])`。

### 完全背包问题
**问题描述**：与 0-1 背包类似，但每种物品可以无限选取。

**解题思路**：
1. **状态定义**：同 0-1 背包。
2. **状态转移**：`dp[i][w] = max(dp[i-1][w], dp[i][w-weight[i]] + value[i])`。

### 多重背包问题
**问题描述**：每种物品有一定数量的限制。

**解题思路**：
1. **状态定义**：同 0-1 背包。
2. **状态转移**：考虑每种物品的数量限制。

### 代码示例（0-1 背包问题）
TypeScript 实现：

```typescript
function knapsack(weights: number[], values: number[], W: number): number {
    let N = weights.length;
    let dp = Array.from({ length: N + 1 }, () => new Array(W + 1).fill(0));

    for (let i = 1; i <= N; i++) {
        for (let w = 1; w <= W; w++) {
            if (w - weights[i - 1] < 0) {
                dp[i][w] = dp[i - 1][w];
            } else {
                dp[i][w] = Math.max(dp[i - 1][w], dp[i - 1][w - weights[i - 1]] + values[i - 1]);
            }
        }
    }
    return dp[N][W];
}

// 示例使用
console.log(knapsack([1, 3, 4], [15, 20, 30], 4)); // 输出 35
```

### 注意事项
- 背包问题的关键是正确定义状态和状态转移方程。
- 不同类型的背包问题状态转移方程略有不同。
- 在实际编码时，注意数组的初始化和边界条件。
