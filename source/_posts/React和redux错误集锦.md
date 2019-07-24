---
title: React和redux错误集锦
date: 2019-07-24 12:48:28
tags: redux
---
## react 基础

### 安装react环境(只安装一次)

```
yarn gloabal add create-react-app 
```

### 创建react项目(myapp)

```
create-react-app myapp
cd myapp
yarn start 
```

### jsx语法

- 在 js 里面引用 html，需要使用`()`,可以省略，但在 html 里面使用 js，需要使用 mustacle 语法,用`{}`包起来。
- 事件绑定函数如果不写成箭头函数，而是普通函数，需要绑定 this 值。
- 最外层只能有一个父级
- jsx 是通过 babel-preset-react 来进行转码的。

### react页面更新方式

- 通过 setState 方式，更改 state 状态值，触发页面自动刷新

### props 的只读性

无论是使用函数或是类来声明一个组件，它决不能修改它自己的props。
> 需要避免的情况：需要将一个对象中的某属性变更之后赋给另外一个对象。这样传的是引用。

### 组件创建方式

- 函数式无状态组件
    使用函数定义组件时，没有内部状态，组件内接受的 props 为只读状态，只做数据渲染用。

- ES6 有状态组件
    - 具有局部状态：状态是私有的，完全受控于当前的组件
    - 具有生命周期钩子函数

### 正确的使用状态

**（1）不要直接更新状态**

- 构造函数是唯一能够初始化 this.state 的地方。
- 通过 setState 更改状态

**（2）状态更新可能是异步的**
- this.props 和 this.state 可能是异步更新的，你不应该依靠它们的值来计算下一个状态。可以使用下面的方式。

    ```js
    //错误
    this.setState({
        counter: this.state.counter + this.props.increment,
    });
    //正确
    this.setState((prevState, props) => ({
        counter: prevState.counter + props.increment
    }));
    ```
**（3）状态更新合并**

### react 里的表单

**（1）受控组件**
**（2）使用ref**
ref 放到传统 html 元素上，返回该节点，如果放到自己创建的元素上，返回的就是 react 元素对象。
```js
<input ref={(node)=>{this.input = node}}/>
```
这种方式不推荐使用，除非遇到上传那种元素很难拿到的情况。

### 状态提升

遇到几个组件需要共用状态的情况，将状态提升到父组件，子组件通过调用父组件的方法修改状态，该状态通过 props 传递给子组件使用。

### 组件间通信

**（1）父组件向子组件通信**
通过传递 props 的方式，向子组件通信，如果父组件与子组件之间不止一个层级，可以通过扩展运算符`...`传递给更深层次的组件

**（2）子组件向父组件通讯**
同样也是通过传递 props 的方式，只不过这里传递的是作用域为父组件的函数,子组件调用该函数，将子组件想要传递的信息，作为参数，传递到父组件的作用域中。

```js
//父组件中
handleDel(ind){
    this.setState(state=>{
        state.date = state.data.splice(ind,1);
        return state;
    })
}
    ...
<EduItem key={ind} index={ind} 
    handleDel={this.handleDel.bind(this)}>
</EduItem>
//也可以使用箭头函数，则不需要bind（this）


//子组件中
 {this.props.index!==0?
 <button onClick={()=>{this.props.handleDel(this.props.index)}}>删除</button>:
 null}
```

**（3）兄弟组件通讯**

对于没有直接关联的组件，需要找到他们公有的父节点，将状态提升到该父节点，再结合前两种方式，进行通信，但这种方法有一个问题，由于父节点的 state 发生变化，会触发该父节点及从属于该节点的子组件的生命周期

## redux
**(1) store**

接受一个函数作为参数，返回一个store对象。这个函数就是一个reducer。
```js
import { createStore } from 'redux';
const store = createStore(reducer)
```

**(2) state**

对store生成快照，获得当前时刻的状态。
```js
const state = store.getState()
```

**(2) action**

