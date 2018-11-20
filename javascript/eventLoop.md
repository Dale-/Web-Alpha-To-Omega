## JavaScript 运行机制 -- Event Loop

### 堆、栈、队列
* 堆（heap）是指程序运行时申请的动态内存，在JS运行时用来存放对象。
* 栈（stack）遵循的原则是“现金后出”，JS中的基本数据类型与指向对象的地址存放在栈内存中，此外还有一块栈内存用来执行JS主线程--执行栈（execution context stack）。
* 队列（queue）遵循的原则是“先进后出”，JS中除了主线程外还存在一个“任务” 。


### 浏览器环境下的Event Loop
* 当主线程运行的时候，JS会产生堆和栈（执行栈）
* 主线程中调用的webapi所产生的异步操作（dom事件、ajax回调、定时器等）只要产生结果，就把这个回调塞进“任务队列”中等待执行
* 当主线程中的同步任务执行完毕，系统就会依次读取“任务队列”中的任务，将任务放进执行栈中执行
* 执行任务时可能还会产生新的异步操作，会产生新的循环，整个过程是循环不断的
![EventLoop](/source/img/javascript/event-loop.png)


```javascript

console.log(1)

setTimeout(() => {
  console.log(2);
}, 0)

Promise.resolve().then(() => {
  console.log(3)
}).then(() => {
  console.log(4)
})

console.log(5)

```
