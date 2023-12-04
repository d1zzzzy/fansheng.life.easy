---
title: 代码随想录算法训练营第五天| 有效的字母异位词、两个数组的交集、快乐数、两数之和
date: 2023-12-04 14:11:38
tags:
  - Algorithm
  - HashMap
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/code-camp-day05.png
---

# 代码随想录算法训练营第五天

## 有效的字母异位词

> 题目描述：
> 
> 给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

### 思路

#### 解法一：排序

排序的思路很简单，就是将两个字符串排序，然后比较排序后的字符串是否相等。

```typescript
function isAnagram(s: string, t: string): boolean {
	return s.split('').sort().join('') === t.split('').sort().join('');
}
```

#### 解法二：哈希表

哈希表的思路也很简单，就是遍历字符串，然后将字符串中的字符作为 key，出现的次数作为 value，最后比较两个哈希表是否相等。

```typescript
function isAnagram(s: string, t: string): boolean {
  if (s.length !== t.length) {
    return false;
  }

  const count: { [key: string]: number } = {};

  for (let i = 0; i < s.length; i++) {
    // 字符在 s 串中出现一次，就在 count 中加一
	  // 字符在 t 串中出现一次，就在 count 中减一
	  // 最后如果 count 中的所有值都为 0，说明两个字符串中的字符出现的次数相同
    count[s[i]] = (count[s[i]] || 0) + 1;
    count[t[i]] = (count[t[i]] || 0) - 1;
  }

  for (let key in count) {
    if (count[key] !== 0) {
      return false;
    }
  }

  return true;
}
```

##### 复杂度分析

+ 排序
  + 时间复杂度：O(nlogn)
  + 空间复杂度：O(1)
+ 哈希表
	+ 时间复杂度：O(n)
	+ 空间复杂度：O(1)

## 两个数组的交集

> 题目描述：
> 
> 给定两个数组，编写一个函数来计算它们的交集。

### 思路

#### 解法一：哈希表

遍历第一个数组，然后将第一个数组中的元素作为 key，出现的次数作为 value，然后遍历第二个数组，如果第二个数组中的元素在哈希表中出现过，则将该元素放到结果数组中，并且将哈希表中该元素的 value 减一，最后返回结果数组即可。

```typescript
function intersection(nums1: number[], nums2: number[]): number[] {
  const map: { [key: number]: boolean } = {};
  const result: number[] = [];

  for (let num of nums1) {
    map[num] = true;
  }

  for (let num of nums2) {
    if (map[num]) {
      result.push(num);
      delete map[num];  // 移除元素以避免重复
    }
  }

  return result;
}

```

#### 解法二：排序 + 双指针

先对两个数组进行排序，然后使用双指针的思路，分别用两个指针指向两个数组的头部，然后比较两个指针指向的元素的大小，如果两个元素相等，则将该元素放到结果数组中，并且两个指针都向后移动一位，如果两个元素不相等，则将较小的那个元素的指针向后移动一位，直到其中一个数组遍历完毕。

```typescript
function intersection(nums1: number[], nums2: number[]): number[] {
	nums1.sort((a, b) => a - b);
	nums2.sort((a, b) => a - b);

	const result: number[] = [];
	let i = 0;
	let j = 0;

	while (i < nums1.length && j < nums2.length) {
		if (nums1[i] === nums2[j]) {
			result.push(nums1[i]);
			i++;
			j++;
		} else if (nums1[i] < nums2[j]) {
			i++;
		} else {
			j++;
		}
	}

	return result;
}
```

##### 复杂度分析

+ 哈希表
	+ 时间复杂度：O(n)
	+ 空间复杂度：O(n)
+ 排序 + 双指针
	+ 时间复杂度：O(nlogn)
	+ 空间复杂度：O(1)

## 快乐数

> 题目描述：
> 
> 编写一个算法来判断一个数 n 是不是快乐数。

### 思路

#### 解法一：哈希表

遍历数字的每一位，然后将每一位的平方相加，然后判断相加的结果是否为 1，如果不为 1，则将相加的结果作为新的数字，继续重复上述步骤，直到相加的结果为 1 或者相加的结果在哈希表中出现过。

```typescript
function isHappy(n: number): boolean {
  const seen: Set<number> = new Set();

  while (n !== 1 && !seen.has(n)) {
    seen.add(n);
    n = getNext(n);
  }

  return n === 1;
}

function getNext(n: number): number {
  let totalSum = 0;
  while (n > 0) {
    let digit = n % 10;
    n = Math.floor(n / 10);
    totalSum += digit * digit;
  }
  return totalSum;
}
```

#### 解法二：快慢指针

快慢指针的思路很简单，就是让快指针每次走两步，慢指针每次走一步，如果快指针和慢指针相等，则说明存在循环，如果快指针走到了 1，则说明是快乐数，否则不是快乐数。

```typescript
function isHappy(n: number): boolean {
	let slow = n;
	let fast = getNext(n);

	while (fast !== 1 && slow !== fast) {
		slow = getNext(slow);
		fast = getNext(getNext(fast));
	}

	return fast === 1;
}
function getNext(n: number): number {
  let totalSum = 0;
  while (n > 0) {
    let digit = n % 10;
    n = Math.floor(n / 10);
    totalSum += digit * digit;
  }
  return totalSum;
}
```

##### 复杂度分析

+ 哈希表
	+ 时间复杂度：O(logn)
	+ 空间复杂度：O(logn)
+ 快慢指针
	+ 时间复杂度：O(logn)
	+ 空间复杂度：O(1)

## 两数之和

> 题目描述：
> 
> 给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

### 思路

#### 解法一：暴力解法

暴力解法的本质是：遍历数组，然后将当前元素和后面的元素相加，如果相加的结果等于目标值，则返回两个元素的下标。

```typescript
function twoSum(nums: number[], target: number): number[] {
	for (let i = 0; i < nums.length; i++) {
		for (let j = i + 1; j < nums.length; j++) {
			if (nums[i] + nums[j] === target) {
				return [i, j];
			}
		}
	}
	return [];
}
```

#### 解法二：哈希表

哈希表的思路也很简单，就是遍历数组，然后将数组中的元素作为 key，元素的下标作为 value，然后遍历数组，如果目标值减去当前元素的差值在哈希表中出现过，则返回两个元素的下标。

```typescript
function twoSum(nums: number[], target: number): number[] {
  const map: Map<number, number> = new Map();

  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (map.has(complement)) {
      return [map.get(complement)!, i];
    }
    map.set(nums[i], i);
  }

  return []; // 如果没有找到符合条件的数字对
}
```

##### 复杂度分析

+ 暴力解法
	+ 时间复杂度：O(n^2): 嵌套循环 `n * n`
	+ 空间复杂度：O(1)
+ 哈希表
	+ 时间复杂度：O(n)
	+ 空间复杂度：O(n)
