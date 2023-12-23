---
title: CodeCampDay25
date: 2023-12-24 06:46:00
tags:
  - Algorithm
  - Backtrack
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_04.jpg
---

# 代码随想录算法训练营第二十五天| 组合总和 III、电话号码的字母组合

## 组合总和 III

### 题目描述

> 找出所有相加之和为 n 的 k 个数的组合。组合中只允许含有 1 - 9 的正整数，并且每种组合中不存在重复的数字。

### 思路

1. **递归遍历**：从数字 1 开始，递归地添加数字到当前组合中。
2. **约束条件**：确保当前组合中数字的数量不超过 k，并且它们的和不超过 n。
3. **剪枝**：如果当前组合的和已经超过 n，或者组合中的数字数量超过 k，就停止当前路径的探索。
4. **记录有效组合**：一旦找到一个有效的组合（即组合中数字的和为 n，且包含 k 个数字），就将其添加到结果列表中。
5. **回溯**：在探索完一个数字后，回溯以去掉这个数字，尝试下一个数字。

```typescript
function combinationSum3(k: number, n: number): number[][] {
	let results: number[][] = [];
	let path: number[] = [];

	function backtrack(start: number, sum: number) {
		if (sum > n || path.length > k) return;
		if (sum === n && path.length === k) {
			results.push([...path]);
			return;
		}

		for (let i = start; i <= 9; i++) {
			path.push(i);
			backtrack(i + 1, sum + i);
			path.pop();
		}
	}

	backtrack(1, 0);
	return results;
}
```

## 电话号码的字母组合

### 题目描述

> 给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

### 思路

1. **映射构建**：首先构建一个电话号码到字母的映射表。
2. **递归遍历**：从左到右遍历输入的数字字符串，对于每一个数字，尝试映射表中的所有可能字母，并递归地进行下一数字的选择。
3. **记录有效组合**：一旦完成所有数字的处理，记录当前的字母组合。
4. **回溯**：在尝试完一个字母后，回溯以尝试下一个字母。

```typescript
function letterCombinations(digits: string): string[] {
	if (digits.length === 0) return [];

	const phoneMap: { [key: string]: string } = {
		'2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
		'6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
	};

	let results: string[] = [];
	let combination: string[] = [];

	function backtrack(index: number) {
		if (index === digits.length) {
			results.push(combination.join(''));
			return;
		}

		let letters = phoneMap[digits[index]];
		for (let letter of letters) {
			combination.push(letter);
			backtrack(index + 1);
			combination.pop();
		}
	}

	backtrack(0);
	return results;
}
```
