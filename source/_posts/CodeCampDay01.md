---
title: 代码随想录算法训练营第一天| 704. 二分查找、27. 移除元素
date: 2023-11-29 15:37:41
comments: true
tags: 
  - Algorithm
  - Array
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/code-camp-day01.png
---

# 代码随想录算法训练营第一天

> 算法: 在数学（算学）和计算机科学之中，指一个被定义好的、计算机可施行其指示的有限步骤或次序，常用于计算、数据处理和自动推理。算法是有效方法，包含一系列定义清晰的指令，并可于有限的时间及空间内清楚的表述出来。

今天开始复习算法内容，顺便结合算法题回想一下数学知识。格式大概为：算法题目、数学联想、思路、代码、复杂度分析。

## 二分查找

### 前提/已知

一般来说，二分查找依赖于以下前提或已知：

+ 数组是升序的
+ 数组是有限的


查找某个 `target` 可以当成求出 `y = target` 在 `x ∈ [0, n)` 区间内与函数的交点。

> ### 数学联想
> 由于数组下标是递增的，数组本身也是递增的，所以可以把下标当成 x 轴
> 数组元素即可以看成一个在某个区域内关于某个映射（函数）的值域
> 查找的目标值即为求 `y = target` 与函数的交点
> 数学意义也就是解方程 `f(x) - target = 0`
> 但是这个映射是随机的，所以无法通过解方程的方式求出交点，只能用类似 **中值定理** 的方式求出交点

### 思路

下标值范围为: `[0, length) = [0, length - 1]` (下标为整数);

1. 如果左侧端点已经比目标值大，此时可以直接返回 -1
2. 取中点的值判断
   3. 如果该值等于目标值，则得到结果
   4. 如果该值比目标值大，则目标值在 `[0, mid]` 区间内， 重复步骤 2
   5. 如果该值比目标值小，则目标值在 `[mid, length - 1]` 区间内， 重复步骤 2

#### 解法一：区域取 [0, length - 1]

```typescript
function search(nums: number[], target: number): number {
	// 初始区间范围
	let left = 0;
	let right = nums.length - 1;

  // 由于区间范围是 [0, length - 1]，所以当 left === right 时，区间范围为 [left, left]，即只有一个元素
	while (left <= right) {
    // 取中点
		const mid = left + ((right - left) >> 1);

		if (nums[mid] === target) {
			return mid;
		} else if (nums[mid] > target) {
			right = mid - 1;
		} else {
			left = mid + 1;
		}
	}
	return -1;
}
```

#### 解法二：区域取 [0, length)

```typescript
function search(nums: number[], target: number): number {
	// 初始区间范围
	let left = 0;
	let right = nums.length;

  // 由于区间范围是 [0, length)，所以退出条件是当 left === right - 1 时，区间范围为 [left, right)，即只有一个元素
	while (left < right) {
    // 取中点
		const mid = left + ((right - left) >> 1);

		if (nums[mid] === target) {
			return mid;
		} else if (nums[mid] > target) {
			// 区别：因为右边界是开区间下一轮不会进入判断，所以这里不需要减一
			right = mid;
		} else {
			left = mid + 1;
		}
	}
	return -1;
}
```

#### 复杂度分析

两种解法的空间和时间复杂度都是:

+ 空间复杂度：O(1)
+ 时间复杂度：O(logn)

## 移除元素

### 思路

移除数组中的某个元素，可以看成是把数组中的某个元素移动到数组末尾，然后把数组末尾的元素删除。

### 解法一：暴力解法

暴力解法的本质是：遍历数组，如果当前元素等于目标值，则把当前元素后面的元素向前移动一位，然后数组长度减一，并且重复这个操作。

```typescript
function removeElement(nums: number[], val: number): number {
  // 遍历数组
  for (let i = 0; i < nums.length; i++) {
	  // 如果当前元素等于目标值
    if (nums[i] === val) {
      for (let j = i + 1; j < nums.length; j++) {
        nums[j - 1] = nums[j];
      }

      i--;
      nums.length--;
    }
  }

  return nums.length;
}
```

#### 复杂度分析

+ 空间复杂度：O(1)
+ 时间复杂度：O(n^2)

### 解法二：快慢指针

双指针解法的本质：交换数组中两项的位置，而至于应该删除的数据全会被移到数组末端，如果需要删除的话，统计一下目标值的数量然后 `arr.length = arr.length - sum` 即可。

```typescript
function removeElement(nums: number[], val: number): number {
	// 慢指针
	let slow = 0;
	// 快指针
	let fast = 0;

	while (fast < nums.length) {
		// 如果快指针指向的元素不等于目标值
		if (nums[fast] !== val) {
			// 把快指针指向的元素赋值给慢指针指向的元素
			nums[slow] = nums[fast];
			// 慢指针向后移动一位
			slow++;
		}
		// 快指针向后移动一位
		fast++;
	}

	return slow;
}
```

#### 复杂度分析

+ 空间复杂度：O(1)
+ 时间复杂度：O(n)
