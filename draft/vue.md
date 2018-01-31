# VUE

- vue

![自带指令](https://github.com/ChesterBu/Blog/blob/master/img/vueImg/vue.png?raw=true)

## vue 指令

- vue指令

![自带指令](https://github.com/ChesterBu/Blog/blob/master/img/vueImg/vdirective.png?raw=true)

- 自定义指令

![自定义指令](https://github.com/ChesterBu/Blog/blob/master/img/vueImg/directive.png?raw=true)

## v-model的简单实现

```html
<input type="text" id="input">
<script>
    let i = document.querySelector('#input');
    let obj = {};
    let temp = {}; 
    Object.defineProperty(obj,'name',{
        get(){  // 取obj值时触发
            return temp.name;
        },
        set(val){  //设置obj值时触发，且会调用get
            temp.name = val;
            i.value = val;
        }
    })
    i.addEventListener('input',function(){
        obj.name = this.value;
    })
</script>

```

这里加了temp变量是因为如果直接给obj设置值的话代码会写为

```js
set(val){
    obj.name = val
}
```

obj.name本质就是调用set方法，从而导致循环调用所以要加个temp临时变量。

## vue 的生命周期

- 生命周期

![生命周期](https://github.com/ChesterBu/Blog/blob/master/img/vueImg/lifecycle.png?raw=true)

- 钩子函数

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

## vue API

![API](https://github.com/ChesterBu/Blog/blob/master/img/vueImg/vueAPI.png?raw=true)
