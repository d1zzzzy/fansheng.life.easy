---
title: 代码随想录算法训练营第六十天| 柱状图中最大的矩形
date: 2024-01-28 09:54:18
tags:
  - Algorithm
  - MonotoneStack
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_00.jpg
---

# 代码随想录算法训练营第六十天

## 柱状图中最大的矩形

### 题目描述

> 给定 `n` 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 `1` 。
>
> 求在该柱状图中，能够勾勒出来的矩形的最大面积。
 
### 题目分析

"柱状图中最大的矩形"问题要求在给定一系列柱状图的高度，计算出能够勾勒出的最大矩形的面积。这个问题是经典的栈应用问题。

### 解决方案

一个有效的方法是使用栈。遍历每个柱子，使用栈来维护一个高度递增的序列。当遇到一个比栈顶柱子矮的柱子时，可以计算前面柱子形成的矩形面积。

### 代码实现

```typescript
function largestRectangleArea(heights: number[]): number {
  let maxArea = 0;
  const stack: number[] = [];
  heights = [0, ...heights, 0]; // 在头尾添加高度为0的柱子

  for (let i = 0; i < heights.length; i++) {
    while (stack.length > 0 && heights[stack[stack.length - 1]] > heights[i]) {
      const height = heights[stack.pop()!];
      const width = i - stack[stack.length - 1] - 1;
      maxArea = Math.max(maxArea, height * width);
    }
    stack.push(i);
  }

  return maxArea;
}
```

### 代码解释

+ 栈初始化时放入一个哨兵值 `-1`，这简化了边界处理。
+ 遍历 `heights` 数组。对于每个柱子： 
  + 当栈顶元素不是哨兵值且当前柱子的高度小于栈顶柱子的高度时，计算并更新最大面积。
  + 将当前柱子的索引压入栈中。
+ 遍历结束后，处理栈中剩余的柱子，计算可能的最大面积。
+ 返回最大面积 `maxArea`。
