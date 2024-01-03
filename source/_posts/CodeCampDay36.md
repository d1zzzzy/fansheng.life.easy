---
title: 代码随想录算法训练营第三十六天| 无重叠区间、划分字母区间、合并区间
date: 2024-01-03 09:20:47
tags:
  - Algorithm
  - Greedy
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_01.jpg
---

# 代码随想录算法训练营第三十六天

## 无重叠区间

### 题目描述

> 给定一个区间的集合，找到需要移除区间的最小数量，使得剩余区间互不重叠。

### 思路

1. **排序**：首先根据每个区间的结束时间对所有区间进行排序。
2. **选择区间**：遍历排序后的区间，每次选择结束时间最早的区间，同时排除所有与它重叠的区间。
3. **计算移除数量**：每次排除重叠区间时，移除数量加一。

### 实现

```typescript
function eraseOverlapIntervals(intervals: number[][]): number {
  if (intervals.length === 0) return 0;

  intervals.sort((a, b) => a[1] - b[1]);

  let count = 0;
  let end = intervals[0][1];

  for (let i = 1; i < intervals.length; i++) {
    if (intervals[i][0] < end) {
      count++;
    } else {
      end = intervals[i][1];
    }
  }

  return count;
}
```

## 划分字母区间

### 题目描述

> 给定一个字符串 `S`，将 `S` 分割成一些子串，使得每个子串都是由同一种字符组成。

### 思路

1. **记录字母最后出现的位置**：首先遍历字符串 `S`，记录每个字母最后一次出现的位置。
2. **遍历字符串并划分区间**：再次遍历字符串，用一个变量 `end` 记录当前遍历字母的最远边界（即当前字母和已遍历字母的最后出现位置中的最远位置）。当遍历到 `end` 时，当前片段结束，开始新的片段。

### 实现

```typescript
function partitionLabels(S: string): number[] {
  let lastIndices = new Map<string, number>();
  for (let i = 0; i < S.length; i++) {
    lastIndices.set(S[i], i);
  }

  let partitions: number[] = [];
  let start = 0, end = 0;
  for (let i = 0; i < S.length; i++) {
    end = Math.max(end, lastIndices.get(S[i])!);
    if (i === end) {
      partitions.push(end - start + 1);
      start = i + 1;
    }
  }
  return partitions;
}
```

## 合并区间

### 题目描述

> 给出一个区间的集合，请合并所有重叠的区间。

### 思路

1. **排序**：首先按照区间的起始位置排序。
2. **合并区间**：遍历区间数组，对于每个区间： 
   + 如果结果数组为空，或者当前区间的起始位置大于结果数组中最后区间的结束位置，则不重叠，直接添加到结果数组。
   + 否则，它们重叠，更新结果数组中最后区间的结束位置为当前区间和最后区间结束位置的较大值。

### 实现

```typescript
function merge(intervals: number[][]): number[][] {
  if (intervals.length === 0) return [];

  intervals.sort((a, b) => a[0] - b[0]);
  let merged: number[][] = [intervals[0]];

  for (let i = 1; i < intervals.length; i++) {
    let last = merged[merged.length - 1];

    if (intervals[i][0] > last[1]) {
      merged.push(intervals[i]);
    } else {
      last[1] = Math.max(last[1], intervals[i][1]);
    }
  }

  return merged;
}
```