action 是一个对象，是 View 发出的通知，表示 state 要发生变化了。改变state的唯一方法，就是使用action，他会运送数据到store
```js
const action ={
    type:"ADD_TODO",//action的类型
    payload:"hello world" //携带的信息
}
```

**(3) Action Creator**

定义的用来生成action的函数

**(4) dispatch**

发出action的唯一方式，接受一个action对象作为参数。
```js
store.dispatch(action);
```

**(5) reducer**

store 接受到新的 action 之后，如何才能更新 View 呢，通过返回新的 state，这个state的计算过程就叫做 reducer，reducer是一个函数，他接受当前 state 和一个 action 为参数，返回新的 state。
```js
const defaultState = 0;
//整个应用的初始状态可作为state的默认值
const reducer = (state = defaultState, action) => {
  switch (action.type) {
    case 'ADD':
      return state + action.payload;
    default: 
      return state;
  }
};

//这个state就是新的state
const state = reducer(1, {
  type: 'ADD',
  payload: 2
});
```
Reducer 函数里面不能改变 State，必须返回一个全新的对象，必须是一个纯函数，可以参照以下写法
```js
function reducer(state, action) {
    return Object.assign({}, state, { thingToChange });
    // 或者
    return { ...state, ...newState };
  }
  
  // State 是一个数组
  function reducer(state, action) {
    return [...state, newItem];
}

```
> 注意：
> - 修改传入参数
> - 执行有副作用的操作，如 API 请求和路由跳转；
> - 调用非纯函数，如 Date.now() 或 Math.random()

### PureComponent

新旧值一样，就不会刷新页面，提高性能，但只是浅比较，如果需要传入的 *props* 或者 *state* 是引用值，要引起更新必须是新的引用。

### propsTypes



### context

共享对于组件树来说是“全局”的数据，例如当前认证的用户、主题或首选语言，避免通过组件层层传递 *props*

**API**

- **React.createContext**

```js
// context.js
import * as React from 'react';
// 默认主题是Light
export const { Provider, Consumer } = React.createContext("Light");
```

- **Context.Provider**

    Provider是需要使用Context的所有组件的根组件。它接受一个value作为props，它表示Context传递的值，它会修改你在创建Context时候设定的默认值。
```js
import React from 'react';
import './App.css';
import Father from './components/Father.js'
import {Provider} from "./context";

function App() {
  return (
    
    <div className="App">
      <Provider value="dark">
        <Father></Father>
      </Provider>

    </div>
  );
}

export default App;
```
- **Context.Consumer**

    Consumer 表示消费者,它接受一个 render props 作为唯一的 children 。其实就是一个函数，这个函数会接收到 Context 传递的数据作为参数，并且需要返回一个组件。

```js
import React, { Component } from 'react'
import { Consumer } from '../context'
export default class Father extends Component {
  render() {
    return (
      <Consumer>
        {
          theme=><div>this theme is {theme}</div>
        }
      </Consumer>
      
    )
  }
}
//this theme is dark
```

- Class.contextType

挂载在 class 上的 contextType 属性被重赋值为一个由 React.createContext() 创建的 Context 对象。这能让你使用 this.context 来使用最近 Context 上的那个值。==你可以在任何生命周期中访问到它，包括 render 函数中。==
```js
import React, { Component } from 'react'
import { Consumer} from '../context'
// const myContext = React.createContext("green");
export default class Father extends Component {
  render() {
    return (
      <Consumer>
        {
          theme=><div>this theme is {theme},this background is {this.context}</div>
        }
      </Consumer>
      
    )
  }
}
//this theme is dark,this background is green
const myContext = React.createContext("green");
Father.contextType = myContext;
```

### 高阶组件

高阶组件是一个==函数==，不是一个组件

> - 不要在高阶组件中直接修改传入组件，容易产生耦合
> - 最大化使用组合
> - 不要在render中使用高阶组件
> - 高阶组件不能传递refs

## react-redux

## 进阶

## 遇到的错误




