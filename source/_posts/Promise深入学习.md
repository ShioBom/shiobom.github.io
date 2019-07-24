---
title: ES6-Promise深入学习和Generator自动执行
date: 2019-07-15 09:05:45
tags: ES6 promise
---
## Promise
### Promise 用途
1.回调地狱问题
2.回调函数执行多次的问题,promise只resolve一次，剩下的会被忽略
3.回调函数不执行的问题 ,promise.race 能解决

```js
    function timeoutPromise(delay) {
        return new Promise( function(resolve,reject){
            setTimeout( function(){
                reject( "Timeout!" );
            }, delay );
        } );
    }

    Promise.race( [
        foo(),
        timeoutPromise( 3000 )
    ] )
    .then(function(){}, function(err){});
```
4.回调函数有时同步执行，有时异步执行的问题,promise.then() 永远是异步执行的，它是微任务  
[宏任务与微任务的概念可以看这篇文章了解](https://juejin.im/post/59e85eebf265da430d571f89?tdsourcetag=s_pctim_aiomsg)

### promise 嵌套  

```js
    Promise.all([loadSomething(), loadAnotherthing()])
    .then(function ([something, another]) {
        DoSomethingOnThem(...[something, another]);
    });
 ```
### 断开的 promise 链  

```js
    function anAsyncCall() {
        var promise = doSomethingAsync();
        return promise.then(function() {
            somethingComplicated()
        });
    }
```
    
### 混乱的集合 
```js
    // bad
    function workMyCollection(arr) {
        var resultArr = [];
        function _recursive(idx) {
            if (idx >= resultArr.length) return resultArr;

            return doSomethingAsync(arr[idx]).then(function(res) {
                resultArr.push(res);
                return _recursive(idx + 1);
            });
        }

        return _recursive(0);
    }
```
你可以写成  
```js
    function workMyCollection(arr) {
        return Promise.all(arr.map(function(item) {
            return doSomethingAsync(item);
        }));
    }
```
### 捕获错误的两种方式
- catch(cb)  
```js
    somethingAsync()
    .then(function() {
        return somethingElseAsync();
    })
    .catch(function(err) {
        handleMyError(err);
    });
```
- then(null,cb)
```js
    somethingAsync
    .then(function() {
        return somethingElseAsync()
    })
    .then(null, function(err) {
        handleMyError(err);
    });
```

### Promise的局限性
**（1） 错误被吃掉**
Promise 内部的错误不会影响到 Promise 外部的代码，而这种情况我们就通常称为 “吃掉错误”。因此promise链中的错误很容易被忽略。
**（2） 只能传递单一值**
通常使用解构赋值来传递多个值，封装对象或数组，再在then中解封赋值。
**（3） Promise 一旦新建它就会立即执行，无法中途取消。**
**（4） 无法得知 pending 状态**  
当处于 pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）
### 练习题
**红绿灯问题**  题目：红灯三秒亮一次，绿灯一秒亮一次，黄灯2秒亮一次；如何让三个灯不断交替重复亮灯？（用 Promise 实现）
### 自己实现一个简单的 Promise  
[参考文章](https://juejin.im/post/5a30193051882503dc53af3c)

```js
function Promise2(fn){
    var _this = this;
  
  // 用来保存then传入的回调函数
  this.callback = undefined;
  
  function resolve (val) {
    _this.callback && _this.callback(val);
  }
  
  fn(resolve);
}
Promise2.prototype.then = function (cb) {
    this.callback = cb;
};
function timeout(ms) {
    return new Promise2((resolve) => {
        setTimeout(resolve, ms);
    });
}

timeout(3000).then(() => {
    console.log('我3秒后被输出');
});
```

## Generator自动执行
### 原理
 yield 的时候返回一个 Promise 对象，给这个 Promise 对象添加 then 方法，当异步操作成功时执行 then 中的 onFullfilled 函数，onFullfilled 函数中又去执行 g.next，从而让 Generator 继续执行，然后再返回一个 Promise，再在成功时执行 g.next，然后再返回……
### 简单的启动器函数

```js
// 实现功能： 自动执行 Generator 返回的 Promise 对象， 并且将 Promise 的结果在传入到 Generator 进行下一次运行。
var fetch = require('node-fetch');
//启动器函数
function autoRunGen(gen) {
    var g = gen();
    function next(data){
        var result = g.next(data);

        if (result.done) return;
        result.value.then(function(data) {
            next(data);
        });
    }
    next();
}

function* fetchStepGen(){
    var url = 'https://api.github.com/users/github';
    var result = yield fetch(url);
    var jsonData = yield result.json();
    console.log(jsonData.bio); // "How people build software."
}

autoRunGen(fetchStepGen);
```

### co模块
用于函数的自动执行
[原博客文章](https://github.com/mqyqingfeng/Blog/issues/98)
