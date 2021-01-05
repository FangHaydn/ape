javascript代码是单线程执行，宏观上分同步任务和异步任务。

异步任务可细分为宏任务（macrotask queue）和微任务（microtask queue）

所有的异步任务通过eventloop事件循环实现调度:

step1：主线程读取JS代码，此时为同步环境，形成相应的堆和执行栈;

step2: 主线程遇到异步任务，指给对应的异步进程进行处理（WEB API）;

step3: 异步进程处理完毕（Ajax返回、DOM事件处罚、Timer到等），将相应的异步任务推入任务队列；

step4：主线程查询任务队列，执行microtask queue，将其按序执行，全部执行完毕；

step5：主线程查询任务队列，执行macrotask queue，取队首任务执行，执行完毕；

step6：重复step4、step5。


![pic-1](./images/eventloop-2.jpg)

![pic-2](./images/eventloop-1.jpg)

[知乎解答](https://zhuanlan.zhihu.com/p/34229323)


Vue的nextTickets()就是使用Promise中then实现微任务