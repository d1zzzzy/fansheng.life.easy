---
title: 代码随想录算法训练营第三十九天| 不同路径、不同路径II
date: 2024-01-09 23:10:53
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_05.png
---

# 代码随想录算法训练营第三十九天

## 不同路径

### 题目描述

> 一个机器人位于一个 `m x n` 网格的左上角。机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角。问总共有多少条不同的路径？
> 
> **注意**：`m` 和 `n` 的值均不超过 100。

### 思路

动态规划是解决这个问题的有效方法。可以创建一个二维数组 `dp`，其中 `dp[i][j]` 表示到达 `(i, j)`位置的不同路径数量。

1. **状态转移方程**：`dp[i][j] = dp[i-1][j] + dp[i][j-1]`，即到达当前位置的路径数等于从上方来和从左方来的路径数之和。
2. **初始化**：第一行和第一列的所有位置的路径数都是 1，因为只有一种方法到达。
3. **填充表格**：按行或按列遍历网格，计算每个位置的路径数。

### 实现

```typescript
function uniquePaths(m: number, n: number): number {
	let dp = new Array(m).fill(0).map(() => new Array(n).fill(1));

	for (let i = 1; i < m; i++) {
		for (let j = 1; j < n; j++) {
			dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
		}
	}

	return dp[m - 1][n - 1];
}
```

## 不同路径II

### 题目描述

> 在上题的基础上，网格中加入了障碍物。一个机器人位于一个 `m x n` 网格的左上角。机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角。问总共有多少条不同的路径？
> 
> **注意**：`m` 和 `n` 的值均不超过 100。

### 思路

使用动态规划来解决这个问题，方法与"不同路径"类似，但需要处理障碍物。

1. 初始化和状态转移： 
   + 创建一个二维数组 `dp`，其中 `dp[i][j]` 表示到达 `(i, j)` 位置的不同路径数量。
   + 如果 `(i, j)` 位置有障碍物，则 `dp[i][j] = 0`。
   + 否则，`dp[i][j]` = `dp[i-1][j]` + `dp[i][j-1]`。
2. 边界情况： 
   + 如果起点或终点有障碍物，则无法到达，返回 0。
   + 第一行和第一列的初始化需要考虑障碍物。

### 实现

```typescript
function uniquePathsWithObstacles(obstacleGrid: number[][]): number {
	let m = obstacleGrid.length;
	let n = obstacleGrid[0].length;
	let dp = new Array(m).fill(0).map(() => new Array(n).fill(0));

	// 初始化第一行和第一列
	for (let i = 0; i < m && obstacleGrid[i][0] == 0; i++) dp[i][0] = 1;
	for (let j = 0; j < n && obstacleGrid[0][j] == 0; j++) dp[0][j] = 1;

	// 计算 dp
	for (let i = 1; i < m; i++) {
		for (let j = 1; j < n; j++) {
			if (obstacleGrid[i][j] == 0) {
				dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
			}
		}
	}

	return dp[m - 1][n - 1];
}
```
