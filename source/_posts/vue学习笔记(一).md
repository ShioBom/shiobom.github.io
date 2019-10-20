---
title: vue学习笔记
date: 2019-08-08 16:19:10
tags: 
   - vue
   - watch侦听器
   - 生命周期
---

今天开始了 Vue 的学习，以下是今天的笔记和总结。对自己不太了解的知识点做个总结以加深记忆。

### 侦听器 watch

**使用场景**

当需要在数据变化时执行异步或开销较大的操作时，如 Ajax 获取数据

- handler回调:侦听的属性值变化的时候执行的回调。
```js
watch: {
        obj: {
            handler: function(newVal, oldVal) {
                console.log(newVal); 
            },
            deep: true,
            immediate: true 
        }
    }
```
- deep:设置为 true 用于监听对象内部值的变化

- immediate:设置为 true 将立即以表达式的当前值触发回调

### 侦听器watch与计算属性computed的区别

- **watch**：监测的是属性值， 只要属性值发生变化，其都会触发执行回调函数来执行一系列操作。可以执行异步任务。
- **computed**：监测的是依赖值，依赖值不变的情况下其会直接读取缓存进行复用，变化的情况下才会重新计算。计算属性不能执行异步任务

### 计算属性与方法的区别

`computed` 是可以缓存的，只有依赖的值没有发生改变，多次访问计算属性得到的值是之前缓存的计算结果，不会多次执行，`methods` 不能缓存，

### 关于组件需要特别注意的点

> 组件的命名中划线形式`child-component`，因为不区分大小写

### 生命周期

![](https://images.cnblogs.com/cnblogs_com/fly_dragon/276813/o_lifecycle-%E6%A0%87%E6%B3%A8%E7%89%88%E6%9C%AC.png?tdsourcetag=s_pctim_aiomsg)

Vue生命周期经历哪些阶段：
总体来说：初始化、运行中、销毁
详细来说：开始创建、初始化数据、编译模板、挂载Dom、渲染→更新→渲染、销毁等一系列过程

- `BeforeCreated`：此时vue（组件）对象被创建了，但是vue对象的属性还没有绑定，如data属性，computed属性还没有绑定，即没有值。数据都没有，当然更不可能有真实 Dom 了
- `Created`：data，computed都执行了。属性已经赋值，但没有动态创建template属性对应的HTML元素，$el属性还不存在。所以，此时如果更改数据不会触发updated函数
> 如果：数据的初始值就来自于后端，可以发送ajax，或者fetch请求获取数据，但是，此时不会触发updated函数
- `beforeMonut`:此时 this.$el有值，但是数据还没有挂载到页面上。即此时页面中的{{}}还没有被替换
-  `Monuted`:此时已经把数据挂载到了页面上,一般来说，我们在此处发送异步请求（ajax，fetch，axios等），获取服务器上的数据，显示在DOM里
- `beforeUpdate`:数据更新了，但是，vue（组件）对象对应的dom中的内部（innerHTML）没有变，所以叫作组件更新前
- `updated`:vue（组件）对象对应的dom中的内部（innerHTML）改变了
- `activated`：keep-alive组件激活时调用
- `deactivated`：keep-alive组件停用时调用
- `beforeDestroy`：vue（组件）对象销毁之前
- `destroyed`：vue组件销毁后

### 关于样式

自己写 css 算是自己薄弱的地方，很需要加强。今天遇到的一点问题是如何将表单前面文字的对齐。最后采用的办法是在文字部分包裹一个 label，使用`display:inline-block`将 label 变成行内块元素，再设置固定宽度，设置`text-align:right`将文字居左。

### 关于函数防抖和函数节流

这个是学习watch时，看到有说能实现防抖，但自己没有概念。于是去查询了一下资料。

#### 定义

- **函数节流**: 指定时间间隔内只会执行一次任务；即规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。
- **函数防抖**: 任务频繁触发的情况下，只有任务触发的间隔超过指定间隔的时候，任务才会执行。
`一个很有趣的解释:函数防抖就是法师发技能的时候要读条，技能读条没完再按技能就会重新读条。）`

#### 应用场景

- 函数防抖
    - search搜索联想，用户在不断输入值时，用防抖来节约请求资源。
    - window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次
- 函数节流
    - 鼠标不断点击触发，mousedown(单位时间内只触发一次)
    - 监听滚动事件，比如是否滑到底部自动加载更多

[详细关于防抖和节流的内容可以看这篇文章](https://juejin.im/entry/58c0379e44d9040068dc952f)