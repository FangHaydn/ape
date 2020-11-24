javascript代码是单线程执行，宏观上分同步任务和异步任务。

异步任务可细分为宏任务（task queue）和微任务（macrotask queue）

所有的异步任务通过eventloop事件循环实现调度

![img](./images/eventloop-2.jpg)

![img](./images/eventloop-1.jpg)

[知乎解答](https://zhuanlan.zhihu.com/p/34229323)