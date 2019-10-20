---
title: vue学习笔记(二)
date: 2019-08-09 16:23:01
tags:
   - vue
   - 父子组件通信
---

## 需要注意的点

1. 重复行的功能一般做成组件。
2. 在子组件定义data，必须是一个函数，返回一个对象。
3. 自定义事件命名必须是小写+中划线形式，如listen-event

## 父子组件通信

在做项目的过程中经常遇到需要父子组件通信的情况，这里总结一下常用的方法。

### 父传子

在父组件通过 v-bind 指令传值给子组件,在子组件通过props接收
```javascript
//父组件
<todo-item :todo="todo" /> //将父组件的todo传给子组件
//子组件
 props: ["todo"]
```

### 子传父

父组件在子组件上绑定自定义事件，然后在子组件通过 emit 广播触发该事件调用相关函数。传递参数给父组件

```javascript
//父组件中
methods:{
	updateTodo(){
		...
	}
}
...
<todo-item @update="updateTodo" />
//子组件
this.$emit('update',param1,param2)
```
> 注意：自定义事件不能用驼峰，推荐使用中划线。

## vue 组件开发props验证

- prop类型 ：这不仅为你的组件提供了文档，还会在它们遇到错误的类型时从浏览器的 JavaScript 控制台提示用户

```javascript
props: {
  title: String,
  likes: Number,
}
```

- default : 默认值，当type为Array或Object时，default必须是一个函数。

- required ： 必填项

```javascript
	// 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
	},
	// 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
```

> 每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你不应该在一个子组件内部改变 prop。例如以下的情况

1.  这个 prop 用来传递一个初始值；这个子组件接下来希望将其作为一个本地的 prop 数据来使用。推荐使用

```javascript
props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter
  }
}
```
2. 这个 prop 以一种原始的值传入且需要进行转换

```javascript
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

## .native 事件修饰符事件修饰符

你可能想在某个组件的根元素上监听一个原生事件。可以使用 v-on 的修饰符 .native 。通俗点讲：就是在父组件中给子组件绑定一个原生的事件，就将子组件变成了普通的HTML标签，不加'. native'事件是无法触发的。

## v-once

被定义了 v-once 指令的元素或组件（包括元素或组件内的所有子孙节点）只能被渲染一次。首次渲染后，即使数据发生变化，也不会被重新渲染。一般用于静态内容展示，可以用来优化性能。

```javascript
<div id="app">
    <div v-once>
        {{content}}
    </div>
</div>
```

##  动态组件

**:is 指令**

type 定义的是组件名称的变量，通过改变 type 值可以达到切换的目的，比如 `tab栏切换`。
```
<component :is="type"><component/>
```
currentTabComponent 可以包括
- 已注册组件的名字，或
- 一个组件的选项对象

## 插槽
