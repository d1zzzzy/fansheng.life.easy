---
title: 代码随想录算法训练营第二十六天| 组合总和II、分割回文串
date: 2023-12-25 14:18:59
tags:
  - Algorithm
  - Backtrack
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_00.jpg
---

# 代码随想录算法训练营第二十六天

## 组合总和II

### 题目描述

> 给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

### 思路

1. 排序：首先对数组进行排序，以便于处理重复元素。
2. 回溯：使用回溯算法逐一尝试数组中的每个数字，组成可能的组合。
3. 处理重复元素：由于数组中可能包含重复元素，在回溯过程中要跳过相同的元素，以避免产生重复的组合。
4. 剪枝：如果当前组合的和已经超过 target，则不需要继续添加更多的元素。

### 实现

```typescript
function combinationSum2(candidates: number[], target: number): number[][] {
  let results: number[][] = [];
  let combination: number[] = [];
  candidates.sort((a, b) => a - b); // 排序

  function backtrack(start: number, sum: number) {
    if (sum === target) {
      results.push([...combination]);
      return;
    }

    for (let i = start; i < candidates.length; i++) {
      if (i > start && candidates[i] === candidates[i - 1]) continue; // 跳过重复元素
      if (sum + candidates[i] > target) break; // 剪枝

      combination.push(candidates[i]);
      backtrack(i + 1, sum + candidates[i]);
      combination.pop();
    }
  }

  backtrack(0, 0);
  return results;
}
```

## 分割回文串

### 题目描述

> 给定一个字符串 s，将 s 分割成一些子串，使每个子串都是回文串。返回 s 所有可能的分割方案。

### 思路

1. 回溯：使用回溯算法递归地尝试所有可能的分割方法。
2. 检查回文：在每一步中，检查当前子串是否是回文串。如果是，则继续递归地处理剩余的子串。
3. 记录有效的分割：每当到达字符串的末尾时，记录当前的分割方案。

### 实现

```typescript
function partition(s: string): string[][] {
  let results: string[][] = [];
  let path: string[] = [];

  function isPalindrome(start: number, end: number): boolean {
    while (start < end) {
      if (s[start] !== s[end]) return false;
      start++;
      end--;
    }
    return true;
  }

  function backtrack(start: number) {
    if (start === s.length) {
      results.push([...path]);
      return;
    }

    for (let i = start; i < s.length; i++) {
      if (isPalindrome(start, i)) {
        path.push(s.slice(start, i + 1));
        backtrack(i + 1);
        path.pop();
      }
    }
  }

  backtrack(0);
  return results;
}
```
