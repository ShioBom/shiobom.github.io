---
title: react-Hooks
date: 2019-07-29 17:31:53
tags: react
---

### useState

调用 useState Hook 声明一个 state，接受的参数为该变量的初始值，返回一对值，第一个返回值是变量名，第二个返回值是改变该变量的函数。

```js
 1: import React, { useState } from 'react';
 2:
 3:  function Example() {
 4:    const [count, setCount] = useState(0);
 5:
 6:    return (
 7:      <div>
 8:        <p>You clicked {count} times</p>
 9:        <button onClick={() => setCount(count + 1)}>
10:         Click me
11:        </button>
12:      </div>
13:    );
14:  }
```
>    可以使用多次，让函数组件也能使用 state


### useEffects

通过使用这个 Hook，你可以告诉 React 组件需要在渲染后执行某些操作。React 会保存你传递的函数（我们将它称之为 “effect”），并且在执行 DOM 更新之后调用它。
- 需要清除的副作用
    如果你的 effect 返回一个函数，React 将会在执行清除操作时调用它：

