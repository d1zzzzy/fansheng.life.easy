---
title: 代码随想录算法训练营第七天| 反转字符串、反转字符串II、替换数字、翻转字符串里的单词、右旋转字符串
date: 2023-12-06 13:50:05
tags:
  - Algorithm
  - String
categories:
  - [Tech, Algorithm]
---

# 代码随想录算法训练营第七天

## 反转字符串

> 题目描述：
> 
> 编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `char[]` 的形式给出。

### 思路

#### 解法一：双指针

双指针的思路很简单，就是将左右指针指向的字符进行交换，然后左指针向右移动，右指针向左移动，直到左右指针相遇。

```typescript
function reverseString(s: string[]): void {
	let left = 0;
	let right = s.length - 1;

	while (left < right) {
		[s[left], s[right]] = [s[right], s[left]];
		left++;
		right--;
	}
}
```

##### 复杂度分析

+ 时间复杂度：O(n): 双指针遍历一次
+ 空间复杂度：O(1)

#### 解法二：递归

递归的思路也很简单，就是将字符串的第一个字符和最后一个字符进行交换，然后将剩余的字符串进行递归，直到字符串长度为 0 或 1。

```typescript
function reverseString(s: string[]): void {
	function helper(left: number, right: number): void {
		if (left >= right) {
			return;
		}
		[s[left], s[right]] = [s[right], s[left]];
		helper(left + 1, right - 1);
	}
	helper(0, s.length - 1);
}
```

##### 复杂度分析

+ 时间复杂度：O(n): 递归遍历一次
+ 空间复杂度：O(n): 递归调用栈的深度

## 反转字符串II

> 题目描述：
> 
> 给定一个字符串 `s` 和一个整数 `k`，从字符串开头算起，每 `2k` 个字符反转前 `k` 个字符。

### 思路

#### 解法一：双指针

双指针的思路很简单，就是将左右指针指向的字符进行交换，然后左指针向右移动 `2k` 位，右指针向左移动 `k` 位，直到左指针大于等于字符串的长度。

```typescript
function reverseStr(s: string, k: number): string {
	const arr = s.split('');
	let left = 0;
	let right = k - 1;

	while (left < arr.length) {
		reverse(arr, left, right);
		left += 2 * k;
		right += 2 * k;
	}

	return arr.join('');
}

function reverse(arr: string[], left: number, right: number): void {
	while (left < right) {
		[arr[left], arr[right]] = [arr[right], arr[left]];
		left++;
		right--;
	}
}
```

如果结合 Javascript 的字符串 api,实现起来就更简单了。

```typescript
function reverseStr(s: string, k: number) {
    let result = '';

    for (let start = 0; start < s.length; start += 2 * k) {
        let end = Math.min(start + k, s.length);
        let sub = s.substring(start, end);
        result += sub.split('').reverse().join('') + s.substring(end, start + 2 * k);
    }

    return result;
}
```


##### 复杂度分析

+ 时间复杂度：O(n): 双指针遍历一次
+ 空间复杂度：O(n): 字符串 api 会产生新的字符串

#### 解法二：递归

递归的思路也很简单，就是将字符串的前 `k` 个字符进行反转，然后将剩余的字符串进行递归，直到字符串长度为 0 或 1。

```typescript
function reverseStr(s: string, k: number): string {
	const arr = s.split('');
	let left = 0;
	let right = k - 1;

	while (left < arr.length) {
		reverse(arr, left, right);
		left += 2 * k;
		right += 2 * k;
	}

	return arr.join('');
}

function reverse(arr: string[], left: number, right: number): void {
  while (left < right) {
    [arr[left], arr[right]] = [arr[right], arr[left]];
    left++;
    right--;
  }
}
```

##### 复杂度分析

+ 时间复杂度：O(n): 递归遍历一次
+ 空间复杂度：O(n): 递归调用栈的深度

## 替换数字

> 题目描述：
> 
> 给你一个字符串 `s`，请你返回将 `s` 中的数字（`0`-`9`）替换成 'number' 字符串。

### 思路

#### 解法一：哈希表

哈希表的思路很简单，就是遍历字符串，然后将字符串中的数字作为 key，对应的字符串作为 value，然后将字符串中的数字替换成对应的字符串。

```typescript

// 给你一个字符串 `s`，请你返回将 `s` 中的数字（`0`-`9`）替换成 'number' 字符串。
function replaceDigits(s: string): string {
	const map: { [key: string]: number } = {
		'0': 1,
		'1': 1,
		'2': 1,
		'3': 1,
		'4': 1,
		'5': 1,
		'6': 1,
		'7': 1,
		'8': 1,
		'9': 1,
	};

	let result = '';

	for (let i = 0; i < s.length; i++) {
		if (map[s[i]]) {
			result += 'number';
		} else {
			result += s[i];
		}
	}

	return result;
}
```

