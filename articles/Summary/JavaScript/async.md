# 异步

## 同步与异步

同步和异步关注的是消息通知机制

- 同步就是发出调用后，没有得到结果之前，该调用不返回，一旦调用返回，就得到返回值了。 简而言之就是调用者主动等待这个调用的结果

- 而异步则相反，调用者在发出调用后这个调用就直接返回了，所以没有返回结果。换句话说当一个异步过程调用发出后，调用者不会立刻得到结果，而是调用发出后，被调用者通过状态、通知或回调函数处理这个调用。

## 阻塞与非阻塞

阻塞和非阻塞关注的是程序在等待调用结果（消息，返回值）时的状态.

- 阻塞调用是指调用结果返回之前，当前线程会被挂起。调用线程只有在得到结果之后才会返回。

- 非阻塞调用指在不能立刻得到结果之前，该调用不会阻塞当前线程。

## 组合

同步异步取决于被调用者，阻塞非阻塞取决于调用者

- 同步阻塞
- 异步阻塞
- 同步非阻塞
- 异步非阻塞

## eventloop

![eventloop](https://github.com/ChesterBu/Blog/blob/master/img/Summary/JS/eventloop.png?raw=true)

## 宏任务与微任务

![job](https://github.com/ChesterBu/Blog/blob/master/img/Summary/JS/job.jpeg?raw=true)
![job2](https://github.com/ChesterBu/Blog/blob/master/img/Summary/JS/job2.jpeg?raw=true)

