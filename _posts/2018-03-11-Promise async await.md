---
layout: post
title:  "Promise async await"
date:   2018-03-11
categories: Javascript
tags: Promise async await
---

## 摘要

这篇文章主要讲述 Promise 、async-await 的使用。详细的解析可以搜索阮一峰的 ES6 教程。

Promise 是异步编程的解决方案。相比传统直接使用回调函数的方式要更加易读，易写。

async-await 是 Promise 和 generator 的语法糖。只是为了让我们的代码更易写，易读。

## Promise

使用 `new` 来调用 `Promise` ，返回的是一个 `Promise` 对象：

```js
let p1 = new Promise((resolve, reject) => {
   //异步处理
   //处理结束，判断调用 resolve，还是 reject ，当调用其中一个的时候，后面的代码不再执行
});
```

设置回调函数使用 `then` 方法：

```js
p1.then(func1,func2);//func1,func2 分别对应 resolve, reject 所调用的回调函数
```

不过通常使用 `then` 来处理 `resolve` ，`catch` 来处理 `reject` ，这样看起来更清晰

```javascript
p1.then(func1).catch(func2);
```

多个请求并行，全部请求成功时执行某些代码，可用 `Promise.all()` 方法：

多个请求并行，有一个请求成功时执行某些代码，可用 `Promise.race()` 方法：

```js
function createPromise(time, str) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, time, str);
    });
}
let p1 = createPromise(600,'第一个');
let p2 = createPromise(400,'第二个');
let p3 = createPromise(1500,'第三个');
Promise.all([p1,p2,p3]).then(data => {
    console.log(`${data[0]}${data[1]}${data[2]}`);//第一个第二个第三个
});
Promise.race([p1,p2,p3]).then(data => {
    console.log(data);//第一个
});
```

还可以通过 `Promise.resolve()` 、`Promise.reject()` 来使用 `Promise` 。



## async-await

`async` 用来表示函数是异步的，定义的函数会返回一个 Promise 对象，可以使用 then 等方法添加回调函数。

`await` 可以理解为 `async await` 的简写。`await` 必须出现在 `async` 函数内部，不能单独使用。

`await` 后面可以跟任何 JS 表达式。不过主要是用于等待 Promise 对象的状态变为请求成功。 

```js
function createPromise(time, str) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, time, str);
    });
}
async function count() {
    let p1 = await createPromise(600,'第一个');
    console.log(p1);
}
count();//第一个
let p2 = createPromise(700,'第二个');
p2.then(data => {
    console.log(data);
});//第二个
```

上述的情况就是 `async-await` 与 `then` 都能实现同样的功能。

再举个需要相互依赖的请求，例如说，有三个小球，第1个小球先动，后面的小球被碰到的时候，才会开始动。

```js
function moveTheBall(ball, distance, interval) {
    return new Promise( (resolve, reject) => {
        distance += ball.offsetLeft;
        let timer = setInterval( () => {
            let ballLeft = parseInt(ball.offsetLeft,10);
            if (ballLeft < distance) {
                ball.style.left = ballLeft + 10 + 'px';
            } else {
                clearInterval(timer);
                resolve(true);
            }
        },interval);
    });
}
//then
let balls = document.querySelectorAll('.ball');
moveTheBall(balls[0],480,10)
    .then(() =>{
    return moveTheBall(balls[1],280,20);
})
    .then(() => {
    return moveTheBall(balls[2],300,50);
});
//async-await
async function move(balls, distances, intervals) {
    let p1 = await moveTheBall(balls[0],distances[0],intervals[0]);
    let p2 = await moveTheBall(balls[1],distances[1],intervals[1]);
    let p3 = await moveTheBall(balls[2],distances[2],intervals[2]);
}
move(balls,[480,280,300],[10,20,30]);
```

总的来说，就是有些场景，用 `Promise` 就可以了。而有些场景再结合 `async-await` 会让代码更清晰，易懂。



