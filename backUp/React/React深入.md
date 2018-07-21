# React深入

## setState是同步还是异步？

- setState 只在合成事件和钩子函数中是“异步”的，在原生事件和setTimeout 中都是同步的。
- setState 的“异步”并不是说内部由异步代码实现，其实本身执行的过程和代码都是同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形成了所谓的“异步”，当然可以通过第二个参数 setState(partialState, callback) 中的callback拿到更新后的结果。
- setState 的批量更新优化也是建立在“异步”（合成事件、钩子函数）之上的，在原生事件和setTimeout 中不会批量更新，在“异步”中如果对同一个值进行多次setState，setState的批量更新策略会对其进行覆盖，取最后一次的执行，如果是同时setState多个不同的值，在更新时会对其进行合并批量更新。

## React生命周期

### V15

- MOUNTING，页面首次进入，渲染触发的生命周期是：

- - 初始化
- - componentWillMount
- - render
- - componentDidMount

- UPDATING，实例化后，当props或者state发生变化时，下面方法依次被调用：

- - componentWillReceiveProps
- - shouldComponentUpdate
- - componentWillUpdate
- - render
- - componentDidUpdate

- UNMOUNTING，销毁阶段，只有一个函数被调用：

- - componentWillUnmount

### V16
