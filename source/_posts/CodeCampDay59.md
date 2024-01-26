---
title: 代码随想录算法训练营第五十九天| 下一个更大元素 II、接雨水
date: 2024-01-26 17:55:27
tags:
  - Algorithm
  - MonotoneStack
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第五十九天

## 下一个更大元素 II

### 题目描述

> 给定一个循环数组（最后一个元素的下一个元素是数组的第一个元素），输出每个元素的下一个更大元素。数字 `x` 的下一个更大的元素是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 `-1`。

### 题目分析

"下一个更大元素 II" 是一个循环数组的问题，要求找出数组中每个元素的下一个更大元素。数组是循环的，意味着数组的末尾会连接到数组的开头。

### 解决方案

这个问题可以通过使用一个栈来解决。栈用来存储数组元素的索引，而不是元素本身。由于数组是循环的，我们需要遍历两倍长度的数组来模拟循环效果。

### 代码实现

```typescript
function nextGreaterElements(nums: number[]): number[] {
  const n = nums.length;
  const result = new Array(n).fill(-1);
  const stack: number[] = [];

  for (let i = 0; i < n * 2; i++) {
    let num = nums[i % n];
    while (stack.length > 0 && nums[stack[stack.length - 1]] < num) {
      let idx = stack.pop()!;
      result[idx] = num;
    }
    if (i < n) {
      stack.push(i);
    }
  }

  return result;
}
```

### 代码解释

+ 初始化一个结果数组 `result`，长度与输入数组相同，并将所有元素设为 `-1`。
+ 使用栈 `stack` 存储元素的索引。
+ 遍历数组两次来模拟循环。对于每个元素 `num`： 
  + 当栈不为空且当前元素大于栈顶索引对应的元素时，说明找到了更大的元素。更新结果数组，并从栈中弹出索引。
  + 在第一次遍历期间（即 `i < n`），将元素的索引压入栈中。
+ 返回结果数组 `result`。

## 接雨水

### 题目描述

> 给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

### 题目分析

"接雨水"问题要求计算在非负整数数组表示的高度图中能接多少雨水。这个问题可以通过分析每个位置能接的雨水量来解决。

### 解决方案

一个有效的方法是使用两个数组分别存储每个位置左侧和右侧的最高墙高度，然后遍历每个位置，计算能接的雨水量。

### 代码实现

```typescript
function trap(height: number[]): number {
  const n = height.length;
  if (n === 0) return 0;

  const leftMax: number[] = new Array(n);
  const rightMax: number[] = new Array(n);
  let water = 0;

  leftMax[0] = height[0];
  for (let i = 1; i < n; i++) {
    leftMax[i] = Math.max(height[i], leftMax[i - 1]);
  }

  rightMax[n - 1] = height[n - 1];
  for (let i = n - 2; i >= 0; i--) {
    rightMax[i] = Math.max(height[i], rightMax[i + 1]);
  }

  for (let i = 0; i < n; i++) {
    water += Math.min(leftMax[i], rightMax[i]) - height[i];
  }

  return water;
}
```

### 代码解释

+ 使用两个数组 `leftMax` 和 `rightMax` 来存储每个位置左侧和右侧的最高墙高度。
+ 从左到右遍历数组 `height`，更新 `leftMax` 数组。
+ 从右到左遍历数组 `height`，更新 `rightMax` 数组。
+ 再次遍历 `height`，通过 `leftMax[i]` 和 `rightMax[i]` 计算每个位置能接的雨水量，并累加到 `water`。
+ 返回总的雨水量 `water`。
