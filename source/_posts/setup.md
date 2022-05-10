---
title: setup
date: 2022-05-10 10:13:00
description: Vue 组合式 API 编译时的语法糖 setup
tags:
    - vue
categories: 入门知识
---

## 基础语法

- 任何在`<script setup>`声明的顶层的绑定 (包括变量，函数声明，以及 import 引入的内容) 都能在模板中直接使用

```vue
<script setup>
    import { ref } from 'vue'
    const msg = ref('hello')
    const updateMsg = () => {
        msg.value = 'hello world'
    }
</script>
<template>
  <div @click="updateMsg">{{ msg }}</div>
</template>
```

如上所示，定义的顶层的`msg`和`updateMsg()`可以直接在 template 使用

## 自定义指令的使用

必须以 `vNameOfDirective` 的形式来命名本地自定义指令，以使得它们可以直接在模板中使用
就是以`v`开头,`Directive`结尾

```vue
<script setup>
const vMyDirective = {
  beforeMount: (el) => {
    console.log(el.tagName) // H1
  }
}
</script>
<template>
  <h1 v-my-directive>测试指令</h1>
</template>
```

## 组件

### defineProps 和 defineEmits

```vue
<!-- ./parent.vue -->
<script setup>
import { ref } from 'vue'
import Child from './child.vue'
const title = ref('hello')
</script>

<template>
    <Child v-model:title="title"></Child>
</template>
```

```vue
<!-- ./child.vue -->
<script setup>
const props = defineProps({
  title: String
})
const emit = defineEmits(['update:title'])
const onUpdate = function () {
    emit('update:title', 'hello world')
}
</script>

<template>
    <h1>{{ title }}</h1>
    <div @click="onUpdate">点击，将title修改为hello world</div>
</template>
```

### defineExpose

使用`<script setup>`的组件是默认关闭的，也即通过模板`ref`或者`$parent`链获取到的组件的公开实例，不会暴露任何在`<script setup>`中声明的绑定

```vue
<!-- ./parent.vue -->
<script setup>
    import Child from './child.vue'
    import { ref, nextTick } from 'vue'
    const childRef = ref(null)
    console.log(childRef.value.a) // undefined
    console.log(childRef.value.b) // 1
</script>

<template>
    <Child ref="childRef" />
</template>
```

```vue
<!-- ./child.vue -->
<script setup>
const a = 1
const b = 1

defineExpose({
  b
})
</script>
```

如上所示，只有使用`defineExpose`暴露的属性`b`，在ref实例中可以拿到。`a`属性没有暴露，则为`undefined`

### useSlots 和 useAttrs

在 `<script setup>` 使用 slots 和 attrs 的情况应该是很罕见的，因为可以在模板中通过 `$slots` 和 `$attrs` 来访问它们。在你的确需要使用它们的罕见场景中，可以分别用 `useSlots` 和 `useAttrs` 两个辅助函数
`useSlots` 和 `useAttrs` 是真实的运行时函数，它会返回与 `setupContext.slots` 和 `setupContext.attrs` 等价的值，同样也能在普通的组合式 API 中使用。

```vue
<script setup>
import { useSlots, useAttrs } from 'vue'

const slots = useSlots()
const attrs = useAttrs()
</script>
```

### 与普通的 `<script>` 一起使用

`<script setup>` 可以和普通的 `<script>` 一起使用。普通的 `<script>` 在有这些需要的情况下或许会被使用到：

- 无法在 `<script setup>` 声明的选项，例如 `inheritAttrs` 或通过插件启用的自定义的选项。
- 声明命名导出。
- 运行副作用或者创建只需要执行一次的对象

```vue
<script>
// 普通 <script>, 在模块范围下执行(只执行一次)
runSideEffectOnce()

// 声明额外的选项
export default {
  inheritAttrs: false,
  customOptions: {}
}
</script>

<script setup>
// 在 setup() 作用域中执行 (对每个实例皆如此)
</script>
```

```vue
<script setup>
    console.log('script setup')
</script>
<script>
console.log('script')
export default {
    setup() {
        console.log('setup function')
    }
}
</script>
```

控制台打印结果：

```Txt
script
script setup
```

如上所示：

- 当`<script setup>`和`<script>`同时存在时，优先执行`<script>`再执行`<script setup>`
- 当存在`<script setup>`时，`<script>`中的`setup() {}`不再执行

## Typescript中 props/emit 的使用

```ts
const props = defineProps<{
  foo: string
  bar?: number
}>()

const emit = defineEmits<{
  (e: 'change', id: number): void
  (e: 'update', value: string): void
}>()
```

`withDefaults` 编译器宏:

```ts
interface Props {
  msg?: string
  labels?: string[]
}

const props = withDefaults(defineProps<Props>(), {
  msg: 'hello',
  labels: () => ['one', 'two']
})
```

总结：

- 固定格式的自定义指令 `vNameOfDirective`
- 使用`defineProps`和`defineEmits`的方法，对参数的接受和传递
- 使用`defineExpose`暴露出父组件要访问的属性
