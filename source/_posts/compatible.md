---
title: 问题总结
date: 2022-02-18 10:00:00
description: 工作中遇到的问题！
tags:
  - 其他
categories: 总结
---


## 谷歌记住密码样式问题

```谷歌记住密码样式问题
input {

}
&:-internal-autofill-previewed,
&:-internal-autofill-selected
                -webkit-text-fill-color: #2B8888 !important;
                transition: background-color 5000s ease-in-out 0s !important;
```
