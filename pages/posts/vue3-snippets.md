---
title: 使用这个vue snippets，提高百倍开发效率
date: 2022-10-15T17:00:00.000+00:00
lang: zh
duration: 25min
description: 讲解了snippets的作用以及使用，以及vue-3-snippets插件
---

# vue-snippets

随着开发者的年限的增加，不仅头发有点少，重复的代码也不断的增多，为了能够少写代码，GitHub友好的给我们提供了智能代码提示，而我们也以友好的方式赞助它（赞助代码同时也赞助资金🤬），当然也有很多开发者写了类似的插件。

那我为什么要写呢？
我想要有适合自己的代码片段，同时也支持更多的代码片段，更加方便开发者快速使用。

## snippets是什么？
snippets简单来说就是代码片段，比如你想实现下面这段代码
```ts
<div v-for="item in items" :key="item.id">
  {{ item }}
</div>
```
那你每次重新手敲，是不是很麻烦，没有snippets的话那么你需要每个都手写
而当你使用snippets的时候，那么你可以直接使用`vfor-arr`并且写的时候还有提示，方便快速查看和使用（得益于vscode插件的功能）

很多人会在本地`settings`里面设置对应的一些基础片段，但是毕竟数量太多了，所以装插件是比较方便使用的。

## vue-3-snippets插件支持的功能
1. 支持快速定义vue的模板
使用`vbase`可以快速生成一下代码
```ts
<script lang="ts" setup>
import { ref } from 'vue'
</script>

<template>
  <div>
    
  </div>
</template>

<style lang="scss" scoped>
  
</style>
```
还支持使用tsx和render的语法的代码模板
2. 支持模板语法
使用`vfor-arr`可以快速生成`for`循环的代码
```ts
<div v-for="item in items" :key="item.id">
  {{ item }}
</div>
```
3. 支持vue script语法
使用`vref`可以快速生成如下代码
```ts
const name = ref(initialValue)
```
4. 支持vue-router相关代码
比如使用`vrouter-beforeRouteEnter`指令可以快速生成如下代码
```ts
beforeRouteEnter(to, from, next) {
  // called before the route that renders this component is confirmed.
  // does NOT have access to `this` component instance, because it has not been created yet when this guard is called!
  // can call `next`...
}
```
5. 支持vuex相关代码
使用`vuexbase-type`可以快速生成`vuex`的配置模板并且还带`typescript`
```ts
import type { InjectionKey } from 'vue'
import type { Store } from 'vuex'
import { useStore as baseUseStore, createStore } from 'vuex'

export interface State {
  count: number
}

export const key: InjectionKey<Store<State>> = Symbol()

export const store = createStore<State>({
  state: {
    count: 0
  }
})

// 定义自己的 `useStore` 组合式函数
export function useStore() {
  return baseUseStore(key)
}
```
更多的文档请查看[vue-3-snippets](https://github.com/Talljack/vue3-snippets)
目前支持以上这些功能，如果有需要更多的功能请前往[vue-3-snippets](https://github.com/Talljack/vue3-snippets)提交issue
当然也欢迎提交pr

希望这个snippets能帮助到大家。
