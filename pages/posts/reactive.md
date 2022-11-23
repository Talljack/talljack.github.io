---
title: Vue reactive原理解析
date: 2022-11-20T22:24:00.000+00:00
lang: zh
duration: 35min
description: Vue reactive原理解析
---

## 前言

大家好，我是落叶小小少年，虽然比较菜，虽然才开始写作分享，我始终相信

-   **核心demo更容易理解深的技术点**
-   **每一次基础的学习都是对知识的巩固**

因为从年初就开始使用Vue3了，现在才来学习Vue3，但是也不算晚，学到就是赚到，知识无价，只要今天的知识比昨天多一点就是在丰富自己。那么我们就来学习下**Vue3的响应式原理**

## Vue3的响应式原理

大家都知道Vue3使用的是Proxy进行代理的，这里我们先用Proxy实现一个最基础的响应式

我知道看vue3源码是一件比较头疼的事，所以在这里给大家提取出最简单的一个响应式demo，方便大家学习和理解

### 1. 响应式函数

先写一个创建reactive的函数，里面使用了Proxy进行target对象的代理

```
function createReactiveObject(target, baseHandler) {
  const proxy = new Proxy(target, baseHandler);
  return proxy;
}
```

### 2. Proxy的handler函数

接着我们实现对应的baseHandler函数，其中最主要的是get和set（当然还有has、ownKeys等就先不实现）

```
function get(target, key, recivier) {
  const res = Reflect.get(target, key, recivier);
  track(target, key);
  // 深层响应式
  return res !== null && typeof res === 'object' ? createReactiveObject(res) : res;
}
​
function set(target, key, value, recivier) {
  const oldValue = target[key];
  const res = Reflect.set(target, key, value, recivier);
  if (!Object.is(value, oldValue)) {
    trigger(target, key);
  }
  return res;
}
```

对应的get和set函数做的最主要的事情就是追踪和触发

追踪就是指当你去获取这个target上的key值时，能追踪到你使用了这个target上的key值，如果放到target对象来说，就是指你调用了target[key]，比如`console.log(target[key])`，这就是最简单的访问

触发就是指当你去修改这个值时，你已经追踪了这个值，那么你会有一个bucket去存这个副作用函数，当你修改对应key的值的时候，就需要从这个bucket里面找到对应的key的副作用函数，再去执行，达到响应式的效果

### 3. 如何设计副作用函数收集数据结构

那么我们应该怎么设计这个bucket才能正确存储上这个effect呢？

我们已经知道访问target[key]时要进行track，可以看出来和target和key有关系，但是一个target[key]肯定不止一个副作用函数使用，当然一个target上也有多个key可以收集副作用函数，所以我们需要这么设计：

**targetMap是一个target对象到Map的映射，一个target上有多个key，所以key可以用Map存储**

**keyToDepMap是每个key对应的dep的映射，一个key可以对应多个dep（副作用函数收集）**

![在这里插入图片描述](https://img-blog.csdnimg.cn/fa520622da9347c78e38ab72fef6cb41.png)


### 4. 收集和触发

收集主要在track函数里面，触发则主要在trigger函数里面，也就是按照我们上面的结构在get时进行副作用收集和set时取出对应的副作用函数触发即可

```
const targetMap = new WeakMap();
function track(target, key) {
  let depsMap = targetMap.get(target);
  if (!depsMap) {
    targetMap.set(target, (depsMap = new Map()));
  }
  let dep = depsMap.get(key);
  if (!dep) {
    depsMap.set(key, (dep = new Set()));
  }
  // 添加副作用函数
  dep.add(effect);
}
​
function trigger(target, key) {
  const depsMap = targetMap.get(target);
  if (!depsMap) return;
  const dep = depsMap.get(key);
  if (dep) {
    // 副作用函数触发
    dep.forEach((fn) => fn());
  }
}
```
最简单的方法就是直接写一个effect函数吧

上面看到我们实现了track和trigger，那么副作用函数从哪来呢？
```
const data = {
  name: 'reactive'![在这里插入图片描述](https://img-blog.csdnimg.cn/9335d91de11b4df49f02de84e522c8a8.png)


}
const proxyData = createReactiveObject(data, {
  get,
  set,
});
function effect () {
  // 直接访问了proxyData上的name属性
  console.log(proxyData.name); // reactive
}
effect()
// 修改后会触发trigger里面收集到的函数
proxyData.name = 'ref' // ref
```
正如我们所预料的，当修改了name值后，重新触发了effect函数，也就是从targetMap中找到了对应的effect函数，重新输出了ref值。targetMap的值如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/3ced2126f9e542858462c57c4f820787.png)


至此已经实现了一个最小最简单的响应式原理。

