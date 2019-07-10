---
title: antd踩过的坑
date: 2019-07-02 11:40:55
tags:
---

## react antd 踩过的坑

1. Select,Cascader,Datepicker 的下拉框随着页面的滚动而滚动

- 原因：下拉框默认相对于页面 body 定位
- 解决方案：让下拉框相对于父级元素定位即可

  - Select,Cascader

  ```javascript
  getPopupContainer={trigger => trigger.parentNode}
  ```

  - Datepicker

  ```javascript
  getCalendarContainer={trigger => trigger.parentNode}
  ```
