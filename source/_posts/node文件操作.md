---
title: node文件操作
date: 2019-08-06 16:04:07
tags:
---

引入 fs 文件模块即可使用

```javascript
const fs = require("fs");
```

## 文件读取

第二个参数表示编码格式，若未指定，则 data 为原始 buffer

```javascript
fs.readFile("1.txt", "utf-8", (err, data) => {
  if (err) {
    console.log(err);
  } else {
    console.log(data); // 文件内容，string类型
  }
});
```

## 文件写入

### 文件重写

```javascript
const data = "重写内容";
fs.writeFile("1.txt", data, err => {
  if (err) {
    console.log(err);
  } else {
    console.log("写入成功"); // 文件内容，string类型
  }
});
```

### 文件内容追加（在内容末尾添加）

```javascript
const data = "重写内容";
fs.appendFile("1.txt", data, err => {
  if (err) {
    console.log(err);
  } else {
    console.log("写入成功"); // 文件内容，string类型
  }
});
```

### 在指定位置添加内容

- 思路：先使用 readFile 方法读取文件内容，由于读取的内容是 string 类型且有换行，因此可以通过 split 方法，以换行符为分割，将内容字符串分割成一个数组，在使用数组的 splice 方法将要添加的内容添加到指定位置，在将添加内容后的数据通过 join 方法组合成字符串，最后使用 writeFile 方法重写文件。

- 代码：

```javascript
fs.readFile("1.txt", "utf-8", (err, data) => {
  if (err) {
    console.log(err);
  } else {
    let newData = data.split(/\r\n|\n|\r/gm); // 以换行符分割成数组
    newData.splice(2, 0, "添加的内容");
    fs.writeFile("1.txt", newData.join("\r\n"), err => {
      if (err) {
        console.log(err);
      } else {
        console.log("添加成功");
      }
    });
  }
});
```

## 文件重命名

```javascript
fs.rename("1.txt", "2.js", err => {
  if (err) {
    console.log(err);
  } else {
    console.log("修改成功");
  }
});
```

## 文件删除

```javascript
fs.unlink("1.txt", err => {
  if (err) {
    console.log(err);
  } else {
    console.log("删除成功");
  }
});
```

## 获取文件夹文件列表

```javascript
fs.readdir("./", (err, file) => {
  file.forEach(item => {
    console.log(item); // item为文件名，包括文件夹，带文件类型
  });
});
```

## 新建文件夹

```javascript
fs.mkdir("test", err => {
  if (err) {
    console.log("创建失败");
  } else {
    console.log("创建成功");
  }
});
```
