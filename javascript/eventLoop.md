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

### 例子

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

* Event loop
 每一轮Event Loop时，都会将“任务队列”中需要执行的任务，一次执行完。setTimeout和setInterval都是把任务添加到“任务队列”的尾部。因此，它们实际上要等到当前脚本的所有同步任务执行完，然后再等到本次Event Loop的“任务队列”的所有任务执行完，才会开始执行。由于前面的任务到底需要多少时间执行完，是不确定的，所以没有办法保证，setTimeout和setInterval指定的任务，一定会按照预定时间执行。

* setTimeout
  setTimeout和setInterval的运行机制是，将指定的代码移出本次执行，等到下一轮Event Loop时，再检查是否到了指定时间。如果到了，就执行对应的代码；如果不到，就等到再下一轮Event Loop时重新判断。这意味着，setTimeout指定的代码，必须等到本次执行的所有代码都执行完，才会执行。

* setTimeout(fn, 0)
  setTimeout(fn, 0)将第二个参数设为0，作用是让f在现有的任务（脚本的同步任务和“任务队列”中已有的事件）一结束就立刻执行。也就是说，setTimeout(f,0)的作用是，尽可能早地执行指定的任务。

> 执行结果 1 5 3 4 2

* 上面代码的执行结果说明，setTimeout(fn, 0)在Promise.resolve之后执行。这是因为setTimeout语句指定的是“正常任务”，即不会在当前的Event Loop（事件循环）执行。而Promise会将它的回调函数，在状态改变后的那一轮Event Loop（事件循环）指定为微任务。所以，3和4输出在5之后、2之前。

* setTimeout(fn, 0)的一大应用是，可以调整事件的发生顺序。比如，网页开发中，某个事件先发生在子元素，然后冒泡到父元素，即子元素的事件回调函数，会早于父元素的事件回调函数触发。如果，我们先让父元素的事件回调函数先发生，就要用到setTimeout(fn, 0)。  
