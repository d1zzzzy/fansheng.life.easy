---
title: 代码随想录算法训练营第二天
date: 2023-11-30 18:05:41
tags:
  - Algorithm
  - Array
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/code-camp-day02.png
---

# 代码随想录算法训练营第二天

练习的第二天，有时候发现自己的思路不够清晰，回过头来看题目描述，发现自己的理解有偏差，
所以还是要仔细题目描述，像做数学题一样明确已知条件。

## 长度最小的子数组

> 题目描述:
> 给定一个含有 `n` 个正整数的数组和一个正整数 `target` 。
> 
> 找出该数组中满足其总和大于等于 `target` 的长度最小的 连续子数组 `[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

### 前提/已知/要求

+ 已知
  + 正整数：求和的话，这个值肯定是递增的
+ 要求
  + 连续
  + 最小长度

### 思路

#### 解法一：暴力法

暴力法的思路很简单，就是遍历所有的子数组，然后求和，然后比较大小，最后得到最小的长度。

```typescript
function minSubArrayLen(target: number, nums: number[]): number {
  let minLen = Infinity;
  for (let i = 0; i < nums.length; i++) {
    let sum = 0;
    for (let j = i; j < nums.length; j++) {
      sum += nums[j];
      if (sum >= target) {
        minLen = Math.min(minLen, j - i + 1);
        break;
      }
    }
  }
  return minLen === Infinity ? 0 : minLen;
}
```

##### 复杂度分析

+ 时间复杂度：O(n^2): 嵌套循环 `n * n`
+ 空间复杂度：O(1)

#### 解法二：滑动窗口

滑动窗口的思路是，维护一个窗口，窗口内的元素和大于等于 `target` 时，窗口左侧向右移动，直到窗口内的元素和小于 `target` 为止，然后窗口右侧向右移动，直到窗口内的元素和大于等于 `target` 为止，然后窗口左侧向右移动，直到窗口内的元素和小于 `target` 为止，如此循环，直到窗口右侧到达数组末尾。

##### 区别于暴力解法

滑动窗口就是利用了已知条件，数组元素是正整数，所以数组元素和是递增的，所以可以通过移动窗口的方式来减少计算次数。

##### 代码实现

```typescript
function minSubArrayLen(target: number, nums: number[]): number {
  let i = 0;
  let j = 0;
	let lastArrayLen = 0
	let sum = 0;

  while(j < nums.length) {
		sum += nums[j];

		while (sum >= target) {
			lastArrayLen = lastArrayLen === 0 ? j - i + 1 : Math.min(lastArrayLen, j - i + 1);
			sum -= nums[i++];
		}

		j++;
  }

	return lastArrayLen;
};
```
##### 复杂度分析

+ 时间复杂度：O(n)
+ 空间复杂度：O(1)

## 有序数组的平方

> 题目描述：
> 
> 给你一个按**非递减顺序**排序的整数数组`nums`，返回每个数字的平方组成的新数组，要求也按***非递减顺序***排序。

### 前提/已知/要求

+ 已知
  + 非递减顺序
+ 要求
  + 非递减顺序
  + 平方

### 思路

1. 由于该数组是非递减的，但是平方之后前后的大小关系就不一定了，所以分别用两个指针指向数组的头尾
2. 比较两个指针指向的元素的平方的大小，把较大的那个元素放到新数组的末尾，然后移动指针
   1. 如果左指针指向的元素的平方大于右指针指向的元素的平方，则把左指针指向的元素的平方放到新数组的末尾，然后左指针向右移动一位
   2. 如果左指针指向的元素的平方小于右指针指向的元素的平方，则把右指针指向的元素的平方放到新数组的末尾，然后右指针向左移动一位
3. 重复步骤 2，直到左指针大于等于右指针

### 代码实现

```typescript
  let i = 0; // 头指针
  let j = nums.length - 1; // 尾指针
  let length = nums.length;
  const result: number[] = [];

  while (i <= j) {
    const ii = nums[i] ** 2;
    const jj = nums[j] ** 2;

    // 较大的入栈
    result[--length] = ii > jj ? ii : jj;
    // 移动指针
    ii > jj ? i++ : j--;
  }

  return result;
