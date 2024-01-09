---
title: 代码随想录算法训练营第四十一天| 整数拆分、不同的二叉搜索树
date: 2024-01-09 23:19:18
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第四十一天

## 整数拆分

### 题目描述

> 给定一个正整数 `n`，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。返回你可以获得的最大乘积。
> 
> **注意**：你可以假设 `n` 不小于 2 且不大于 58。

### 思路

使用动态规划来解决这个问题。定义一个数组 `dp`，其中 `dp[i]` 表示将整数 `i` 拆分成至少两个正整数之和后得到的最大乘积。

1. 状态转移方程：`dp[i]` = `max(dp[i], max(j * (i - j), j * dp[i - j]))`，对于所有 `1 <= j < i`。
2. 初始化：初始化 `dp[2] = 1`，因为 `2` 只能拆分成 `1 + 1`。
3. 计算 dp：从 `3` 开始计算到 `n`。
4. 最大乘积：`dp[n]` 就是最大乘积。

### 实现

```typescript
function integerBreak(n: number): number {
	let dp = new Array(n + 1).fill(0);
	dp[2] = 1;

	for (let i = 3; i <= n; i++) {
		for (let j = 1; j < i; j++) {
			dp[i] = Math.max(dp[i], Math.max(j * (i - j), j * dp[i - j]));
		}
	}

	return dp[n];
}
```

## 不同的二叉搜索树

### 题目描述

> 给定一个整数 `n`，求以 `1 ... n` 为节点组成的二叉搜索树有多少种？
> 
> **注意**：你可以假设 `n` 不小于 0 且不大于 58。

### 思路

动态规划是解决这个问题的有效方法。定义一个数组 `dp`，其中 `dp[i]` 表示有 `i` 个节点的二叉搜索树的数量。

1. **状态转移方程**：对于有 `i` 个节点的二叉搜索树，选择其中一个节点作为根，剩下的 `i-1` 个节点可以分配给左子树和右子树。因此，`dp[i]` 是所有可能的左子树和右子树组合的数量之和。
2. **初始化**：`dp[0] = 1` 和 `dp[1] = 1`，没有节点或只有一个节点的树只有一种结构。
3. **计算 dp**：计算从 `2` 到 `n` 的每个 `dp[i]`。

### 实现

```typescript
function numTrees(n: number): number {
	let dp = new Array(n + 1).fill(0);
	dp[0] = 1;
	dp[1] = 1;

	for (let i = 2; i <= n; i++) {
		for (let j = 1; j <= i; j++) {
			dp[i] += dp[j - 1] * dp[i - j];
		}
	}

	return dp[n];
}
```