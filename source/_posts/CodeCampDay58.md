---
title: 代码随想录算法训练营第五十八天| 每日温度、下一个更大元素 I
date: 2024-01-26 17:38:11
tags:
  - Algorithm
  - MonotoneStack
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_03.png
---

# 代码随想录算法训练营第五十八天

## 每日温度

### 题目描述

> 根据每日气温列表，请重新生成一个列表，对应位置的输出是需要再等待多久温度才会升高超过该日的天数。如果之后都不会升高，请在该位置用 `0` 来代替。

### 题目分析

"每日温度"问题要求根据每日气温列表，计算出需要等待多少天才能有更高的气温。如果未来没有更高的气温，则在结果中填入 0。

### 解决方案

这个问题可以通过使用栈来解决。栈用来存储温度较低的日子的索引，当遇到一个温度较高的日子时，可以计算出栈顶日子与当前日子的差距。

### 代码实现

```typescript
function dailyTemperatures(T: number[]): number[] {
  const n = T.length;
  const result = new Array(n).fill(0);
  const stack: number[] = [];

  for (let i = 0; i < n; i++) {
    while (stack.length !== 0 && T[i] > T[stack[stack.length - 1]]) {
      const idx = stack.pop()!;
      result[idx] = i - idx;
    }
    stack.push(i);
  }

  return result;
}
```

### 代码解释

+ 初始化一个结果数组 `result`，其长度与输入数组 `T` 相同，并将所有元素设置为 `0`。
+ 使用一个栈 `stack` 来存储还未找到更高温度的日子的索引。
+ 遍历温度数组 `T`。对于每一天： 
  + 当栈不为空且当前温度大于栈顶日子的温度时，从栈中弹出索引，并计算差值，更新 `result`。
  + 将当前日子的索引压入栈中。
+ 返回 `result` 数组。

## 下一个更大元素 I

### 题目描述

> 给定两个 **没有重复元素** 的数组 `nums1` 和 `nums2` ，其中 `nums1` 是 `nums2` 的子集。找到 `nums1` 中每个元素在 `nums2` 中的下一个比其大的值。

### 题目分析

"下一个更大元素 I" 问题需要找出在第二个数组中对应第一个数组中每个元素的下一个更大元素。如果在第二个数组中找不到对应的下一个更大元素，则输出 -1。

### 解决方案

这个问题可以通过使用栈以及哈希表来解决。栈用于存储第二个数组中尚未找到下一个更大元素的元素，而哈希表用于存储第二个数组中每个元素的下一个更大元素。

### 代码实现

```typescript
function nextGreaterElement(nums1: number[], nums2: number[]): number[] {
  const stack: number[] = [];
  const map = new Map<number, number>();

  // 遍历 nums2，寻找每个元素的下一个更大元素
  for (const num of nums2) {
    while (stack.length > 0 && stack[stack.length - 1] < num) {
      map.set(stack.pop()!, num);
    }
    stack.push(num);
  }

  // 构建结果数组
  const result = nums1.map(num => map.get(num) ?? -1);
  return result;
}
```

### 代码解释

+ 使用栈 `stack` 来存储 `nums2` 中的元素，确保栈顶元素总是最小的。
+ 遍历 `nums2`，对于每个元素 `num`，检查栈顶元素。如果 `num` 大于栈顶元素，则说明找到了栈顶元素的下一个更大元素。将它们的关系存入哈希表 `map` 中，并从栈中弹出该元素。
+ 如果 `num` 小于或等于栈顶元素，或者栈为空，则将 `num` 压入栈中。
+ 使用 `nums1` 中的每个元素作为键，从 `map` 中检索下一个更大元素。如果找不到，则返回 -1。
