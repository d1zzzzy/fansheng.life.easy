---
title: 代码随想录算法训练营第八天| 实现 strStr、重复的子字符串
date: 2023-12-07 11:13:42
tags:
  - Algorithm
  - String
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_01.webp
---

# 代码随想录算法训练营第八天

## KMP 算法

### 核心思想

KMP 算法的核心在于，当发生不匹配时，能够利用已经匹配的部分信息，避免从头开始重新搜索。这是通过一个被称为“部分匹配表”的数据结构来实现的，它记录了模式字符串每个位置的最长相等前后缀长度。

### 原理

当在文本字符串中搜索到不匹配的字符时，KMP 算法会参考部分匹配表，将模式字符串向右滑动适当的距离。这种滑动策略避免了不必要的比较，显著提高了搜索效率。

### 部分匹配表实现

```typescript
function buildPrefixTable(s: string): number[] {
  let prefixTable: number[] = new Array(s.length).fill(0);
  let len = 0; // 最长相等前后缀的长度
  let i = 1; // 从第二个字符开始

  while (i < s.length) {
    if (s[i] === s[len]) {
      len++;
      prefixTable[i] = len;
      i++;
    } else {
      if (len !== 0) {
        len = prefixTable[len - 1];
      } else {
        prefixTable[i] = 0;
        i++;
      }
    }
  }

  return prefixTable;
}
```

## 实现 strStr

> 题目描述：
> 
> 给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1` 。

### 代码实现

```typescript
function buildPrefixTable(pattern: string): number[] {
  let prefixTable: number[] = new Array(pattern.length).fill(0);
  let len = 0; // 最长相等前后缀的长度
  let i = 1;

  while (i < pattern.length) {
    if (pattern[i] === pattern[len]) {
      // 当前字符和最长前缀的下一个字符匹配
      len++;
      prefixTable[i] = len;
      i++;
    } else {
      if (len > 0) {
        // 回退到前一个最长匹配前缀的位置
        len = prefixTable[len - 1];
      } else {
        // 没有匹配的前缀
        prefixTable[i] = 0;
        i++;
      }
    }
  }
  return prefixTable;
}


function strStr(haystack: string, needle: string): number {
	if (needle.length === 0) return 0;

    let prefixTable = buildPrefixTable(needle);
    let i = 0; // 文本字符串的索引
    let j = 0; // 模式字符串的索引

    while (i < haystack.length) {
      if (haystack[i] === needle[j]) {
        // 当前字符匹配成功
        if (j === needle.length - 1) {
            return i - j; // 找到匹配的模式字符串
        }
        i++;
        j++;
      } else {
        if (j !== 0) {
          // 根据部分匹配表回退
          j = prefixTable[j - 1];
        } else {
          // 模式字符串的第一个字符不匹配
          i++;
        }
      }
    }
  return -1; // 未找到匹配项
};
```

### 复杂度分析

+ 时间复杂度：O(m + n)

## 重复的子字符串

> 题目描述：
> 
> 给定一个非空的字符串 s ，检查是否可以通过由它的一个子串重复多次构成。

### 代码实现

```typescript
function buildPrefixTable(s: string): number[] {
  let prefixTable: number[] = new Array(s.length).fill(0);
  let len = 0; // 最长相等前后缀的长度
  let i = 1; // 从第二个字符开始

  while (i < s.length) {
    if (s[i] === s[len]) {
      len++;
      prefixTable[i] = len;
      i++;
    } else {
      if (len !== 0) {
        len = prefixTable[len - 1];
      } else {
        prefixTable[i] = 0;
        i++;
      }
    }
  }

  return prefixTable;
}

function repeatedSubstringPattern(s: string): boolean {
  let n = s.length;
  let prefixTable = buildPrefixTable(s);
  let len = prefixTable[n - 1];

  // 检查最后一个值是否非零且能被字符串长度整除
  return len > 0 && n % (n - len) === 0;
}
```

### 参考链接

[如何更好地理解和掌握 KMP 算法?](https://www.zhihu.com/question/21923021)
[KMP算法](https://zh.wikipedia.org/wiki/KMP%E7%AE%97%E6%B3%95)
