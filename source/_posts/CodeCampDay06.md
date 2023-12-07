---
title: 代码随想录算法训练营第六天| 四数相加II、赎金信、三数之和、四数之和
date: 2023-12-05 18:28:03
tags:
  - Algorithm
  - HashMap
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_00.png
---

# 代码随想录算法训练营第六天

## 四数相加II

> 题目描述：
> 
> 给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 `(i, j, k, l)` ，使得 `A[i] + B[j] + C[k] + D[l] = 0`。

### 思路

#### 解法一：暴力法

暴力法的思路很简单，就是遍历所有的组合，然后求和，然后比较大小，最后得到符合条件的组合的个数。

```typescript
function fourSumCount(A: number[], B: number[], C: number[], D: number[]): number {
	let count = 0;
	for (let i = 0; i < A.length; i++) {
		for (let j = 0; j < B.length; j++) {
			for (let k = 0; k < C.length; k++) {
				for (let l = 0; l < D.length; l++) {
					if (A[i] + B[j] + C[k] + D[l] === 0) {
						count++;
					}
				}
			}
		}
	}
	return count;
}
```

##### 复杂度分析

+ 时间复杂度：O(n^4): 嵌套循环 `n * n * n * n`
+ 空间复杂度：O(1)

#### 解法二：哈希表

哈希表的思路也很简单，就是遍历所有的组合，然后求和，然后比较大小，最后得到符合条件的组合的个数。

```typescript
function fourSumCount(A: number[], B: number[], C: number[], D: number[]): number {
  const mapAB = new Map<number, number>();
  let count = 0;

  for (let a of A) {
    for (let b of B) {
      mapAB.set(a + b, (mapAB.get(a + b) || 0) + 1);
    }
  }

  for (let c of C) {
    for (let d of D) {
      count += mapAB.get(-c - d) || 0;
    }
  }

  return count;
}
```

##### 复杂度分析

+ 时间复杂度：O(n^2)
+ 空间复杂度：O(n^2)

## 赎金信

> 题目描述：
> 
> 给定一个赎金信 (ransom) 字符串和一个杂志(magazine)字符串，判断第一个字符串 ransom 能不能由第二个字符串 magazines 里面的字符构成。如果可以构成，返回 true ；否则返回 false。

### 思路

#### 解法一：暴力解法

暴力解法的本质是：遍历 `ransomNote` 字符串，然后判断 `magazine` 字符串中是否包含 `ransomNote` 字符串中的字符，如果包含，则将 `magazine` 字符串中的字符删除，然后继续遍历 `ransomNote` 字符串，直到遍历完 `ransomNote` 字符串，如果 `magazine` 字符串中的字符全部被删除，则返回 `true`，否则返回 `false`。

```typescript
function canConstruct(ransomNote: string, magazine: string): boolean {
	const magazineArr = magazine.split('');
	for (let i = 0; i < ransomNote.length; i++) {
		const index = magazineArr.indexOf(ransomNote[i]);
		if (index > -1) {
			magazineArr.splice(index, 1);
		} else {
			return false;
		}
	}
	return true;
}
```

##### 复杂度分析

+ 时间复杂度：O(n^2)
+ 空间复杂度：O(n)

#### 解法二：哈希表

哈希表的思路也很简单，就是遍历 `ransomNote` 字符串，然后判断 `magazine` 字符串中是否包含 `ransomNote` 字符串中的字符，如果包含，则将 `magazine` 字符串中的字符删除，然后继续遍历 `ransomNote` 字符串，直到遍历完 `ransomNote` 字符串，如果 `magazine` 字符串中的字符全部被删除，则返回 `true`，否则返回 `false`。

```typescript
function canConstruct(ransomNote: string, magazine: string): boolean {
  const charCount = new Map<string, number>();

  for (const char of magazine) {
    charCount.set(char, (charCount.get(char) || 0) + 1);
  }

  for (const char of ransomNote) {
    if (!charCount.has(char) || charCount.get(char) === 0) {
      return false;
    }
    charCount.set(char, charCount.get(char)! - 1);
  }

  return true;
}

```

##### 复杂度分析

+ 时间复杂度：O(n)
+ 空间复杂度：O(n)

## 三数之和

> 题目描述：
> 
> 给你一个包含 n 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

### 思路

#### 解法一：暴力解法

暴力解法的本质是：遍历数组，然后固定一个数，然后再遍历数组，找到另外两个数，使得三个数的和为 0，然后将三个数放到一个数组中，然后将该数组排序，然后将该数组转换成字符串，然后判断该字符串是否在结果数组中，如果不在，则将该字符串放到结果数组中。

