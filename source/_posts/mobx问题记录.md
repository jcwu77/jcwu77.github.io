---
title: mobx问题记录
date: 2019-09-10 10:29:35
tags:
---

## 安装（基于 create-react-app）

- npm 安装 `npm install mobx mobx -D`
- 使用装饰器

  - 由于 babel 的更新，解决浏览器无法识别装饰器的解决方案为安装`npm install @babel/plugin-proposal-decorators -D`，然后在`package.json`中添加

  ```javascript
   "babel": {
    "plugins": [
      [
        "@babel/plugin-proposal-decorators",
        {
          "legacy": true
        }
      ]
    ],
   }
  ```

- 使用示例

```javascript
import React from "react";
import { observable } from "mobx";
import { observer } from "mobx-react/custom"; // 注意，如果写成import { observer } from "mobx-react" 会报错

import styles from "./index.module.less";

@observer
class MobxTest extends React.Component {
  @observable count = 0;

  handleAdd = () => {
    this.count++;
  };

  handleDelete = () => {
    this.count--;
  };

  render() {
    return (
      <div className={styles.container}>
        <span onClick={this.handleAdd}>增加</span>
        {this.count}
        <span onClick={this.handleDelete}>减少</span>
      </div>
    );
  }
}

export default MobxTest;
```
