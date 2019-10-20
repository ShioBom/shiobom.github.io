---
title: React.setState使用中需要注意的地方
date: 2019-08-05 15:29:50
tags: 
    - react
    - setState
---

## 不要滥用setState

由于setState每调用一次，就会触发页面的重新渲染。所以在使用过程中需要注意以下几点。

1. **与渲染无关的状态尽量不要放在state中来管理**

    这样就能保证setState改变的状态都是和渲染有关的状态，就能避免不必要的重复渲染。你可以用React performance tools中的printWasted来查看什么时候会发生不必要渲染

2. **避免不必要的修改**
    当state的值没有发生改变的时候，尽量不要使用setState。虽然shouldComponentUpdate和PureComponent可以避免不必要的重复渲染，但是还是增加了一层shallowEqual的调用，造成多余的浪费。

3. **并不是所有的组件状态都应该用setState来进行保存和更新的**
    复杂的组件可能会有各种各样的状态需要管理。用setState来管理这些状态不但会造成很多不需要的重新渲染，也会造成相关的生命周期钩子一直被调用

## setState并不保证同步，但在某些情况下是同步的。

1. **如果需要在setState后直接获取修改后的值**

    - 方式一：直接传入参数的值，而不是通过this.state调用。

    ```js
    this.setState({
    selection: value
    })
    this.fireOnSelect(value)
    ```

    - 方式二：通过传入回调函数，它会在setState完成以后调用。

    ```js
    this.setState({
    selection: value
    }, this.fireOnSelect(this.state.selection))
    ```
2. setState同步执行的情况
    - 在setTimeOut中