```

### 复杂度分析

+ 时间复杂度：O(n)
+ 空间复杂度：O(n)
+ n 为数组的长度

## 螺旋矩阵II

> 题目描述：
> 
> 给你一个正整数`n`，生成一个包含`1`到`n^2`所有元素，且元素按顺时针顺序螺旋排列的`n x n`正方形矩阵`matrix`。

### 前提/已知/要求

+ 已知
  + 顺时针

其实这个题目就是一个找规律的题目，我们要做的就是找到合适的规律，然后把规律用代码实现出来。

### 思路

[拆分后的三阶螺旋矩阵](/fansheng.life.easy/images/spiral-matrix.png)

[拆分后的四阶螺旋矩阵](/fansheng.life.easy/images/spiral-matrix-4l.png)

拆分矩阵，每次拆分出一个环，按上、下、左、右的顺序进行拆分。因此，我们需要对于我们设置的拆分规则设定循环边界。

这里也可以用数学归纳法进行归纳：

1. n 阶矩阵被拆成 Math.floor(n / 2) 个环
2. 每个环分为四个部分，上、下、左、右
   1. 上环，行不变，列递增，列的范围为 [start, n - loop]
   2. 右环，列不变，行递增，行的范围为 [start, n - loop]
   3. 下环，行不变，列递减，列的范围为 [n - loop, loop]
   4. 左环，列不变，行递减，行的范围为 [n - loop, loop]
3. 在`loop++ < Math.floor(n / 2)`的循环，反复执行步骤 2
4. 处理奇数情况: 当 n 为奇数时，最后一个元素需要单独处理

### 代码实现

```typescript
function generateMatrix(n) {
  // 高速出口
  if (n === 0) return [];

  if (n === 1) return [[1]];

  let i = 0; // 行
  let j = 0; // 列

  let loop = 0; // 当前循环的环数
  let current = 1; // 当前应该填充的值
  let start = 0; // 当前环的起始值
  const result = []; // 最后的结果

  // 初始化二维数组
  for (let k = 0; k < n; k++) {
    result[k] = [];
  }

  // 根据三阶、四阶拆分的规律：
  // 三阶循环 1 次，剩最后一个元素，可以直接赋值
  // 四阶循环 2 次
  // 可以得出循环的次数为 n / 2,向下取整
  // 奇数次的话，最后一个元素需要单独处理
  while (loop++ < Math.floor(n / 2)) {
    // 处理上边, 循环列
    // 例如：第一圈, start = 0
    // 此循环填充的是第一行的列数 n - loop 内 [start = 0,1,2..., n - loop]
    for (j = start; j < n - loop; j++) {
      result[start][j] = current++;
    }

    // 处理右边, 循环行
    // 例如：第一圈, start = 0
    // 此循环填充的是第一列的行数为 n - loop 内 [start = 0,1,2..., n - loop]
    for (i = start; i < n - loop; i++) {
      result[i][j] = current++;
    }

    // 处理下边, 循环列
    // 此时 j 的初始值为 n - loop
    for (; j >= loop; j--) {
      result[i][j] = current++;
    }
    
    // 处理左边, 循环行
    // 此时 i 的初始值为 n - loop
    for (; i >= loop; i--) {
      result[i][j] = current++;
    }

    start++;
  }

  // 奇数情况，处理最后一个元素
  if (n % 2 === 1) {
    result[start][start] = current;
  }

  return result;
}
```

### 复杂度分析

+ 时间复杂度：O(n^2)
+ 空间复杂度：O(n^2)
+ n 为矩阵的阶数

## 数组知识点总结

1. 注意边界范围，防止越界
2. 双指针法
   1. 快慢指针/滑动窗口
      1. 快指针先走，慢指针后走，这样存在一个区间，可以通过判断来实现区间内连续元素符合某个条件
      2. 处理数组连续元素的问题
   2. 左右指针
3. 循环条件
   1. 循环次数
   2. 循环边界
   3. 循环终止条件
