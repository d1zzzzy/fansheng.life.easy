---
title: 代码随想录算法训练营第三十天| 重新安排行程、N皇后、解数独
date: 2023-12-29 13:36:59
tags:
  - Algorithm
  - Backtrack
categories:
  - [Tech, Algorithm]
thumbnail: /fansheng.life.easy/images/default_cover_05.png
---

# 代码随想录算法训练营第三十天

## 重新安排行程

### 题目描述

> 给定一个机票的字符串二维数组 `[from, to]`，子数组中的两个成员分别表示飞机出发和降落的机场地点，对该行程进行重新规划排序。所有这些机票都属于一个从 `JFK`（肯尼迪国际机场）出发的先生，所以该行程必须从 `JFK` 出发。说明:
> 
> 1. 如果存在多种有效的行程，你可以按字符自然排序返回最小的行程组合。例如，行程 `["JFK", "LGA"]` 与 `["JFK", "LGB"]` 相比就更小，排序更靠前
> 2. 所有的机场都用三个大写字母表示（机场代码）。
> 3. 假定所有机票至少存在一种合理的行程。
> 4. 所有的机票必须都用一次 且 只能用一次。

### 思路

这个问题可以通过深度优先搜索（DFS）算法结合贪心策略来解决。算法步骤如下：

1. **构建图**：首先将所有的航班票转换成一个图的形式，图的节点表示机场，边表示航班。边是有向的，并且按字母顺序排序。
2. **深度优先搜索**：从 JFK 机场开始进行深度优先搜索。
3. **后序遍历**：当访问完一个机场的所有出边后，将该机场加入到路径中。这样可以确保在递归返回的过程中形成路径。

### 实现

```typescript
function findItinerary(tickets: string[][]): string[] {
  let map: Map<string, string[]> = new Map();
  let path: string[] = [];

  // 构建图，并对每个机场的航班列表排序
  tickets.forEach(([src, dst]) => {
    if (!map.has(src)) map.set(src, []);
    map.get(src)!.push(dst);
  });
  for (let dst of map.values()) {
    dst.sort();
  }

  function dfs(airport: string) {
    let dsts = map.get(airport);
    while (dsts && dsts.length > 0) {
      let nextDst = dsts.shift()!; // 移除并返回第一个航班
      dfs(nextDst);
    }
    path.push(airport); // 将机场加入到路径中
  }

  dfs("JFK");
  return path.reverse(); // 反转得到正确的行程顺序
}
```

## N皇后

### 题目描述

> 给定一个整数 `n`，返回所有不同的 `n` 皇后问题的解决方案。

### 思路

1. **回溯**：使用回溯法来逐行放置皇后。
2. **有效性检查**：在放置每个皇后之前检查其所在的行、列和对角线上是否已有其他皇后。
3. **记录解**：当成功放置了 N 个皇后，记录当前的棋盘布局作为一个解。

### 实现

```typescript
function solveNQueens(n: number): string[][] {
  let results: string[][] = [];
  let queens = Array(n).fill(-1);

  function isValid(row: number, col: number): boolean {
    for (let i = 0; i < row; i++) {
      if (queens[i] === col || queens[i] - i === col - row || queens[i] + i === col + row) {
        return false;
      }
    }
    return true;
  }

  function backtrack(row: number) {
    if (row === n) {
      buildSolution();
      return;
    }
    for (let col = 0; col < n; col++) {
      if (isValid(row, col)) {
        queens[row] = col;
        backtrack(row + 1);
        queens[row] = -1;
      }
    }
  }

  function buildSolution() {
    let solution: string[] = [];
    for (let i = 0; i < n; i++) {
      let row = Array(n).fill('.');
      row[queens[i]] = 'Q';
      solution.push(row.join(''));
    }
    results.push(solution);
  }

  backtrack(0);
  return results;
}
```

## 解数独

### 题目描述

> 编写一个程序，通过填充空格来解决数独问题。数独的解法需 遵循如下规则：
> 
> 1. 数字 `1-9` 在每一行只能出现一次。
> 2. 数字 `1-9` 在每一列只能出现一次。
> 3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。
> 4. 数独部分空格内已填入了数字，空白格用 `'.'` 表示。
> 5. 你可以假设给定的数独只有唯一解。
> 6. 给定数独永远是 `9x9` 形式的。

### 思路

1. **回溯法**：逐个尝试填充网格中的空白格。
2. **有效性检查**：对于每个填充的数字，检查其所在的行、列和 3x3 子网格是否有效（即没有重复的数字）。
3. **递归填充**：递归地填充下一个空白格，如果无法填充，则回溯。
4. **找到解后停止**：一旦所有的空白格都被有效地填充，即找到了一个有效解，停止递归。

### 实现

```typescript
function solveSudoku(board: string[][]): void {
  function isValid(row: number, col: number, num: string): boolean {
    for (let i = 0; i < 9; i++) {
      if (board[row][i] === num || board[i][col] === num || board[3 * Math.floor(row / 3) + Math.floor(i / 3)][3 * Math.floor(col / 3) + i % 3] === num) {
          return false;
      }
    }
    return true;
  }

  function solve(): boolean {
    for (let row = 0; row < 9; row++) {
      for (let col = 0; col < 9; col++) {
        if (board[row][col] === '.') {
          for (let num = 1; num <= 9; num++) {
            let charNum = num.toString();
            if (isValid(row, col, charNum)) {
              board[row][col] = charNum;
              if (solve()) return true;
              board[row][col] = '.';
            }
          }
          return false;
        }
      }
    }
    return true;
  }

  solve();
}
```

## 回溯总结

回溯算法通常用于解决需要遍历所有可能情况的问题，特别是那些涉及决策树的问题。这种算法在每一步都尝试所有可能的选择，并在必要时回溯到上一步以尝试不同的选择。回溯算法经常被应用于组合问题、排列问题、子集问题、分割问题等。

回溯算法的一个通用解题模板如下：

### 1. 定义解决问题的函数
首先，定义一个函数，它用于解决问题。这个函数通常接受输入数据、一个表示当前状态的变量（如路径、所选的数字等）和其他帮助控制回溯过程的变量。

```typescript
function backtrack(参数) {
  // 终止条件（可选）
  if (满足特定条件) {
    存储解或返回结果;
    return;
  }

  // 遍历所有可能的选择
  for (选择：选择列表) {
    做出选择;
    backtrack(参数);
    撤销选择;
  }
}
```

### 2. 处理特定问题的细节
在实现回溯算法时，需要根据具体问题处理终止条件、选择列表的生成、如何做出和撤销选择。

- **终止条件**：通常是递归到决策树的底部，无法再做出新的选择时。
- **选择列表**：这是当前状态下你拥有的所有选择。
- **做出选择**：将当前选择添加到路径中。
- **递归**：调用函数，进入下一层决策树。
- **撤销选择**：在回溯到上一层决策树之前，需要撤销当前的选择，以便下一次迭代或递归可以使用。

### 3. 调用函数
最后，在主函数中调用这个回溯函数，并初始化一些参数。

```typescript
function mainFunction(输入参数) {
  // 初始化变量
  let path = [];
  let results = [];

  backtrack(初始参数);
  return results;
}
```

### 注意事项
- 在实现回溯算法时，需要特别注意递归状态的维护，包括状态的更新和恢复。
- 在某些问题中，可能需要额外的数据结构来支持更快的选择和撤销选择操作。
- 对于复杂问题，有效的剪枝操作可以显著减少搜索空间，提高算法效率。
- 回溯算法可能导致高时间复杂度，特别是在决策树较大时。在实际应用中，应评估是否有更高效的替代算法。
