1、调用栈顺序调用任务
2、当调用栈发现异步任务时，将异步任务交给其他模块处理，自己继续进行下面的调用
3、异步执行完毕，异步模块将任务推入任务队列，并通知调用栈
4、调用栈在执行完当前任务后，将执行任务队列里的任务
5、调用栈执行完任务队列里的任务之后，继续执行其他任务


事实上，事件循环决定了代码的执行顺序，从全局上下文进入函数调用栈开始，直到调用栈清空，然后执行所有的micro-task（微任务），当所有的micro-task（微任务）执行完毕之后，再执行macro-task（宏任务），其中一个macro-task（宏任务）的任务队列执行完毕（例如setTimeout 队列），再次执行所有的micro-task（微任务），一直循环直至执行完毕。

1、不同的任务会放进不同的任务队列之中。
2、先执行macro-task，等到函数调用栈清空之后再执行所有在队列之中的micro-task。
3、等到所有micro-task执行完之后再从macro-task中的一个任务队列开始执行，就这样一直循环。
4、宏任务和微任务的队列执行顺序排列如下：
5、macro-task（宏任务）：script(整体代码), setTimeout, setInterval, setImmediate（NodeJs）, I/O, UI rendering。
6、micro-task（微任务）: process.nextTick（NodeJs）, Promise, Object.observe(已废弃), MutationObserver(html5新特性)



链接：[事件执行顺序](https://juejin.im/post/5bc1adc45188255c82553921)
