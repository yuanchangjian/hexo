---
title: Event Loop
date: 2020-06-17 16:59:50
categories: 前端面试题
tags: Event Loop

---

# 简介

Event Loop是一个执行模型，在不同的地方有不同的实现。浏览器和NodeJS基于不同的技术实现了各自的Event Loop。下面是[Philip Roberts: Help, I'm stuck in an event-loop](https://vimeo.com/96425312)视频演讲中描述的模型，可能因为年代久远，并未涉及到宏任务和微任务的概念。我以前也是如此理解，但现在碰到宏任务和微任务的概念，所以重新梳理一下Event Loop的原理。

![image-20200617174852957](https://yuanchangjian.github.io/cloudImage/images/20200617174854.png)

<!-- more -->



# 宏任务和微任务

## 宏任务（MacroTask）

- setTimeout
- setInterval
- setImmediate (Node独有)
- requestAnimationFrame (浏览器独有)
- I/O
- UI rendering (浏览器独有)



## 微任务（MicroTask）

- process.nextTick (Node独有)
- Promise
- Object.observe(废弃)
- MutationObserver



## 执行顺序

![image-20200617175655711](https://yuanchangjian.github.io/cloudImage/images/20200617175657.png)

{% note danger %} 

微任务比宏任务优先级高，任务调度需先清空微任务，然后执行下一个宏任务。

 {% endnote %}

# 浏览器的Event Loop

![image-20200617180011157](https://yuanchangjian.github.io/cloudImage/images/20200617185017.png)

浏览器Event Loop有两个队列（也要考虑浏览器的实现，现在已成为业界的规范），一个宏任务队列，一个微任务队列。

举例说明：

```
console.log('script start');

setTimeout(function () {
  console.log('setTimeout');
}, 0);

Promise.resolve()
  .then(function () {
    console.log('promise1');
  })
  .then(function () {
    console.log('promise2');
  });

console.log('script end');

// 执行结果：
script start
script end
promise1
promise2
setTimeout
```

下面在提及一个知识点，UI渲染的时机，如下图所示，是执行完一轮循环，准备执行下一个宏任务时选择性的渲染（浏览器很聪明，会节省性能）

![image-20200617184951269](https://yuanchangjian.github.io/cloudImage/images/20200617184952.png)

{% note danger %} 

requestAnimationFrame是在UI渲染前调用，非常适合做动画。因为像setTimeout这种会当作一个宏任务执行，还可能被其他任务导致排队，这样便会出现丢帧现象。

 {% endnote %}





# NodeJS中的Event Loop

![image-20200617180211278](https://yuanchangjian.github.io/cloudImage/images/20200617185221.png)

各个阶段执行的任务如下：

- **timers阶段**：这个阶段执行setTimeout和setInterval预定的callback
- **I/O callback阶段**：执行除了close事件的callbacks、被timers设定的callbacks、setImmediate()设定的callbacks这些之外的callbacks
- **idle, prepare阶段**：仅node内部使用
- **poll阶段：获取新的I/O事件**，适当的条件下node将阻塞在这里
- **check阶段**：执行setImmediate()设定的callbacks
- **close callbacks阶段**：执行socket.on('close', ....)这些callbacks



**NodeJS中宏队列主要有4个**

由上面的介绍可以看到，回调事件主要位于4个macrotask queue中：

1. Timers Queue
2. IO Callbacks Queue
3. Check Queue
4. Close Callbacks Queue



**NodeJS中微队列主要有2个**：

1. Next Tick Queue：是放置process.nextTick(callback)的回调任务的
2. Other Micro Queue：放置其他microtask，比如Promise等



{% note danger %} 

注意点：

1. 微任务比宏任务优先级高
2. 微任务中先执行Next Tick Queue后执行Other Micro Queue
3. 宏任务与浏览器中不同，不是从队列中取一个执行，而是按照顺序执行队列的所有任务（宏队列顺序：Timers Queue > IO Callbacks Queue > Check Queue > Close Callbacks Queue）

 {% endnote %}





{% note danger %} 

Node poll阶段不太明白，先留个坑

 {% endnote %}





# 参考

* [Philip Roberts: Help, I'm stuck in an event-loop](https://vimeo.com/96425312)
* [带你彻底弄懂Event Loop](https://juejin.im/post/5b8f76675188255c7c653811)
* [Tasks, microtasks, queues and schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
* [Microtask and Macrotask: A Hands-on Approach](https://blog.bitsrc.io/microtask-and-macrotask-a-hands-on-approach-5d77050e2168)
* [The Node.js Event Loop, Timers, and process.nextTick()](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)
* [深入解析 EventLoop 和浏览器渲染、帧动画、空闲回调的关系](https://zhuanlan.zhihu.com/p/142742003)