```typescript
function threeSum(nums: number[]): number[][] {
	const result: number[][] = [];
	for (let i = 0; i < nums.length; i++) {
		for (let j = i + 1; j < nums.length; j++) {
			for (let k = j + 1; k < nums.length; k++) {
				if (nums[i] + nums[j] + nums[k] === 0) {
					const arr = [nums[i], nums[j], nums[k]].sort();
					const str = arr.join('');
					if (!result.find(item => item.join('') === str)) {
						result.push(arr);
					}
				}
			}
		}
	}
	return result;
}
```

##### 复杂度分析

+ 时间复杂度：O(n^3)
+ 空间复杂度：O(n)

#### 解法二：哈希表

哈希表的思路也很简单，就是遍历数组，然后固定一个数，然后再遍历数组，找到另外两个数，使得三个数的和为 0，然后将三个数放到一个数组中，然后将该数组排序，然后将该数组转换成字符串，然后判断该字符串是否在结果数组中，如果不在，则将该字符串放到结果数组中。

```typescript
function threeSum(nums: number[]): number[][] {
  nums.sort((a, b) => a - b);
  let length = nums.length;
  let left: number = 0,
    right: number = length - 1;
  let resArr: number[][] = [];
  for (let i = 0; i < length; i++) {
    if (nums[i]>0) {
      return resArr; //nums经过排序后，只要nums[i]>0, 此后的nums[i] + nums[left] + nums[right]均大于0,可以提前终止循环。	
    }
    if (i > 0 && nums[i] === nums[i - 1]) {
      continue;
    }
    left = i + 1;
    right = length - 1;
    while (left < right) {
      let total: number = nums[i] + nums[left] + nums[right];
      if (total === 0) {
        resArr.push([nums[i], nums[left], nums[right]]);
        left++;
        right--;
        while (nums[right] === nums[right + 1]) {
          right--;
        }
        while (nums[left] === nums[left - 1]) {
          left++;
        }
      } else if (total < 0) {
        left++;
      } else {
        right--;
      }
    }
  }
  return resArr;
};

```

##### 复杂度分析

+ 时间复杂度：O(n^2)
+ 空间复杂度：O(n)

## 四数之和

> 题目描述：
> 
> 给定一个包含 n 个整数的数组 `nums` 和一个目标值 `target`，判断 `nums` 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 `target` 相等？找出所有满足条件且不重复的四元组。

### 思路

#### 解法一：暴力解法

暴力解法的本质是：遍历数组，然后固定一个数，然后再遍历数组，找到另外三个数，使得四个数的和为 `target`，然后将四个数放到一个数组中，然后将该数组排序，然后将该数组转换成字符串，然后判断该字符串是否在结果数组中，如果不在，则将该字符串放到结果数组中。

```typescript
function fourSum(nums: number[], target: number): number[][] {
	const result: number[][] = [];
	for (let i = 0; i < nums.length; i++) {
		for (let j = i + 1; j < nums.length; j++) {
			for (let k = j + 1; k < nums.length; k++) {
				for (let l = k + 1; l < nums.length; l++) {
					if (nums[i] + nums[j] + nums[k] + nums[l] === target) {
						const arr = [nums[i], nums[j], nums[k], nums[l]].sort();
						const str = arr.join('');
						if (!result.find(item => item.join('') === str)) {
							result.push(arr);
						}
					}
				}
			}
		}
	}
	return result;
}
```

##### 复杂度分析

+ 时间复杂度：O(n^4)
+ 空间复杂度：O(n)

#### 解法二：哈希表

哈希表的思路也很简单，就是遍历数组，然后固定一个数，然后再遍历数组，找到另外三个数，使得四个数的和为 `target`，然后将四个数放到一个数组中，然后将该数组排序，然后将该数组转换成字符串，然后判断该字符串是否在结果数组中，如果不在，则将该字符串放到结果数组中。

```typescript
function fourSum(nums: number[], target: number): number[][] {
  const result: number[][] = [];
  nums.sort((a, b) => a - b);

  for (let i = 0; i < nums.length - 3; i++) {
    if (i > 0 && nums[i] === nums[i - 1]) continue; // 跳过重复值

    for (let j = i + 1; j < nums.length - 2; j++) {
      if (j > i + 1 && nums[j] === nums[j - 1]) continue; // 跳过重复值

      let left = j + 1, right = nums.length - 1;
      while (left < right) {
        const sum = nums[i] + nums[j] + nums[left] + nums[right];
        if (sum === target) {
          result.push([nums[i], nums[j], nums[left], nums[right]]);
          while (left < right && nums[left] === nums[left + 1]) left++; // 跳过重复值
          while (left < right && nums[right] === nums[right - 1]) right--; // 跳过重复值
          left++;
          right--;
        } else if (sum < target) {
          left++;
        } else {
          right--;
        }
      }
    }
  }

  return result;
}

```

##### 复杂度分析

+ 时间复杂度：O(n^3)
+ 空间复杂度：O(n)

