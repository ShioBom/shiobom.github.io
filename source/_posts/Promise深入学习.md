---
title: Promise深入学习
date: 2019-07-15 09:05:45
tags: ES6
---
## Promise 用途
- 回调地狱问题
- 回调函数执行多次的问题  
promise只resolve一次，剩下的会被忽略
- 回调函数不执行的问题  
promise.race 能解决,下面的实例还没太了解
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
- 回调函数有时同步执行，有时异步执行的问题
promise.then() 永远是异步执行的，它是微任务  
[宏任务与微任务的概念可以看这篇文章了解](https://juejin.im/post/59e85eebf265da430d571f89?tdsourcetag=s_pctim_aiomsg)