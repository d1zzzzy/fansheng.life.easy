---
title: 代码随想录算法训练营第十一天| 滑动窗口最大值、前 K 个高频元素
date: 2023-12-11 11:41:53
tags:
  - Algorithm
  - Stack
  - Queue
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_00.jpg
---

# 代码随想录算法训练营第十一天

## 滑动窗口最大值

> 题目描述：
> 
> 给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

### 思路

使用双端队列来解决，队列中存储的是数组的下标，队列中的元素从大到小排列，队首元素是滑动窗口中的最大值。

### 代码实现

#### 常见解法

```typescript
function maxSlidingWindow(nums: number[], k: number): number[] {
	let res: number[] = [];
	let deque: number[] = [];

	for (let i = 0; i < nums.length; i++) {
		// 如果队首元素已经不在滑动窗口中了，就把队首元素弹出
		if (deque.length > 0 && deque[0] === i - k) {
			deque.shift();
		}

		// 如果队列中有元素，且队尾元素小于当前元素，则弹出队尾元素
		while (deque.length > 0 && nums[deque[deque.length - 1]] < nums[i]) {
			deque.pop();
		}

		// 把当前元素的下标加入队列
		deque.push(i);

		// 如果已经形成了滑动窗口，就把队首元素加入结果数组
		if (i >= k - 1) {
			res.push(nums[deque[0]]);
		}
	}

	return res;
}
```

#### 优化解法

在上面的基础上，我们讲将滑动窗口维护成一个递减的队列，这样队首元素就是滑动窗口中的最大值。

然后当 `i = 0` 时, 我们先把前 `k` 个元素推进队列，获取第一个最大值。后续从 `i = 1` 开始遍历即可。 

```typescript
class MonotonicQueue {
    private queue: number[] = [];

    // 向队列中添加元素，并保持递减顺序
    enqueue(value: number) {
        while (this.queue.length > 0 && this.queue[this.queue.length - 1] < value) {
            this.queue.pop();
        }
        this.queue.push(value);
    }

    // 返回队列头部的元素（当前最大值）
    max(): number {
        return this.queue[0];
    }

    // 如果队列头部的元素是旧的最大值，则将其移除
    dequeue(value: number) {
        if (this.queue.length > 0 && this.queue[0] === value) {
            this.queue.shift();
        }
    }
}

function maxSlidingWindow(nums: number[], k: number): number[] {
    let result: number[] = [];
    let window = new MonotonicQueue();

    for (let i = 0; i < k; i++) {
        window.enqueue(nums[i]);
    }

    result.push(window.max());

    for (let i = k; i < nums.length; i++) {
        // 移除窗口最左侧的元素
        window.dequeue(nums[i - k]);
        window.enqueue(nums[i]);
        // 当窗口滑动到大小为 k 时，记录最大值
        result.push(window.max());
    }

    return result;
}
```

#### 复杂度分析

+ 时间复杂度：O(n)，其中 n 是数组 `nums` 的长度。每一个下标恰好被放入队列一次，删除队列中的元素不超过一次。因此操作总时间复杂度为 O(n)。
+ 空间复杂度：O(k)，即为队列的空间代价。

## 前 K 个高频元素

> 题目描述：
> 
> 给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

### 思路

#### 使用「哈希表」和「最小堆」

1. 使用哈希表统计每个元素出现的频率。
2. 使用一个大小为 k 的最小堆来存储最高频的元素。堆中存储的是元素及其频率。
3. 遍历哈希表，对于每个元素： 
   + 如果堆的大小小于 k，直接添加元素到堆中。
   + 如果当前元素的频率大于堆顶元素的频率，则弹出堆顶元素，将当前元素加入堆中。
4. 最终，堆中的元素即为前 k 个高频元素。

```typescript
class MinHeap {
    private heap: [number, number][]; // 存储 [元素, 频率] 对

    constructor() {
        this.heap = [];
    }

    push(item: [number, number]) {
        this.heap.push(item);
        this.heapifyUp();
    }

    pop(): [number, number] | undefined {
        const n = this.heap.length;
        if (n === 0) return undefined;
        
        const top = this.heap[0];
        const bottom = this.heap.pop()!;
        if (n > 1) {
            this.heap[0] = bottom;
            this.heapifyDown();
        }
        return top;
    }

    getSize(): number {
        return this.heap.length;
    }

    private heapifyUp() {
        let index = this.heap.length - 1;
        while (index > 0 && this.heap[index][1] < this.heap[this.parentIndex(index)][1]) {
            this.swap(index, this.parentIndex(index));
            index = this.parentIndex(index);
        }
    }

    private heapifyDown() {
        let index = 0;
        while (this.leftChildIndex(index) < this.heap.length) {
            let smallerChildIndex = this.leftChildIndex(index);
            if (this.rightChildIndex(index) < this.heap.length &&
                this.heap[this.rightChildIndex(index)][1] < this.heap[smallerChildIndex][1]) {
                smallerChildIndex = this.rightChildIndex(index);
            }
            if (this.heap[index][1] <= this.heap[smallerChildIndex][1]) break;

            this.swap(index, smallerChildIndex);
            index = smallerChildIndex;
        }
    }

    private parentIndex(index: number): number {
        return Math.floor((index - 1) / 2);
    }

    private leftChildIndex(index: number): number {
        return 2 * index + 1;
    }

    private rightChildIndex(index: number): number {
        return 2 * index + 2;
    }

    private swap(i: number, j: number) {
        [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
    }
}

function topKFrequent(nums: number[], k: number): number[] {
    const frequencyMap = new Map<number, number>();
    nums.forEach(num => frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1));

    const minHeap = new MinHeap();
    frequencyMap.forEach((freq, num) => {
        minHeap.push([num, freq]);
        if (minHeap.getSize() > k) {
            minHeap.pop();
        }
    });

    const topK = [];
    while (minHeap.getSize() > 0) {
        topK.push(minHeap.pop()![0]);
    }

    return topK;
}
```

