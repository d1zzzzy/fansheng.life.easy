---
title: Typescript 中的类型(一)
date: 2023-12-12 10:42:37
tags:
  - Typescript
categories:
  - [Tech, Typescript]
thumbnail: /fansheng.life.easy/images/default_cover_03.png
---

# Typescript 中的类型

## 基础类型

### 布尔值、数字、字符串

+ 布尔值：是逻辑实体，用于表示真（`true`）或假（`false`）。只能被 `true` 和 `false` 赋值，对于 javascript 中除了二者之外的真值和假值都无法为该类型赋值。

+ 数字：Typescript 中所有的数字都是浮点数。

+ 字符串：表示文本数据，可以用双引号（"）、单引号（'）或者模板字符串（使用反引号 ``）表示。

### 数组

+ 定义数组的方式有两种：

	+ 在元素类型后面接上 `[]`，表示由此类型元素组成的一个数组。

	+ 使用数组泛型，`Array<元素类型>`。

> 需要注意的是单独的 `[]` 表示空数组，而不是 `any[]`。

### 元组

表示一个已知元素数量和类型的数组，意味着它是强类型的，每个位置的元素类型在声明时就已经确定。

+ 访问一个已知索引的元素，会得到正确的类型
+ 访问一个越界的元素，会使用联合类型替代

#### 元组与数组的区别
尽管元组看起来像数组，但它们在功能上有所不同：

+ 元组：长度固定，类型已知且可能不同。
+ 数组：长度可变，类型单一。

```typescript
// 元组
let x: [string, number];
x = ['hello', 10]; // OK

// 数组
let y: string[];
y = ['hello', 10];
```

#### 高级用法

##### 可选元素、剩余元素

```typescript
// 可选元素
let tupleWithOptional: [number, string?, boolean?];
tupleWithOptional = [1];  // 正确
tupleWithOptional = [1, "Hello", true];  // 正确

// 剩余元素
let tupleWithRest: [number, ...string[]];
tupleWithRest = [1, "Hello", "World"];  // 正确
```

> 元祖长度固定与通过剩余元素可以得到任意数量的元素冲突吗?
> 
> 不冲突。
> + 基本形式下，元组的长度是固定的，每个位置的元素类型都是预先定义的
> + 使用剩余元素时，元组的前几个元素类型和数量仍然是固定的，但在元组的末尾，你可以有任意数量的额外元素，这些额外元素的类型也是固定的。
>
> 剩余元素的引入并不矛盾，而是提供了一种在保持类型安全的前提下增加元组灵活性的方法。通过这种方式，TypeScript 允许元组在保持其核心特性（即预定义的固定类型位置）的同时，对特定位置之后的元素数量有所放宽。这种设计既保持了元组的基本特征，又增加了其应用的灵活性和广泛性。

### 枚举

最基本的枚举类型是数值枚举，其中成员的值默认从 0 开始自增。

```typescript
enum Color {
		Red,
		Green,
		Blue,
}

Color.Red; // 0
Color.Green; // 1
```

#### 字符串枚举

每个成员都必须使用字符串字面量或另一个字符串枚举成员进行初始化。

```typescript
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT"
}
```
#### 常量枚举

使用 const 关键字可以定义常量枚举。常量枚举在编译阶段会被完全移除，它们的成员会在使用的地方被内联。

```typescript
const enum Direction {
  Up,
  Down,
  Left,
  Right
}

let directions = [Direction.Up, Direction.Down];
```

在生成的 JavaScript 中，`Direction.Up` 和 `Direction.Down` 会被替换为实际的数值，提高了运行时的性能。

#### 枚举的反向映射

数值枚举具有一个特性，即可以从枚举值到枚举名的反向映射。

```typescript
enum Direction {
  Up,
  Down,
  Left,
  Right
}

// Direction[0] 将返回字符串 "Up"
let dirName: string = Direction[0]; // "Up"
```

### any | never | void

+ **any**: 表示任意类型，当你不知道数据类型时，可以使用该类型。
+ **never**: 表示永不存在的值的类型，一般用于抛出异常或无法执行到终点的情况。
+ **void**: 通常用于没有返回值的函数。它表示没有任何类型，但与 `any` 不同，`void` 类型的变量不能随意赋值。

#### any 的影响

+ **类型检查绕过**： 任何赋给 `any` 类型的变量的值都不会进行类型检查。这意味着，即使代码中存在类型错误，编译时也不会报错。

+ **安全隐患**： 使用 any 类型可能会隐藏潜在的类型相关错误，这可能导致运行时错误。

#### never

never类型是任何类型的子类型，也可以赋值给任何类型；然而，没有类型是never的子类型或可以赋值给never类型（除了never本身之外）。 即使 any也不可以赋值给never。

### null | undefined

默认情况下null和undefined是所有类型的子类型。当你指定了--strictNullChecks标记，null和undefined只能赋值给void和它们各自。 

### Object

object表示非原始类型，也就是除 `number`，`string`，`boolean`，`symbol`，`null` 或 `undefined` 之外的类型。

## 接口

接口定义了对象应该有哪些属性和方法，以及这些属性和方法的类型。主要用于类型检查，并且是定义对象类型、函数签名、类结构和复合类型的关键部分。

### 可选属性

接口中的属性和方法可以是可选的，这意味着实现接口的对象可以选择是否实现这些属性和方法。

### 只读属性

可以在属性名前用 `readonly` 来指定只读属性。

```typescript
interface Point {
  readonly x: number;
  readonly y: number;
}
```

> 函数参数中使用 `readonly` 的场景
> 
> + 保护数组/对象属性不被修改
> readonly 关键字用于防止对数组或对象的成员进行修改，这有助于实现不可变性和函数编程中的一些原则。

### 扩展接口

接口可以扩展一个或多个其他接口，这使得你可以从其他接口创建新的接口。

```typescript
interface Named {
  name: string;
}

interface Greetable extends Named {
  greet(phrase: string): void;
}
```
