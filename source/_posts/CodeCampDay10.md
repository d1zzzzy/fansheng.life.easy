---
title: 代码随想录算法训练营第十天| 有效的括号、删除字符串中的所有相邻重复项、逆波兰表达式求值
date: 2023-12-09 20:34:36
tags:
  - Algorithm
  - Stack
  - Queue
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_05.png
---

# 代码随想录算法训练营第十天

## 有效的括号

> 题目描述：
> 
> 给定一个只包括` '('，')'，'{'，'}'，'['，']'`的字符串`s`，判断字符串是否有效。

### 思路

使用栈来解决，遇到左括号就入栈，遇到右括号就出栈，最后判断栈是否为空。

### 代码实现

```typescript
function isValid(s: string): boolean {
	// 使用数组模拟栈结构
	let stack: string[] = [];
	
	// 用一个对象来映射每种右括号对应的左括号
	let mapping: { [key: string]: string } = {
		')': '(',
		'}': '{',
		']': '['
	};

	// 遍历字符串中的每个字符
	for (let char of s) {
		// 如果当前字符是右括号
		if (mapping[char]) {
			// 弹出栈顶元素，如果栈为空，则给 topElement 赋一个非括号的值
			let topElement = stack.length === 0 ? '#' : stack.pop();

			// 如果栈顶元素不是与当前右括号匹配的左括号，则字符串无效
			if (topElement !== mapping[char]) {
				return false;
			}
		} else {
			// 如果是左括号，压入栈中
			stack.push(char);
		}
	}

	// 如果栈为空，说明所有括号都正确匹配，字符串有效
	return stack.length === 0;
}
```

### 复杂度分析

+ 时间复杂度：O(n)，其中 n 是字符串`s`的长度。
+ 空间复杂度：O(n + ∣Σ∣)，其中 Σ 表示字符集，本题中字符串只包含 6 种括号，∣Σ∣=6。栈中的字符数量为 O(n)，而哈希表使用的空间为 O(∣Σ∣)，相加即可得到总空间复杂度。

## 删除字符串中的所有相邻重复项

> 题目描述：
> 
> 给出由小写字母组成的字符串`S`，重复项删除操作会选择两个相邻且相同的字母，并删除它们。

### 思路

使用栈来解决，遍历字符串中的每个字符，如果当前字符与栈顶元素相同，则弹出栈顶元素，否则将当前字符压入栈中。

### 代码实现

```typescript
function removeDuplicates(s: string): string {
	let stack: string[] = [];

	// 遍历字符串中的每个字符
	for (let char of s) {
		// 如果栈不为空且当前字符与栈顶字符相同
		if (stack.length > 0 && stack[stack.length - 1] === char) {
			// 弹出栈顶字符（消除一对相邻重复项）
			stack.pop();
		} else {
			// 如果不相同，将当前字符压入栈中
			stack.push(char);
		}
	}

	// 将栈中的字符组合成字符串作为结果返回
	return stack.join('');
}
```

### 复杂度分析

+ 时间复杂度：O(n)，其中 n 是字符串`s`的长度。
+ 空间复杂度：O(n)。在最坏情况下，栈中的所有字符都不相同。

## 逆波兰表达式求值

> 题目描述：
> 
> 根据[ 逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437)，求表达式的值。

### 思路

使用栈来解决，遍历数组中的每个元素，如果当前元素是运算符，则从栈中弹出两个元素，进行运算，将结果压入栈中，否则将当前元素压入栈中。

### 代码实现

```typescript
function evalRPN(tokens: string[]): number {
	let stack: number[] = [];

	for (let token of tokens) {
		if (!isNaN(parseInt(token))) {
			// 如果是操作数，转换为数字后压入栈
			stack.push(parseInt(token));
		} else {
			// 遇到操作符，从栈中弹出两个操作数
			let a = stack.pop()!;
			let b = stack.pop()!;
			switch (token) {
				case '+':
					stack.push(b + a);
					break;
				case '-':
					stack.push(b - a);
					break;
				case '*':
					stack.push(b * a);
					break;
				case '/':
					stack.push(Math.trunc(b / a)); // 注意整数除法
					break;
			}
		}
	}

	// 栈顶元素即为计算结果
	return stack.pop()!;
}
```

### 复杂度分析

+ 时间复杂度：O(n)，其中 n 是数组`tokens`的长度。
+ 空间复杂度：O(n)。在最坏情况下，栈中的所有元素都是操作数。
