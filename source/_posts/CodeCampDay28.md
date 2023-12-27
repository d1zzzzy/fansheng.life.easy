---
title: 代码随想录算法训练营第三十天| 非递减子序列、全排列II
date: 2023-12-27 19:53:34
tags:
  - Algorithm
  - Backtrack
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_03.png
---

# 代码随想录算法训练营第三十天

## 非递减子序列

### 题目描述

> 给你一个整数数组 nums ，找出并返回所有该数组中不同的递增子序列，递增子序列中 至少有两个元素 。你可以按 任意顺序 返回答案。
>
> 数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

### 思路

1. **回溯法**：使用回溯法来逐个尝试数组中的每个数字，并构建可能的子序列。
2. **维护非递减顺序**：在构建子序列时，确保添加的元素不小于子序列中的最后一个元素，以保持非递减顺序。
3. **去重**：如果数组中有重复元素，需要确保同一元素在同一位置上不被重复选择。

### 实现

```typescript
function findSubsequences(nums: number[]): number[][] {
  let results: number[][] = [];
  let path: number[] = [];

  function backtrack(start: number) {
    if (path.length > 1) {
      results.push([...path]);
    }

    let used: boolean[] = Array(201).fill(false); // 用于标记同一层级是否使用过相同的数字

    for (let i = start; i < nums.length; i++) {
      if ((path.length === 0 || nums[i] >= path[path.length - 1]) && !used[nums[i] + 100]) {
        path.push(nums[i]);
        used[nums[i] + 100] = true; // 标记当前数字已使用
        backtrack(i + 1);
        path.pop();
      }
    }
  }

  backtrack(0);
  return results;
}
```

## 全排列II

### 题目描述

> 给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

### 思路

由于输入数组可能包含重复数字，我们需要确保不生成重复的排列。算法步骤如下：

1. **排序**：首先对数组进行排序，以便于处理重复元素。
2. **回溯**：使用回溯算法生成所有可能的排列。
3. **去重**：在回溯过程中，如果一个数字与前一个数字相同，并且前一个数字已经被撤销选择了（不在当前排列中），则跳过当前数字，以避免重复的排列。

### 实现

```typescript
function permuteUnique(nums: number[]): number[][] {
  let results: number[][] = [];
  let path: number[] = [];
  let used: boolean[] = new Array(nums.length).fill(false);
  nums.sort((a, b) => a - b); // 排序

  function backtrack() {
    if (path.length === nums.length) {
      results.push([...path]);
      return;
    }

    for (let i = 0; i < nums.length; i++) {
      if (used[i] || (i > 0 && nums[i] === nums[i - 1] && !used[i - 1])) {
        continue;
      }
      used[i] = true;
      path.push(nums[i]);
      backtrack();
      used[i] = false;
      path.pop();
    }
  }

  backtrack();
  return results;
}
```
