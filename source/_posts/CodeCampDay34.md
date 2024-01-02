---
title: 代码随想录算法训练营第三十四天| K 次取反后最大化的数组和、加油站、分发糖果
date: 2024-01-02 10:14:28
tags:
  - Algorithm
  - Greedy
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第三十四天

## K 次取反后最大化的数组和

### 题目描述

> 给定一个整数数组 `A`，我们只能用以下方法修改该数组：我们选择某个索引 `i` 并将 `A[i]` 替换为 `-A[i]`，然后总共重复这个过程 `K` 次。（我们可以多次选择同一个索引 `i`。）
> 
> 以这种方式修改数组后，返回数组可能的最大和。

### 思路

使用贪心算法来解决这个问题的思路是先将数组中最小的负数依次转变为正数。如果 `K` 次操作未用完，且数组中没有负数了，那么应该反复对数组中最小的正数进行取反操作（因为反复取反不改变和）。整个过程如下：

1. **排序**：首先将数组排序。
2. **贪心取反**：依次对数组中的负数取反，直到用完 `K` 次操作或没有更多的负数。
3. **处理剩余操作**：如果还有剩余的操作次数，且次数为奇数，对数组中最小的元素再次取反（因为偶数次取反相当于没有操作）。
4. **计算总和**：计算数组所有元素的总和。

### 实现

```typescript
function largestSumAfterKNegations(A: number[], K: number): number {
  A.sort((a, b) => a - b);

  for (let i = 0; i < A.length && K > 0; i++) {
    if (A[i] < 0) {
      A[i] = -A[i];
      K--;
    } else {
      break;
    }
  }

  if (K % 2 === 1) A.sort((a, b) => a - b)[0] *= -1;
  return A.reduce((acc, val) => acc + val, 0);
}
```

## 加油站

### 题目描述

> 在一条环路上有 `N` 个加油站，其中第 `i` 个加油站有汽油 `gas[i]` 升。
> 
> 你有一辆油箱容量无限的的汽车，从第 `i` 个加油站开往第 `i+1` 个加油站需要消耗汽油 `cost[i]` 升。你从其中的一个加油站出发，开始时油箱为空。
> 
> 如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。

### 思路

使用贪心算法来解决。主要思路是尝试从每个加油站出发，检查是否能够环绕一圈。如果从某个加油站出发无法到达下一个加油站，那么这个加油站之前的所有加油站都不能作为起点。

1. 初始化：设置两个变量，`totalTank` 表示总油量和消耗的差值，`currTank` 表示当前油量和消耗的差值。
2. 遍历加油站：遍历每个加油站，更新 `totalTank` 和 `currTank`。
3. 检查是否能到达下一个加油站：如果 `currTank` 小于 0，说明无法从当前加油站或之前的加油站出发，将起点设置为下一个加油站，并重置 `currTank`。
4. 检查是否能环绕一圈：最后，如果 `totalTank` 小于 0，则无法环绕一圈；否则，返回起点加油站的索引。

### 实现

```typescript
function canCompleteCircuit(gas: number[], cost: number[]): number {
  let totalTank = 0, currTank = 0, startingStation = 0;

  for (let i = 0; i < gas.length; i++) {
    totalTank += gas[i] - cost[i];
    currTank += gas[i] - cost[i];
    if (currTank < 0) {
      startingStation = i + 1;
      currTank = 0;
    }
  }

  return totalTank >= 0 ? startingStation : -1;
}
```

## 分发糖果

### 题目描述

> 给定一个数组 `ratings`，请你将数组 `ratings` 中的元素按照如下规则进行排序：
> 
> - 每个 `ratings[i]` 需要至少分配 `1` 个糖果。
> - 相邻的孩子中，评分高的孩子必须获得更多的糖果。
> - 需要尽可能地满足上述两个条件。
> - 返回需要分配糖果的最少数量。

### 思路

使用两次贪心策略来解决这个问题。首先从左到右遍历一次，确保每个孩子与其左边的孩子相比，如果评分更高，则分得更多的糖果；然后从右到左遍历一次，确保每个孩子与其右边的孩子相比也满足相同的规则。

1. 初始化：创建一个和 `ratings` 相同长度的数组 `candies`，用来存储每个孩子分到的糖果数，初始值为 1。
2. 从左到右遍历：遍历 `ratings`，如果右边孩子的评分比左边孩子高，增加右边孩子的糖果数。
3. 从右到左遍历：再次遍历 `ratings`，如果左边孩子的评分比右边孩子高，并且左边孩子的糖果数不多于右边孩子，增加左边孩子的糖果数。
4. 计算总糖果数：将 `candies` 数组中的所有数值相加，得到最少需要的糖果数。

### 实现

```typescript
function candy(ratings: number[]): number {
  let n = ratings.length;
  let candies = new Array(n).fill(1);

  for (let i = 1; i < n; i++) {
    if (ratings[i] > ratings[i - 1]) {
      candies[i] = candies[i - 1] + 1;
    }
  }

  for (let i = n - 2; i >= 0; i--) {
    if (ratings[i] > ratings[i + 1]) {
      candies[i] = Math.max(candies[i], candies[i + 1] + 1);
    }
  }

  return candies.reduce((a, b) => a + b);
}
```

#### 注意事项

+ 在从左到右和从右到左的遍历中，都需要考虑当前孩子与相邻孩子的评分比较。
+ 第二次遍历时，需要使用 `Math.max` 来确保孩子的糖果数不会因为左边的规则而减少。
+ 这个问题中的贪心策略是在满足局部最优（每个孩子与其相邻孩子的比较）的情况下达到全局最优。
