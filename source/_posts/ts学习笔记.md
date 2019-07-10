---
title: ts学习笔记
date: 2019-06-26 10:06:42
tags:
---

## 数据类型

1. 类型推论。

   ```javascript
   let myFavoriteNumber = "seven";
   ```

   等价于

   ```javascript
   let myFavoriteNumber: string = "seven";
   ```

   ts 会在没有明确的指定类型的时候推测出一个类型。但是，**如果定义的时候没有赋值，不管之后 有没有赋值，都会被推断成 any 类型而完全不被类型检查**。

   ```javascript
   let myFavoriteNumber;
   myFavoriteNumber = "seven";
   ```

   等价于

   ```javascript
   let myFavoriteNumber: any;
   myFavoriteNumber = "seven";
   ```

2. 接口（interface）

   - 自定义属性

     ```typescript
     interface Person {
       name: string;
       age?: number;
       [x: string]: string;
     }

     let tom: Person = {
       name: "Tom",
       age: 25,
       gender: "male"
     };
     ```

     **一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集**，上面代码中定义了任意属性，而 age 的值却是 number，number 不是 string 的子属性，所以编译会报错