##### 复杂度分析

+ 时间复杂度：O(n): 哈希表遍历一次
+ 空间复杂度：O(1): 哈希表的大小是固定的

#### 解法二：字符串 api

字符串 api 的思路也很简单，就是遍历字符串，然后将字符串中的数字替换成对应的字符串。

```typescript
function replaceDigits(s: string): string {
	let result = '';

	for (let i = 0; i < s.length; i++) {
		if (s[i].charCodeAt(0) >= 48 && s[i].charCodeAt(0) <= 57) {
			result += 'number';
		} else {
			result += s[i];
		}
	}

	return result;
}
```

##### 复杂度分析

+ 时间复杂度：O(n)
+ 空间复杂度：O(1)

#### 解法三：正则

直接使用 `repalceAll` api 进行替换。

```typescript
function replaceDigits(s: string): string {
	return s.replaceAll(/\d+/g, 'number');
}
```

##### 复杂度分析

+ 时间复杂度：O(n)
+ 空间复杂度：O(1)

## 翻转字符串里的单词

> 题目描述：
> 
> 给定一个字符串，逐个翻转字符串中的每个单词。

### 思路

#### 解法一：双指针

双指针的思路很简单，就是将字符串中的每个单词进行反转，然后将整个字符串进行反转。

```typescript
function reverse(str: string[], left: number, right: number): void {
  while (left < right) {
    let temp = str[left];
    str[left++] = str[right];
    str[right--] = temp;
  }
}

function reverseWords(s: string): string {
  let arr = Array.from(s);
  let n = arr.length;

  // 反转整个字符串
  reverse(arr, 0, n - 1);

  // 反转每个单词
  let start = 0;
  for (let end = 0; end < n; end++) {
    if (arr[end] === ' ') {
      reverse(arr, start, end - 1);
      start = end + 1;
    }
  }
  reverse(arr, start, n - 1); // 反转最后一个单词

  // 清理空格
  let left = 0, right = 0;
  while (right < n) {
    // 跳过单词前的空格
    while (right < n && arr[right] === ' ') right++;
    // 移动单词
    while (right < n && arr[right] !== ' ') arr[left++] = arr[right++];
    // 避免添加多余的空格
    while (right < n && arr[right] === ' ') right++;
    // 添加单词间的单个空格
    if (right < n) arr[left++] = ' ';
  }

  return arr.slice(0, left).join('');
}
```

##### 复杂度分析

+ 时间复杂度：O(n): 双指针遍历一次
+ 空间复杂度：O(n)

#### 解法二：字符串 api

字符串 api 的思路也很简单，就是将字符串中的每个单词进行反转，然后将整个字符串进行反转。

```typescript
function reverseWords(s: string): string {
  return s.trim().replaceAll(/\s+/g, ' ').split(' ').reverse().join(' ');
}
```

##### 复杂度分析

+ 时间复杂度：O(n)
+ 空间复杂度：O(n)

## 右旋转字符串

> 题目描述：
> 
> 字符串的右旋转操作是把字符串尾部的若干个字符转移到字符串的前面。给定一个字符串 s 和一个正整数 k，请编写一个函数，将字符串中的后面 k 个字符移到字符串的前面，实现字符串的右旋转操作。

### 思路

#### 解法一：字符串 api

字符串 api 的思路很简单，就是将字符串中的后面 `k` 个字符移到字符串的前面。

```typescript
function reverseLeftWords(s: string, n: number): string {
  return s.substring(s.length - n, s.length) + s.substring(0, s.length - n);
}
```

##### 复杂度分析

+ 时间复杂度：O(n)
+ 空间复杂度：O(n)

#### 解法二：双指针

1. 标准化 `k` 值：由于 `k` 可能大于字符串长度，所以首先对 `k` 进行取模操作。
2. 反转整个字符串：反转整个字符串，这将把要移动到前面的字符移到正确的位置，但这些字符和剩余字符的内部顺序会反转。
3. 反转前 `k` 个字符：再反转前 `k` 个字符，使它们恢复原来的顺序。
4. 反转剩余字符：最后，反转剩下的字符，使它们恢复原来的顺序。

```typescript
function reverse(arr: string[], start: number, end: number): void {
  while (start < end) {
    let temp = arr[start];
    arr[start++] = arr[end];
    arr[end--] = temp;
  }
}

function rightRotateString(s: string, k: number): string {
  let arr = Array.from(s);
  let n = arr.length;
  k %= n; // 防止 k 大于字符串长度

  // 反转整个字符串
  reverse(arr, 0, n - 1);
  // 反转前 k 个字符
  reverse(arr, 0, k - 1);
  // 反转剩下的字符
  reverse(arr, k, n - 1);

  return arr.join('');
}
```

##### 复杂度分析

+ 时间复杂度：O(n): 双指针遍历一次
+ 空间复杂度：O(n)
