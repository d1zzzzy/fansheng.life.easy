---
title: 代码随想录算法训练营第三十五天| 柠檬水找零、根据身高重建队列、用最少数量的箭引爆气球
date: 2024-01-02 11:03:47
tags:
  - Algorithm
  - Greedy
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_02.png
---

# 代码随想录算法训练营第三十五天

## 柠檬水找零

### 题目描述

> 在柠檬水摊上，每一杯柠檬水的售价为 5 美元。
> 
> 顾客排队购买你的产品，（按账单 bills 支付的顺序）一次购买一杯。
> 
> 每位顾客只买一杯柠檬水，然后向你付 5 美元、10 美元或 20 美元。你必须给每个顾客正确找零，也就是说净交易是每位顾客向你支付 5 美元。
> 
> 注意，一开始你手头没有任何零钱。
> 
> 如果你能给每位顾客正确找零，返回 true ，否则返回 false 。

### 思路

使用贪心算法来解决这个问题，关键在于优先使用大面额的钞票进行找零，以保留更多的小面额钞票应对未来的找零需求。

1. **初始化**：记录手头持有的5美元和10美元钞票数量。
2. **遍历账单**：遍历每个顾客支付的账单。 
   + 如果顾客支付了5美元，无需找零，直接增加5美元钞票的数量。
   + 如果顾客支付了10美元，需要找零5美元，如果没有5美元钞票，则返回 `false`；否则，减少5美元钞票数量，增加10美元钞票数量。
   + 如果顾客支付了20美元，优先使用一张10美元和一张5美元进行找零，如果不够，则尝试使用三张5美元进行找零，如果还是不够，则返回 `false`。
3. **返回结果**：如果能够为所有顾客找零，返回 `true`。

### 实现

```typescript
function lemonadeChange(bills: number[]): boolean {
  let five = 0, ten = 0;

  for (let bill of bills) {
    if (bill === 5) {
      five++;
    } else if (bill === 10) {
      if (five === 0) return false;
      five--;
      ten++;
    } else {
      if (ten > 0 && five > 0) {
        ten--;
        five--;
      } else if (five >= 3) {
        five -= 3;
      } else {
        return false;
      }
    }
  }
  return true;
}
```

## 根据身高重建队列

### 题目描述

> 假设有打乱顺序的一群人站成一个队列。每个人由一个整数对 (h, k) 表示，其中 h 是这个人的身高，k 是排在这个人前面且身高大于或等于 h 的人数。编写一个算法来重建这个队列。

### 思路

1. **排序**：首先根据身高降序，`k` 值升序对所有人进行排序。
2. **重建队列**：遍历排序后的数组，根据每个人的 `k` 值，将他们插入到结果队列中的 `k` 索引位置。

### 实现

```typescript
function reconstructQueue(people: number[][]): number[][] {
  people.sort((a, b) => {
    if (a[0] !== b[0]) return b[0] - a[0]; // 身高降序
    return a[1] - b[1]; // k 值升序
  });

  let queue: number[][] = [];
  for (let person of people) {
    queue.splice(person[1], 0, person); // 插入到 k 索引位置
  }
  return queue;
}
```

## 用最少数量的箭引爆气球

### 题目描述

> 在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以纵坐标并不重要，因此只要知道开始和结束的横坐标就足够了。开始坐标总是小于结束坐标。
> 
> 一支弓箭可以沿着 x 轴从不同点完全垂直地射出。在坐标 x 处射出一支箭，若有一个气球的直径的开始和结束坐标为 `xstart`，`xend`， 且满足 `xstart ≤ x ≤ xend`，则该气球会被引爆。可以射出的弓箭的数量没有限制。弓箭一旦被射出之后，可以无限地前进。
> 
> 求解最小的弓箭数量，使得所有气球都被引爆。

### 思路

1. **排序**：根据每个气球区间的结束位置对气球进行排序。
2. **寻找重叠区间**：遍历排序后的气球区间，保持当前重叠区间的最小结束位置。
3. **更新箭数**：如果当前气球的开始位置大于当前重叠区间的结束位置，则需要一支新的箭，并更新当前重叠区间的结束位置为当前气球的结束位置

### 实现

```typescript
function findMinArrowShots(points: number[][]): number {
  if (points.length === 0) return 0;

  points.sort((a, b) => a[1] - b[1]);
  let arrows = 1;
  let currEnd = points[0][1];

  for (let i = 1; i < points.length; i++) {
    if (points[i][0] > currEnd) {
      arrows++;
      currEnd = points[i][1];
    }
  }

  return arrows;
}
```
