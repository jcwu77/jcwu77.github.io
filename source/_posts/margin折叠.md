---
title: margin折叠
date: 2019-07-12 10:48:33
tags:
---

## 什么是 margin 折叠

在 CSS 中，两个或多个毗邻的普通流中的盒子（可能是父子元素，也可能是兄弟元素）在垂直方向上的外边距会发生叠加，这种形成的外边距称之为外边距叠加。

- 父子间：
  ![marginPc](/../image/marginPc.png "margin折叠（父子元素）图")

- 兄弟间：
  ![marginB](/../image/marginB.png "margin折叠（兄弟元素）图")、

---

## 什么时候发生 margin 折叠

- 没有被 padding、border、clear 和 line box 分隔开
- 属于同一个 BFC（块级格式化上下文 ）
- 垂直方向
- 属于普通流

---

## 如何形成一个 BFC

1、float 的值不是 none。（float:left 或者 float:right）

2、position 的值不是 static 或者 relative。（position:absolute 或者 position:fixed）

3、display 的值是 inline-block、table-cell、flex、table-caption 或者 inline-flex

4、overflow 的值不是 visible（overflow:hidden、overflow:scroll）

5、父元素与正常文件流的子元素（非浮动子元素）自动形成一个 BFC

---

## 解决方案

- 父子间

  父元素设置以下属性中的一种：

  ```css
  /* padding-top: 1px; */
  /* overflow: hidden; */
  /* border-top: 1px solid red; */
  /* display: inline-block; */
  /* display: table-cell; */
  ```

- 兄弟间
  - 推荐使用 padding 来代替 margin 使用
  - 将兄弟元素分别作为子元素放在块级元素内，然后将其父级元素的渲染规则该为 BFC

---
