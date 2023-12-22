---
title: 代码随想录算法训练营第二十亖天| 回溯算法、组合、组合总和、全排列
date: 2023-12-22 11:10:34
tags:
  - Algorithm
  - Backtrack
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第二十亖天

## 回溯算法

回溯算法是一种通过试错来解决问题的算法。它尝试分步解决一个问题，如果发现当前步骤不能得到有效的解决，或者不能带来最优解，它将撤销上一步或几步的计算，再尝试其他的可能性。这个过程很像在迷宫中探索，当遇到死路时，退回到之前的岔路口选择另一条未探索的路径。

### 通俗理解

> 想象一下，你在尝试完成一道拼图游戏。拼图由多个小块组成，每块都有特定的位置。你尝试将一块块拼图放到合适的位置上，如果发现当前放置的拼图块无法和已放置的拼图块完美拼接，你就会取下这块拼图，然后尝试放置另一块。
>
> 在这个过程中，你可能会多次尝试和错误，每次错误都会让你回到之前的状态，重新尝试其他的拼图块，直到所有的拼图块都正确拼合在一起。

### 特点

+ **系统性地搜索**：回溯算法会尝试所有可能的路径以找到解。
+ **撤销操作**：如果当前路径不通，会撤销到最近的分岔点。
+ **暴力解法**：在某些情况下，回溯算法可能需要尝试所有可能的组合，因此可能非常耗时

## 组合

### 题目描述

> 给定两个整数 n 和 k，返回 1 ... n 中所有可能的 k 个数的组合。

### 实现

```typescript
function combine(n: number, k: number): number[][] {
  let results: number[][] = [];
  let combination: number[] = [];

  function backtrack(start: number) {
    if (combination.length === k) {
      results.push([...combination]);
      return;
    }

    for (let i = start; i <= n; i++) {
      combination.push(i);
      backtrack(i + 1);
      combination.pop();
    }
  }

  backtrack(1);
  return results;
}
```

## 组合总和

### 题目描述

> 给定一个无重复元素的正整数数组 candidates 和一个正整数 target，找出 candidates 中所有可以使数字和为目标数 target 的唯一组合。

### 实现

```typescript
function combinationSum(candidates: number[], target: number): number[][] {
  let results: number[][] = [];
  let combination: number[] = [];

  function backtrack(start: number, total: number) {
    if (total === target) {
      results.push([...combination]);
      return;
    }

    for (let i = start; i < candidates.length; i++) {
      if (total + candidates[i] <= target) {
        combination.push(candidates[i]);
        backtrack(i, total + candidates[i]);
        combination.pop(); // 回溯
      }
    }
  }

  backtrack(0, 0);

  return results;
}
```

## 全排列

### 题目描述

> 给定一个不含重复数字的数组 nums，返回其所有可能的全排列。你可以按任意顺序返回答案。

### 实现

```typescript
function permute(nums: number[]): number[][] {
  let results: number[][] = [];
  let path: number[] = [];

  function backtrack(remaining: number[]) {
    if (path.length === nums.length) {
      results.push([...path]);
      return;
    }

    for (let i = 0; i < remaining.length; i++) {
      path.push(remaining[i]);
      backtrack(remaining.filter((_, index) => index !== i));
      path.pop();
    }
  }

  backtrack(nums);
  return results;
}
```
