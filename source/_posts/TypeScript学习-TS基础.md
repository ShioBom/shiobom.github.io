---
title: TypeScript学习-TS基础
date: 2019-10-20 16:36:50
tags:
---

## TypeScript 学习笔记--TS基础

### 原始数据类型 
- boolean
- string
- number
- void：常用于函数返回值为空的情况
- undefined
- null

> undefined 和 null 是所有类型的子类型。也就是说 undefined 类型的变量，可以赋值给 number 类型的变量,而 void 则不行

### 任意值（any）
- 任意值类型，允许被赋值为任意类型。也就是任何类型的值都可以赋给它。

```typescript
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
myFavoriteNumber = true;
myFavoriteNumber = undefined;
```
- 声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值
- 变量如果在声明的时候，未指定其类型，并且==未赋值==，那么它会被识别为任意值类型，未指定类型，但赋了初值，由于类型推论会推测出一个类型，则不会被识别为任意值类型。

```typescript
let something;
something = 'seven';
something = 7;
```

> **使用场景**:用于不希望类型检查器对某些值进行检查而是直接让它们通过编译阶段的检查。以及引用第三方库的情况。

### 枚举

```typescript
enum Color：{Green,Red,Yellow}
```

我自己在这里把冒号写成了等号

### 元组

```typescript
let x:[string,number]
```

### Never 类型

用于永不存在的值的类型，never类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型

### 类型推论
TypeScript 会在==没有明确的指定类型==的时候推测出一个类型，这就是类型推论。如果被赋予不匹配的类型，编译时就会报错。

### 联合类型

表示取值可以为多种类型中的一种

```typescript
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```
当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法

### 数组的类型

- 类型+方括号：`let num:number[] = [1,2,3]`
- 泛型:`let num:Array<number> = [1,2,3]`
-  用  `any` 表示数组中允许出现任意类型

```typescript
let list: any[] = ['Xcat Liu', 25, { website: 'http://xcatliu.com' }];
```
### 函数的类型

- 函数声明
```typescript
function sum(x: number, y: number): number {
    return x + y;
}
```
- 函数表达式

```typescript
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```
> 在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型

- 可选参数：在参数后面加`?`，==可选参数后面不能出现必须参数==
- 参数默认值：TypeScript 会将添加了默认值的参数识别为可选参数，此时后面允许出现必须参数
- 剩余参数：同ES6
- 函数重载

### 类型断言

- <类型>值
- 值 `as` 类型(推荐)

[别人的读书笔记](http://www.woojean.com/2017/04/15/TypeScript%E5%AE%98%E6%96%B9%E6%96%87%E6%A1%A3/#%E4%BB%BB%E6%84%8F%E5%80%BC)
[TypeScript学习资源合集](https://juejin.im/entry/5b9e4a135188255c3a2d3695)

### ReadonlyArray<T>类型
它与Array<T>相似，只是把所有可变方法去掉了，因此可以确保数组创建后再也不能被修改

```typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
```
> `readonly` vs `const`
最简单判断该用readonly还是const的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用 const，若做为属性则使用readonly。

### 补充知识点
0x 开头：16 进制
0o 开头：8 进制
0b 开头：2 进制

