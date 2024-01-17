---
title: 代码随想录算法训练营第四十六天| 单词拆分、多重背包问题
date: 2024-01-17 17:49:01
tags:
  - Algorithm
  - DynamicProgramming
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_03.png
---

# 代码随想录算法训练营第四十六天

## 单词拆分

### 题目描述

> 给定一个非空字符串 s 和一个包含非空单词列表的字典 wordDict，判定 s 是否可以被空格拆分为一个或多个在字典中出现的单词。

### 思路

1. **定义状态**：创建一个布尔型数组 `dp`，其中 `dp[i]` 表示字符串 `s` 的前 `i` 个字符是否可以被拆分成字典中的单词。
2. **初始化状态**：初始化 `dp[0]` 为 `true`，因为空字符串总是符合条件的。
3. **状态转移**：对于字符串 `s` 的每个字符 `s[i]`，检查 `s[0..i]` 的每个子串 `s[j..i]（0 <= j < i）`，如果 `dp[j]` 为 `true` 并且子串 `s[j..i]` 在字典中，那么设置 `dp[i + 1]` 为 `true`。
4. **求解**：最终答案是 `dp[s.length]`。

### 代码实现

```typescript
function wordBreak(s: string, wordDict: string[]): boolean {
  const wordSet = new Set(wordDict);
  const dp: boolean[] = new Array(s.length + 1).fill(false);
  dp[0] = true;

  for (let i = 1; i <= s.length; i++) {
    for (let j = 0; j < i; j++) {
      if (dp[j] && wordSet.has(s.substring(j, i))) {
        dp[i] = true;
        break;
      }
    }
  }

  return dp[s.length];
}
```

## 多重背包问题

### 多重背包问题的通用解法

多重背包问题是一种典型的动态规划问题，其中每种物品有限定的数量，目标是在不超过背包容量的情况下最大化物品的总价值。

#### 问题描述
给定一组物品，每种物品都有自己的重量和价值，并且每种物品有限定的数量。给定一个背包，它能承载的最大重量为 W。需要选择一些物品装入背包，以使得背包中物品的总价值最大，同时不超过背包的最大重量。

#### 解法步骤
1. **定义状态**：创建一个二维数组 `dp`，其中 `dp[i][j]` 表示考虑前 `i` 种物品，在总重量不超过 `j` 的情况下能够获得的最大价值。
2. **初始化状态**：通常将 `dp[0][..]` 和 `dp[..][0]` 初始化为 0，表示没有物品或背包容量为 0 时的总价值。
3. **状态转移**：遍历每种物品，对于每种物品，遍历可以放入的数量，然后更新 `dp` 数组。对于每个物品 `i` 和每个重量 `j`，更新 `dp[i][j] = max(dp[i][j], dp[i - 1][j - k * weight[i]] + k * value[i])`，其中 `k` 是物品 `i` 被选中的数量，且 `k * weight[i] <= j`。
4. **求解**：最终答案是 `dp[n][W]`，其中 `n` 是物品种类数，`W` 是背包最大重量。

#### 优化
- **一维动态规划**：可以将二维动态规划数组简化为一维数组，减少空间复杂度。在这种情况下，需要逆序更新 `dp[j]`。
- **二进制优化**：当物品数量较大时，可以使用二进制方法来优化。即将每种物品分解为若干个二进制组合的物品，例如将数量为 10 的物品分解为数量为 1、2、4、3 的四种物品。

#### 示例代码
这是一个简化的多重背包问题的示例代码，没有包含所有优化。
```typescript
function multipleKnapsack(weights: number[], values: number[], quantities: number[], W: number): number {
  const n = weights.length;
  const dp: number[] = new Array(W + 1).fill(0);

  for (let i = 0; i < n; i++) {
    for (let j = W; j >= weights[i]; j--) {
      for (let k = 1; k <= quantities[i] && k * weights[i] <= j; k++) {
        dp[j] = Math.max(dp[j], dp[j - k * weights[i]] + k * values[i]);
      }
    }
  }

  return dp[W];
}

// 示例
const weights = [2, 3, 4];  // 每种物品的重量
const values = [3, 4, 5];   // 每种物品的价值
const quantities = [3, 2, 1]; // 每种物品的数量
const W = 10; // 背包最大承重
console.log(multipleKnapsack(weights, values, quantities, W)); // 输出结果
```

这种方法在解决多重背包问题时是有效的，但在实际应用中可能需要根据具体问题进行调整和优化。如果您有任何疑问或需要进一步的解释，请随时告诉我。😊👨‍💻👩‍💻
