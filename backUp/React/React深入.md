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

render函数执行以后，DOM结构已经生成，此时在componentDidMount这个周期中可以有些DOM 上的操作。页面渲染完成后，执行完componentDidMount周期，从此以后，这四个周期就结束了。只要不是初次渲染，这几个周期就不在执行了。接下来的周期才是react重点

- UPDATING，实例化后，当props或者state发生变化时，下面方法依次被调用：

- - componentWillReceiveProps,每当我们通过父组件更新子组件props时（这个也是唯一途径），这个方法就会被调用。
- - shouldComponentUpdate，字面意思，是否应该更新组件，默认返回true。当返回false时，后期函数就不会调用，组件不会在次渲染。
- - componentWillUpdate
- - render
- - componentDidUpdate

![update](https://github.com/ChesterBu/Blog/blob/master/img/reactImg/lifecycle2.jpg?raw=true)

- UNMOUNTING，销毁阶段，只有一个函数被调用：

- - componentWillUnmount，每当组件使用完成后，这个组件就需要从DOM中销毁，此时该方法就会被调用。当我们在组件中使用了setInterval，那我们就需要在这个方法中调用clearTimeout。

![lifecycle](https://github.com/ChesterBu/Blog/blob/master/img/reactImg/lifecycle1.jpg?raw=true)

- setState的时机

- - componentWillMount、componentDidMount、componentWillReceiveProps、componentDidUpdate

- 其他生命周期方法都不能调用setState，主要原因有两个：

- - 产生死循环。例如，shouldComponentUpdate、componentWillUpdate 和 render 中调用setState，组件本次的更新还没有执行完成，又会进入新一轮的更新，导致不断循环更新，进入死循环。

- - 无意义。componentWillUnmount 调用时，组件即将被卸载，setState是为了更新组件，在一个即将卸载的组件上更新state显然是无意义的。实际上，在componentWillUnmount中调用setState也是会抛出异常的。

### V16

![lifecycle](https://github.com/ChesterBu/Blog/blob/master/img/reactImg/lifecycleV16.png?raw=true)

#### 新的生命周期函数

- **static getDerivedStateFromProps(nextProps, prevState)**

- 这个函数会在组件被实例化或接收到新的 props 时被调用。函数返回一个对象用于更新 state 以响应 props 变化。 如果返回值为null 则表明不改变 state 。

- 如果函数返回一个对象，这个对象的键将被合并到现有 state 中。

- **getSnapshotBeforeUpdate(prevProps, prevState)**

- 另外一个生命周期函数，援引从git hook来的叫法，“预提交阶段（pre-commit phase）”，调用时机在VDOM的改动实施到真实DOM之前。

- 它在我们需要读取当前DOM的状态的时候很有用。比如说，我们有一个聊天应用，新的消息会被添加到屏幕顶部。当用户正在向下滚动的时候，一个新的消息被添加到屏幕顶部，新消息就被滚动出屏幕可见区域了。这样的交互是很糟的。如果有了getSnapshotBeforeUpdate， 那我们可以计算屏幕当前滚动的位置 ，并且作出调整（比如屏幕的滚动位置）。

- 即使这个方法并不是静态的，仍然推荐返回一个值，而不是直接修改组件。它的返回值会作为componentDidUpdate的第三个参数。

