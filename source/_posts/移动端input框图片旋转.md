---
title: 移动端input框图片旋转
date: 2020-04-26 14:18:36
tags:
---

## 问题描述

使用 input 进行图片上传，上传后的图片会旋转 90 度。

## 解决方案

1. 使用 EXIF 库来判断上传的图片是否需要旋转
2. 将图片转换成 base64 后使用 canvas 进行旋转
3. 根据需求再将 base64 转换成 file 类型

## 详细步骤

1. 安装 exif 库 `npm install exif-js -D`
2. 判断图片是否需要旋转。EXIF 中，包含一个 Orientation 参数，用来记录拍摄照片时的方向。
   | Orientation | 含义 |
   | ------ | ------ |
   | 1 | 正常 |
   | 2 | 正常镜像 |
   | 3 | 顺时针旋转 180° |
   | 4 | 顺时针旋转 180° 镜像 |
   | 5 | 顺时针旋转 270° 镜像 |
   | 6 | 顺时针旋转 270° |
   | 7 | 顺时针旋转 90° 镜像 |
   | 8 | 顺时针旋转 90° |

   ```javascript
   // 获取图片信息，判断是否旋转
   isNeedFixPhoto = (file) => {
     return new Promise(function (resolve, reject) {
       Exif.getData(file, function () {
         let Orientation = Exif.getTag(this, 'Orientation');
         if (Orientation && Orientation !== 1) {
           //图片角度不正确
           resolve({ flag: true, Orientation });
         } else {
           //不需处理直接上传
           resolve({ flag: false, Orientation });
         }
       });
     });
   };
   ```

3. 将图片转成 base64

   ```javascript
   // 图片转base64
   file2Base64 = (file) => {
     return new Promise(function (resolve, reject) {
       let reader = new FileReader();
       reader.readAsDataURL(file);
       reader.onload = function (ev) {
         resolve(ev.target.result);
       };
     });
   };
   ```

4. 对图片进行旋转处理

   ```javascript
   // 旋转处理图片
   best4Photo = (resultBase64, Orientation, num, w) => {
     return new Promise(function (resolve, reject) {
       let image = new Image();
       image.src = resultBase64;
       image.onload = function () {
         let imgWidth = w,
           imgHeight = (w * this.height) / this.width; //获取图片宽高
         let canvas = document.createElement('canvas');
         let ctx = canvas.getContext('2d');
         canvas.width = imgWidth;
         canvas.height = imgHeight;
         if (Orientation && Orientation !== 1) {
           switch (Orientation) {
             case 6: // 旋转90度
               canvas.width = imgHeight;
               canvas.height = imgWidth;
               ctx.rotate(Math.PI / 2);
               ctx.drawImage(this, 0, -imgHeight, imgWidth, imgHeight);
               break;
             case 3: // 旋转180度
               ctx.rotate(Math.PI);
               ctx.drawImage(this, -imgWidth, -imgHeight, imgWidth, imgHeight);
               break;
             case 8: // 旋转-90度
               canvas.width = imgHeight;
               canvas.height = imgWidth;
               ctx.rotate((3 * Math.PI) / 2);
               ctx.drawImage(this, -imgWidth, 0, imgWidth, imgHeight);
               break;
             default:
               break;
           }
         } else {
           ctx.drawImage(this, 0, 0, imgWidth, imgHeight);
         }
         const result = canvas.toDataURL('image/jpeg', num);
         resolve(result);
       };
     });
   };
   ```

5. 最终执行方法

   ```javascript
   // 图片修正
   repairPhoto = async (file, num, w) => {
     const result = await isNeedFixPhoto(file);
     const resultBase64 = await file2Base64(file);
     const Orientation = result.Orientation;
     const numb = num || 1;
     if (result.flag) {
       // 处理旋转
       return await best4Photo(resultBase64, Orientation, numb, w);
     } else {
       // 不处理旋转
       return await best4Photo(resultBase64, 1, numb, w);
     }
   };
   ```

6. base64 转 file

   ```javascript
   // base64转file
   export const base64URLtoFile = (base64Data, filename) => {
     let arr = base64Data.split(','),
       mime = arr[0].match(/:(.*?);/)[1],
       bstr = atob(arr[1]),
       n = bstr.length,
       u8arr = new Uint8Array(n);
     while (n--) {
       u8arr[n] = bstr.charCodeAt(n);
     }
     return new File([u8arr], filename, { type: mime });
   };
   ```
