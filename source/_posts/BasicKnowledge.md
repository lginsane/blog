---
title: javascript基础知识
date: 2023-03-04 10:13:29
description: javascript常用api整理，便于查询
tags:
  - javascript
categories: 前端
---


### splice

用于添加或删除数组中的元素

#### 语法

```js
array.splice(index, howmany, item1,.....,itemX)
```

#### 参数

`index`: 必需。规定从何处添加/删除元素。该参数是开始插入和（或）删除的数组元素的下标，必须是数字。
`howmany`: 可选。规定应该删除多少元素。必须是数字，但可以是 "0"。如果未规定此参数，则删除从 index 开始到原数组结尾的所有元素。
`item1, ..., itemX`: 可选。要添加到数组的新元素