#### 使用队列

1. 使用哈希表统计每个元素的频率。
2. 创建一个优先队列（最小堆），用于存储元素及其频率。队列大小保持在 k。
3. 遍历哈希表，对于每个元素： 
   + 如果队列未满，直接加入队列。 
   + 如果队列已满，比较当前元素的频率与队列中最小频率元素（队列顶）。如果当前元素频率更高，替换队列顶元素，并重新调整队列。
4. 最终，队列中的元素即为前 k 个高频元素。

```typescript
function topKFrequent(nums: number[], k: number): number[] {
    // 使用哈希表统计频率
    const frequencyMap = new Map<number, number>();
    nums.forEach(num => {
        frequencyMap.set(num, (frequencyMap.get(num) || 0) + 1);
    });

    // 创建优先队列（最小堆）
    const minHeap: [number, number][] = [];
    const compare = (a: [number, number], b: [number, number]) => a[1] - b[1]; // 比较函数

    frequencyMap.forEach((freq, num) => {
        if (minHeap.length < k) {
            minHeap.push([num, freq]);
            minHeap.sort(compare);
        } else if (freq > minHeap[0][1]) {
            minHeap.shift();
            minHeap.push([num, freq]);
            minHeap.sort(compare);
        }
    });

    // 提取前 K 个高频元素
    return minHeap.map(([num, _]) => num);
}
```

##### 复杂度分析

+ 时间复杂度：O(nlogk)，其中 n 是数组 `nums` 的长度。遍历数组并统计频率的时间复杂度是 O(n)，插入和删除元素的时间复杂度是 O(logk)，因此总时间复杂度是 O(nlogk)。
+ 空间复杂度：O(n)，其中 n 是数组 `nums` 的长度。需要用哈希表存储数组中每个元素的频率，哈希表的大小不会超过数组的长度。最坏情况下数组中没有重复元素，需要存储 n 个键值对。

## 总结

### 栈的常见算法问题

1. **有效的括号**：
	- 问题：判断给定的括号字符串是否有效。
	- 解法：使用栈来匹配括号。遇到左括号就压栈，遇到右括号时检查栈顶元素是否匹配，若匹配则出栈。

2. **逆波兰表达式求值**：
	- 问题：计算后缀表达式（逆波兰表达式）的值。
	- 解法：遍历表达式，使用栈存储操作数。遇到操作符时，从栈中弹出操作数进行计算，然后将结果压回栈中。

3. **最小栈**：
	- 问题：设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。
	- 解法：使用两个栈，一个存储所有元素，另一个仅在新元素为当前最小值时才入栈。

4. **栈的日常温度**：
	- 问题：根据每日气温列表，计算出温度升高的天数。
	- 解法：使用栈来存储温度列表的索引，遇到更高的温度时计算索引差。

### 队列的常见算法问题

1. **队列的最大值**：
	- 问题：设计一个队列，能在常数时间内返回最大值。
	- 解法：使用一个双端队列（Deque）来维护可能的最大值。

2. **循环队列**：
	- 问题：设计你的循环队列实现。
	- 解法：使用数组模拟队列，维护头尾指针。

3. **滑动窗口最大值**：
	- 问题：给定一个数组和滑动窗口的大小，找出所有滑动窗口里的最大值。
	- 解法：使用双端队列存储可能的最大值的索引。

4. **墙与门**（广度优先搜索）：
	- 问题：在一个 2D 网格中，找出从每个空房间到最近的门的距离。
	- 解法：使用队列进行广度优先搜索。

### 结论

栈和队列是解决这类问题的理想选择，因为它们能有效地管理元素的顺序。栈适用于需要后进先出逻辑的问题，如字符串解析和递归模拟。队列适用于需要先进先出逻辑的问题，如按顺序处理任务和广度优先搜索。
