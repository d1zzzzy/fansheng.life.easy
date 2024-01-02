---
title: 代码随想录算法训练营第三十二天| 买卖股票的最佳时机II、跳跃游戏 、 跳跃游戏II
date: 2023-12-31 21:29:50
tags:
  - Algorithm
  - Greedy
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_05.png
---

# 代码随想录算法训练营第三十二天

## 买卖股票的最佳时机II

### 题目描述

> 给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
> 
> 设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

### 思路

1. **遍历数组**：从第一天开始，遍历股票价格数组。
2. **寻找买卖机会**：对于每一天，如果明天的价格比今天高，就在今天买入，明天卖出。
3. **累计利润**：将每次交易的利润累加起来。

### 实现

```typescript
function maxProfit(prices: number[]): number {
	let profit = 0;
	for (let i = 0; i < prices.length - 1; i++) {
		if (prices[i + 1] > prices[i]) {
			profit += prices[i + 1] - prices[i];
		}
	}
	return profit;
}
```

#### 注意事项

+ 这种策略的前提是你可以进行无限次的交易，但每次交易需要在再次购买前出售股票。
+ 贪心算法适用于这个问题，因为它寻找局部最优解（每一次小的买卖机会）来达到全局最优解（最大利润）。
+ 贪心算法在这个问题中能够获得最优解，因为它抓住了所有可能的赚钱机会。

## 跳跃游戏

### 题目描述

> 给定一个非负整数数组，你最初位于数组的第一个位置。
> 
> 数组中的每个元素代表你在该位置可以跳跃的最大长度。

### 思路

使用贪心算法解决这个问题。从数组的第一个元素开始，不断更新能跳到的最远位置，如果在某一步最远位置无法再向前移动（即无法超过当前的索引），则返回 false。如果最远位置能到达数组的最后一个位置或更远，则返回 true。

### 实现

```typescript
function canJump(nums: number[]): boolean {
	let maxReach = 0;
	for (let i = 0; i < nums.length; i++) {
		if (i > maxReach) return false;
		maxReach = Math.max(maxReach, i + nums[i]);
		if (maxReach >= nums.length - 1) return true;
	}
	return false;
}
```

#### 注意事项

+ 贪心算法的关键在于每一步都做出当前能达到的最远跳跃。
+ 需要检查是否存在跳跃“死区”，即无法继续向前跳跃的情况。
+ 该算法在最坏情况下的时间复杂度为 O(n)，因为它需要遍历整个数组一次。

## 跳跃游戏II

### 题目描述

> 给定一个非负整数数组，你最初位于数组的第一个位置。
> 
> 数组中的每个元素代表你在该位置可以跳跃的最大长度。

### 思路

1. **初始化**：设置当前能到达的最远位置 maxReach，当前跳跃的边界 end，和跳跃次数 jumps。
2. **遍历数组**：遍历数组，对于每个元素，更新能到达的最远位置 maxReach。
3. **更新跳跃次数**：每当到达当前跳跃的边界时，增加跳跃次数，并更新跳跃边界为当前能到达的最远位置。
4. **返回结果**：返回跳跃次数。

### 实现

```typescript
function jump(nums: number[]): number {
	let jumps = 0, maxReach = 0, end = 0;
	for (let i = 0; i < nums.length - 1; i++) {
		maxReach = Math.max(maxReach, i + nums[i]);
		if (i === end) {
			jumps++;
			end = maxReach;
		}
	}
	return jumps;
}
```

#### 注意事项
+ 在每一步中，都尝试跳到能够使我们更远的位置。
+ 这种贪心策略能够保证以最少的跳跃次数到达数组的末尾。
+ 需要特别注意边界情况的处理，例如数组只有一个元素或无法到达数组末尾的情况。