![在这里插入图片描述](https://img-blog.csdnimg.cn/4e4b259e68324645b30a3fe9117f7ee1.png)


### 5. 解决effect名字写死问题

大家还记得上面我们在track函数里面收集effect函数时吗？

`dep.add(effect)`这个add是固定添加effect函数的，这里就会有两个问题

-   只能使用effect这个副作用函数名
-   如果多个字段的副作用函数没办法区分开来

那么我们怎么解决这个问题呢？

很简单，就是用一个专门的name来收集effect函数，比如activeEffect，在执行某个副作用函数时就将activeEffect赋值乘这个副作用函数就行了

```ts
// 接上
let activeEffect
function effect(fn) {
  activeEffect = fn
  fn()
  activeEffect = null
}
// 上面的track里面添加副作用函数对应修改成如下即可
// 添加副作用函数
dep.add(activeEffect)
```
好了，接下来我们测试一下
```ts
// proxyData接上面的代码
effect(() => {
  console.log(proxyData.name)
})
proxyData.hobby = 'coding'
effect(() => {
  console.log(proxyData.hobby)
})
proxyData.hobby = 'playing'
```
运行发现，报错了，思考一下为什么fn会不存在呢？track的时候为什么会收集一个空函数呢？
![在这里插入图片描述](https://img-blog.csdnimg.cn/5aef09bdaef74079920a5ad4a3a682f9.png)


如果你看过《你不知道的js》你就会知道，当`proxyData.hobby = 'coding';`执行这段代码时，会先通过LHS找到proxyData上的hobby属性，然后就类似触发了getter拦截，于是就在track的时候把activeEffect函数给收集了，但是这不是我们想要的情况，所以我们应该在track收集的时候判断activeEffect是否为空

```ts
// 修改track函数
function track(target, key) {
  // 增加判断是否为空
  if (!activeEffect)
    return
  let depsMap = targetMap.get(target)
  if (!depsMap)
    targetMap.set(target, (depsMap = new Map()))

  let dep = depsMap.get(key)
  if (!dep)
    depsMap.set(key, (dep = new Set()))

  // 添加副作用函数
  dep.add(activeEffect)
}
```
再次执行上面的例子🌰，结果如下，正如我们所期望的一样，hobby值修改，hobby收集的effect函数重新执行了
![在这里插入图片描述](https://img-blog.csdnimg.cn/671a52a1adb64e15af8079c708a0d835.png)


可能用图来它们之间的存储结构或许你更清楚明白🫡

![在这里插入图片描述](https://img-blog.csdnimg.cn/c9fa3f611be64f60a157758b37b537f3.png)


## 封装reactive

上面我们已经实现了创建基础响应式的函数，那么只需要简单的封装下就能得到reactive函数

```ts
const baseHandler = {
  get,
  set,
}
function reactive(obj) {
  return createReactiveObject(obj, baseHandler)
}
// get函数里的递归调用也修改一下
function get(target, key, recivier) {
  const res = Reflect.get(target, key, recivier)
  track(target, key)
  // 使用reactive即可
  return res !== null && typeof res === 'object' ? reactive(res) : res
}
```
那我们就用上面的reactive来试试效果
```ts
const proxyData = reactive(data)
effect(() => {
  console.log(proxyData.name) // reactive
})
proxyData.name = 'reactive2'
// reactive2
```

## 实现ref

实现ref也很简单，因为我们已经有reactive函数了，我们只需要对reactive函数封装一下就可以达到ref的效果

先想一想我们是怎么使用ref的呢？
```ts
const count = ref(10)
// 通过value来取值的
console.log(count.value)
```
所以我们可以利用value的属性创造一个reactive的obj返回即可。
```ts
function ref(value) {
  // {value: value}
  return reactive({ value })
}
```
测试一下是否能生效
```ts
const count = ref(10)
effect(() => {
  console.log(count.value) // 10
})
count.value = 20
// 20
```

可以看到修改count的值，重新执行了effect函数，这个最基本的ref函数没啥问题了

## 实现computed

computed的实现也是在内部封装了一个effect函数，达到响应式的效果。其次就是它也是通过value来获取值，所以我们可以利用ref来实现，返回一个ref值就行
```ts
function computed(fn) {
  const res = ref()
  effect(() => {
    res.value = fn()
  })
  return res
}
```
来测试一下，是否能根据以来的值去更新
```ts
const count = ref(10)
const num = ref(20)
const total = computed(() => {
  return count.value + num.value
})
consoel.log(total.value) // 30
num.value = 30
console.log(total.value) // 40
count.value = 20
console.log(total.value) // 50
```
不出所料，computed也能达到最简单的收集触发了

以下是基本响应式的所有代码

![在这里插入图片描述](https://img-blog.csdnimg.cn/92b4e77d6ceb494f96df233d05efbdbf.png)


## 总结

至此，我们写出了一个最基本的响应式系统的demo和最小实现了ref、reactive和computed

当然你可能也会有疑问，为什么effect函数实现了，computed都实现了为啥不实现watch呢？

实现watch需要增加调度功能，也就是说可以控制trigger触发的时机、次数以及方式

所以后续我们要完善这个响应式系统，增加调度功能、处理嵌套effect、自增多次又要怎么解决等等

如果你觉得这篇文章对你有帮助，请点个赞，鼓励一下作者吧

## 工具

画图工具用的是[excalidraw]()，可以手画出比较好看的图

表情工具用的是[表情符号大全](https://play.igo9go.cn/emojiall/)，里面有很多表情，支持一键快速复制

表情包制作用的是[在线表情制作器](https://www.wakatool.com/maker)，可以选择自己喜欢的表情进行制作

代码图片美化工具是[carbon](https://carbon.now.sh/)，支持多种语言，多种风格，很好用
