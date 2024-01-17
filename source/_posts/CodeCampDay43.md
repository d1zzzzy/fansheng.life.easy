---
title: 代码随想录算法训练营第四十三天| 最后一块石头的重量 II、目标和、一和零
date: 2024-01-17 11:24:33
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_00.jpg
---

# 代码随想录算法训练营第四十三天

## 最后一块石头的重量 II

### 题目描述

> 有一堆石头，每块石头的重量都是正整数。
> 
> 每一回合，从中选出两块 最重的 石头，然后将它们一起粉碎。假设石头的重量分别为 x 和 y，且 x <= y。那么粉碎的可能结果如下：
> 
> - 如果 x == y，那么两块石头都会被完全粉碎；
> - 如果 x != y，那么重量为 x 的石头将会完全粉碎，而重量为 y 的石头新重量为 y-x。
> - 最后，最多只会剩下一块 石头。返回此石头 最小的可能重量 。如果没有石头剩下，就返回 0。

### 思路

该问题可转化为一个背包问题：是否能从数组中选出一些数字，使得这些数字的总和等于整个数组元素和的一半。

1. **计算总重量**：首先，计算所有石头的总重量。
2. **转化问题**：将问题转化为一个背包问题。想象一个能装下总重量一半的背包，我们尝试将石头放入背包，目标是让背包中石头的总重量尽可能接近背包的容量（即总重量的一半）。
3. **动态规划**：创建一个布尔型数组 dp，用于存储每个可能的重量是否可达。`dp[i]` 表示通过选择一定的石头，是否能够达到重量 i。
4. **更新 `dp` 数组**：对于每块石头，更新 dp 数组。如果 `dp[j]` 为真，则 `dp[j + weight]` 也应该为真，因为我们可以通过添加当前的石头来达到新的重量。
5. **寻找最佳分割**：从最大可能重量开始向下查找，找到第一个可达的重量，这代表我们可以达到的最接近总重量一半的重量。
6. **计算结果**：最后剩下的石头的最小可能重量是总重量减去两倍我们找到的重量。

### 代码实现

```typescript
function lastStoneWeightII(stones: number[]): number {
  // 计算所有石头的总重量
  const totalWeight = stones.reduce((a, b) => a + b, 0);
  // 目标是将石头分为重量尽可能接近的两堆
  const target = Math.floor(totalWeight / 2);
  const dp: boolean[] = new Array(target + 1).fill(false);
  dp[0] = true;

  // 动态规划，更新dp数组
  for (const weight of stones) {
    for (let j = target; j >= weight; j--) {
      dp[j] = dp[j] || dp[j - weight];
    }
  }

  // 找到最接近目标重量的值
  for (let i = target; i >= 0; i--) {
    if (dp[i]) {
      // 返回两堆石头重量差的最小值
      return totalWeight - 2 * i;
    }
  }

  return 0;
}

// 示例
const stones = [2, 7, 4, 1, 8, 1];
console.log(lastStoneWeightII(stones)); // 输出结果
```

## 目标和

### 题目描述

> 给你一个整数数组 `nums` 和一个整数 `target` 。
> 
> 向数组中的每个整数前添加 `'+'` 或 `'-'` ，然后串联起所有整数，可以构造一个 **表达式** ：
> 
> - 例如，`nums = [2, 1]` ，可以在 `2` 之前添加 `'+'` ，在 `1` 之前添加 `'-'` ，然后串联起来得到表达式 `"+2-1"` 。
> 
> 返回可以通过上述方法构造的、运算结果等于 `target` 的不同 **表达式** 的数目。

### 思路

该问题可转化为一个背包问题：是否能从数组中选出一些数字，使得这些数字和为 `target`。

1. **转化问题**：这个问题可以转化为一个子集划分问题。如果我们把那些加上 `+` 的数放入一个子集，把加上 `-` 的数放入另一个子集，那么我们的目标就是找到两个子集，使得它们的差的绝对值等于 S。
2. **求总和**：计算数组的总和 `sum`。
3. **检查边界条件**：如果 `sum < S` 或者 `(sum + S) % 2 != 0`，则无解，因为我们无法通过添加 `+` 或 `-` 来达到目标 `S`。
4. **定义新目标**：设定新的目标为 `(sum + S) / 2`。现在问题转化为了找到数组子集，使得子集的和等于新目标。
5. **动态规划**：使用一个数组 `dp`，其中 `dp[i]` 表示数组中的数字能组成总和为 i 的不同方法的数量。初始化 `dp[0]` 为 1，因为总和为 0 总是有一种方法（不选取任何数字）。
6. **填充 dp 数组**：遍历数组中的每个数字，然后从新目标向下遍历到这个数字，更新 dp 数组。
7. **最终结果**：`dp[新目标]` 就是问题的答案。

### 代码实现

```typescript
function findTargetSumWays(nums: number[], S: number): number {
  const sum = nums.reduce((a, b) => a + b, 0);

  // 检查是否有解
  if (sum < S || (sum + S) % 2 != 0) return 0;

  const target = (sum + S) / 2;

  // 额外检查，防止创建非法长度的数组
  if (target < 0 || target > sum) return 0;

  const dp: number[] = new Array(target + 1).fill(0);
  dp[0] = 1;

  for (const num of nums) {
    for (let i = target; i >= num; i--) {
      dp[i] += dp[i - num];
    }
  }

  return dp[target];
}
```

## 一和零

### 题目描述

> 给你一个二进制字符串数组 `strs` 和两个整数 `m` 和 `n` 。
> 
> 请你找出并返回 `strs` 的最大子集的大小，该子集中 **最多** 有 `m` 个 `0` 和 `n` 个 `1` 。

### 思路

该问题可转化为一个背包问题：是否能从数组中选出一些数字，使得这些数字的总和等于整个数组元素和的一半。

1. **预处理每个字符串**：对于数组中的每个字符串，计算其中 '0' 和 '1' 的数量。
2. **定义状态**：创建一个三维数组 `dp`，其中 `dp[i][j][k]` 表示在前` i `个字符串中，使用 `j` 个 '0' 和 k 个 '1' 时能得到的最大子集大小。
3. **初始化状态**：初始化 `dp` 数组为 0。
4. **状态转移**：遍历每个字符串，并更新 `dp` 数组。对于每个字符串，我们考虑取或不取这个字符串，并更新状态。
5. **求解**：最终答案是 `dp[length][m][n]`，其中 `length` 是字符串数组的长度。

### 代码实现

```typescript
function findMaxForm(strs: string[], m: number, n: number): number {
  const length = strs.length;
  const dp: number[][][] = Array.from({ length: length + 1 }, () =>
    Array.from({ length: m + 1 }, () => Array(n + 1).fill(0))
  );

  for (let i = 1; i <= length; i++) {
    const zeros = strs[i - 1].split('').filter(c => c === '0').length;
    const ones = strs[i - 1].length - zeros;

    for (let j = 0; j <= m; j++) {
      for (let k = 0; k <= n; k++) {
        dp[i][j][k] = dp[i - 1][j][k];
        if (j >= zeros && k >= ones) {
          dp[i][j][k] = Math.max(dp[i][j][k], dp[i - 1][j - zeros][k - ones] + 1);
        }
      }
    }
  }

  return dp[length][m][n];
}
```
