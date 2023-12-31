---
title: 代码随想录算法训练营第三十天| 分发饼干、摆动序列、最大子序和
date: 2023-12-29 13:51:58
tags:
  - Algorithm
  - Greedy
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_03.png
---

# 代码随想录算法训练营第三十一天

## 分发饼干

### 题目描述

> 给定两个数组，分别代表孩子的胃口和饼干的大小。每个孩子最多只能给一块饼干，只有饼干的大小大于等于孩子的胃口时，孩子才会得到满足。求解最多可以满足几个孩子。
> 
> **注意**：每个孩子最多只能给一块饼干。

### 思路

使用贪心算法来解决这个问题。基本思路是尽量用小饼干先满足胃口小的孩子：

1. **排序**：首先对孩子的满足度数组 g 和饼干大小数组 s 进行排序。
2. **分配饼干**：遍历每个饼干，尝试去满足胃口最小的孩子。如果当前饼干能满足一个孩子，就将其分配给这个孩子，然后继续检查下一个饼干。

### 实现

```typescript
function findContentChildren(g: number[], s: number[]): number {
  g.sort((a, b) => a - b);
  s.sort((a, b) => a - b);

  let child = 0, cookie = 0;

  while (child < g.length && cookie < s.length) {
    if (g[child] <= s[cookie]) {
      child++;
    }
    cookie++;
  }

  return child;
}
```

## 摆动序列

### 题目描述

> 如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为摆动序列。第一个差（如果存在的话）可能是正数或负数。少于两个元素的序列也是摆动序列。

### 思路

1. **初始化**：定义两个变量 `up` 和 `down` 分别表示到当前位置为止的摆动序列最后一段是上升或下降的最大长度。
2. **遍历数组**：从第二个元素开始遍历数组。
3. **更新 `up` 和 `down`**：对于每个元素，比较它和前一个元素：
4. 如果当前元素大于前一个元素，则更新 `up` 为 `down + 1`。
5. 如果当前元素小于前一个元素，则更新 `down` 为 `up + 1`。
6. 结果：遍历完成后，摆动序列的最大长度为 `max(up, down)`。

### 实现

```typescript
function wiggleMaxLength(nums: number[]): number {
  if (nums.length < 2) return nums.length;

  let up = 1, down = 1;
  for (let i = 1; i < nums.length; i++) {
    if (nums[i] > nums[i - 1]) {
      up = down + 1;
    } else if (nums[i] < nums[i - 1]) {
      down = up + 1;
    }
  }

  return Math.max(up, down);
}
```

## 最大子序和

### 题目描述

> 给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

### 思路

1. **初始化**：设置两个变量，`maxSoFar` 为到目前为止的最大子序和，`maxEndingHere` 为包含当前位置的最大子序和。
2. **遍历数组**：遍历数组 `nums`。
3. ** 动态更新** ：对于每个元素 `nums[i]`，更新 `maxEndingHere` 为 `max(nums[i], maxEndingHere + nums[i])`，然后更新 `maxSoFar` 为 `max(maxSoFar, 
   maxEndingHere)`。
4. **结果**：返回 `maxSoFar`。

### 实现

```typescript
function maxSubArray(nums: number[]): number {
  let maxSoFar = nums[0], maxEndingHere = nums[0];

  for (let i = 1; i < nums.length; i++) {
    maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
    maxSoFar = Math.max(maxSoFar, maxEndingHere);
  }

  return maxSoFar;
}
```
