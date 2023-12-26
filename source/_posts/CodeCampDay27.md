---
title: CodeCampDay27
date: 2023-12-26 17:09:20
tags:
  - Algorithm
  - Backtrack
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_05.png
---

# 代码随想录算法训练营第二十七天| 复原 IP 地址、子集、子集II

## 复原 IP 地址

### 题目描述

> 给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

### 思路

给定一个只包含数字的字符串，要求将它恢复为所有可能的有效 IP 地址组合。有效的 IP 地址由四个数字组成，每个数字的范围是 0 到 255，且不能有前导零，四个数字之间用.分隔。

1. **回溯法**：从字符串的起始位置开始，递归地尝试所有可能的分割方式。
2. **校验有效性**：在每一步中，确保当前部分是一个有效的 `IP` 地址段。
3. **记录有效解**：一旦找到一个完整且有效的 IP 地址，就将其添加到结果列表中。
4. **终止条件**：当找到四个有效段且已到达字符串末尾时，停止递归。

### 实现

```typescript
function restoreIpAddresses(s: string): string[] {
  let results: string[] = [];
  
  function backtrack(start: number, path: string[], remainingDots: number) {
    if (remainingDots === 0) {
      let segment = s.substring(start);
      if (isValid(segment)) {
        results.push([...path, segment].join('.'));
      }
      return;
    }

    for (let i = 1; i <= 3; i++) {
      let segment = s.substring(start, start + i);
      if (isValid(segment)) {
        path.push(segment);
        backtrack(start + i, path, remainingDots - 1);
        path.pop();
      }
    }
  }

  function isValid(segment: string): boolean {
    if (segment.length > 3) return false;
    if (segment.startsWith('0') && segment.length > 1) return false;
    let value = parseInt(segment);
    return value >= 0 && value <= 255;
  }

  backtrack(0, [], 3);
  return results;
}
```

## 子集

### 题目描述

> 给你一个整数数组 nums ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

1. **路径**：记录当前路径，即目前为止选择的元素集合。
2. **选择列表**：表示我们可以选择的元素集合。
3. **结束条件**：当我们到达决策树的底层，无法再做任何选择时。

```typescript
function subsets(nums: number[]): number[][] {
  let result: number[][] = [];
  let path: number[] = [];

  function backtrack(start: number) {
    result.push([...path]);
    for (let i = start; i < nums.length; i++) {
      path.push(nums[i]);
      backtrack(i + 1);
      path.pop();
    }
  }

  backtrack(0);
  return result;
}
```

## 子集II

### 题目描述

> 给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

### 思路

由于数组中可能包含重复元素，我们需要避免生成重复的子集。算法步骤如下：

1. **排序**：首先对数组进行排序，这样相同的元素会被放在一起，便于后续处理重复元素。
2. **回溯**：使用回溯算法逐一尝试数组中的每个数字，组成可能的子集。
3. **去重**：在每一步中，如果发现当前元素与前一个元素相同，则跳过当前元素，以避免生成重复的子集。

```typescript
function subsetsWithDup(nums: number[]): number[][] {
  let results: number[][] = [];
  let path: number[] = [];
  nums.sort((a, b) => a - b); // 排序

  function backtrack(start: number) {
    results.push([...path]);
    for (let i = start; i < nums.length; i++) {
      if (i > start && nums[i] === nums[i - 1]) continue; // 跳过重复元素
      path.push(nums[i]);
      backtrack(i + 1);
      path.pop();
    }
  }

  backtrack(0);
  return results;
}
```
