---
title: Pinia 入门
date: 2022-05-06 16:17:59
description: Pinia 是 Vue 的存储库，允许跨组件/页面共享状态
tags:
---
## 介绍

[官方文档](https://pinia.vuejs.org/)
Pinia 是 Vue 的存储库，允许跨组件/页面共享状态

### 与Vuex的区别？

1.没有mutations，只有state,getters,actions
2.state的使用不同，采用函数式声明，与vue中的data属性一致。可以直接修改state中的值。
3.分模块不需要modules，根据文件区分即可
4.对Typescript支持更友好
5.体积更小

## 安装

```安装
yarn add pinia
# or with npm
npm install pinia
```

## 使用

#### 引入

```js
import { createPinia } from 'pinia'
app.use(createPinia())
```

#### 定义

##### 基础语法

```定义
# ./store/index.js
import { defineStore } from 'pinia'
export const useStore = defineStore('ID', {
  state: () => {
    return {
        name: '',
        count: 0
    }
  },
  getters: {
    doubleCount: (state) => { 
        return state.count * 2
    }
  },
  actions: {
    increment(val) {
      this.count+=val
    }
  }
})
# or
export const useStore = defineStore({
  id: 'ID',
  state: () => {
    return {
        name: '',
        count: 0
    }
  },
  getters: {
    doubleCount: (state) => { 
        return state.count * 2
    }
  },
  actions: {
    increment(val) {
      this.count+=val
    }
  }
})
```

```使用
# 使用
import { useStore } from './store/index.js'

export default {
  setup() {
    const store = useStore()
    # 直接访问
    store.count = 10
    const newCount = store.doubleCount
    store.increment(10)
    return {
      store,
      newCount
    }
  },
}
```

##### storeToRefs

结构赋值时，需要 storeToRefs 处理数据
```
import { useStore } from './store'
import { storeToRefs } from 'pinia'
export default {
  setup() {
    const store = useStore()
    const { name, count, doubleCount } = storeToRefs(store)
    return {
      name,
      count,
      doubleCount
    }
  },
}
```

##### 批量更改值

`$patch` 方法

```批量更改值
const store = useStore()
store.$patch(state => {
    state.count++
    state.num = ''
})
```

## 持久化存储

下载 `pinia-plugin-persist`
[文档](https://seb-l.github.io/pinia-plugin-persist/)

```插件
npm install pinia-plugin-persist --save
```

引入 `pinia-plugin-persist`
```
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import piniaPersist from 'pinia-plugin-persist'

const pinia = createPinia()
pinia.use(piniaPersist)

createApp({})
  .use(pinia)
  .mount('#app')
```

配置项`persist`

```配置
export const useStore = defineStore('ID', {
    state: () => {return{}},
    getters: {},
    actions: {},
    persist: {
        enabled: true,
        strategies: []
    }
})
```
