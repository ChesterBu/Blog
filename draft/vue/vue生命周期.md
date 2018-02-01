# Vue生命周期

- 每个 Vue 实例在被创建之前都要经过一系列的初始化过程。例如需要设置数据监听、编译模板、挂载实例到 DOM、在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，给予用户机会在一些特定的场景下添加他们自己的代码。

## 生命周期

![生命周期](https://github.com/ChesterBu/Blog/blob/master/img/vueImg/lifecycle.png?raw=true)

## 钩子函数

![钩子函数](https://github.com/ChesterBu/Blog/blob/master/img/vueImg/lifecycle-hook.png?raw=true)

```js
beforeCreate
```

- 在实例初始化之后，数据观测(data observer) 和 event/watcher 事件配置之前被调用。

```js
created
```

- 实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。

```js
beforeMount
```

- 在挂载开始之前被调用：相关的 render 函数首次被调用。

```js
mounted
```

- el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。

```js
beforeUpdate
```

- 数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。 你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。

```js
updated
```

- 由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

- 当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态，因为这可能会导致更新无限循环。

- 该钩子在服务器端渲染期间不被调用。

```js
beforeDestroy
```

- 实例销毁之前调用。在这一步，实例仍然完全可用。

```js
destroyed
```

- Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。

## Vue.nextTick

- 在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

```js
Vue.nextTick(function () {
  // DOM 更新了
})
```

- 官方还提供了一种写法，vm.$nextTick，用 this 自动绑定到调用它的实例上

```js
created() {
    setTimeout(() => {
          this.number = 100
          this.$nextTick(() => {
            console.log('nextTick', document.getElementsByTagName('p')[0])
          })
    },100)
}

```

- 什么时候需要到Vue.nextTick()?
- - 在 Vue 生命周期的 created() 钩子函数进行的 DOM 操作一定要放在 Vue.nextTick() 的回调函数中。原因是什么呢，原因是在 created() 钩子函数执行的时候 DOM 其实并未进行任何渲染，而此时进行 DOM 操作无异于徒劳，所以此处一定要将 DOM 操作的 js 代码放进 Vue.nextTick() 的回调函数中。与之对应的就是 mounted 钩子函数，因为该钩子函数执行时所有的 DOM 挂载和渲染都已完成，此时在该钩子函数中进行任何DOM操作都不会有问题 。
- - 在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的 DOM 结构的时候，这个操作都应该放进 Vue.nextTick() 的回调函数中。

## 生命周期小结

生命周期钩子的一些使用方法：

- beforecreate : 可以在这加个loading事件，在加载实例时触发
- created : 初始化完成时的事件写在这里，如在这结束loading事件，异步请求也适宜在这里调用
- mounted : 挂载元素，获取到DOM节点
- updated : 如果对数据统一处理，在这里写上相应函数
- beforeDestroy : 可以做一个确认停止事件的确认框
- nextTick : 更新数据后立即操作dom

## 更多

- * [详解vue生命周期](http://www.cnblogs.com/penghuwan/p/7192203.html)
