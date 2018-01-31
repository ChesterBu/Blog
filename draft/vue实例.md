# Vue 实例

- 在文档中经常会使用 vm 这个变量名表示 Vue 实例，在实例化 Vue 时，需要传入一个选项对象，它可以包含数据(data)、模板(template)、挂载元素(el)、方法(methods)、生命周期钩子(lifecyclehook)等选项。

## data

- Vue 实例的数据都保存在 data 对象中，Vue 将会递归将 data 的属性转换为 getter/setter，从而让 data 的属性能够响应数据变化。

```js

let  vm = new Vue({
  data: {
      a;1
  }
})
vm.a // -> 1
vm.$data  // data

```

- 这样数据就绑定在 HTML 中，Vue 框架监视 data 的数据变化，自动更新 HTML 内容。

## methods

- methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例。

```js

let vm = new Vue({
  data: { a: 1 },
  methods: {
    plus: function () {
      this.a++
    }
  }
})
vm.plus()
vm.a // 2

```

## computed

- 计算属性将被混入到 Vue 实例中。所有 getter 和 setter 的 this 上下文自动地绑定为 Vue

```js
let vm = new Vue({
  data: { a: 1 },
  computed: {
    // 仅读取，值只须为函数
    aDouble: function () {
      return this.a * 2
    },
    // 读取和设置
    aPlus: {
      get: function () {
        return this.a + 1
      },
      set: function (v) {
        this.a = v - 1
      }
    }
  }
})
vm.aPlus   // -> 2
vm.aPlus = 3
vm.a       // -> 2
vm.aDouble // -> 4

```

## watch

- 一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。
- vue实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个属性。

```js
let vm = new Vue({
  data: {
    a: 1,
    b: 2,
    c: 3
  },
  watch: {
    // 监控a变量变化的时候，自动执行此函数
    a: function (val, oldVal) {
      console.log('new: %s, old: %s', val, oldVal)
    },
    // 深度 watcher
    c: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true
    }
  }
})
vm.a = 2 // -> new: 2, old: 1

```

## watch和computed区别

- 从属性名上，computed是计算属性，也就是依赖其它的属性计算所得出最后的值.watch是去监听一个值的变化，然后执行相对应的函数。

- 从实现上，computed的值在getter执行后是会缓存的，只有在它依赖的属性值改变之后，下一次获取computed的值时才会重新调用对应的getter来计算。watch在每次监听的值变化时，都会执行回调。其实从这一点来看，都是在依赖的值变化之后，去执行回调。很多功能本来就很多属性都可以用，只不过有更适合的。如果一个值依赖多个属性（多对一），用computed肯定是更加方便的。如果一个值变化后会引起一系列操作，或者一个值变化会引起一系列值的变化（一对多），用watch更加方便一些。

- watch的回调里面会传入监听属性的新旧值，通过这两个值可以做一些特定的操作。computed通常就是简单的计算。

- watch和computed并没有哪个更底层，watch内部调用的是vm.$watch，它们的共同之处就是每个定义的属性都单独建立了一个Watcher对象。

- 更多：* [谈Vue的依赖追踪系统 ——搞懂methods watch和compute的区别和联系](http://www.cnblogs.com/penghuwan/p/7194133.html)

## filiters

- 过滤器本质就是数据在呈现之前先进行过滤和筛选。

- Vue.js 允许你自定义过滤器，被用作一些常见的文本格式化。过滤器应该被添加在 mustache 插值的尾部，由“管道符”指示：

```js
<!-- in mustaches -->
{{ message | capitalize }}
<!-- in v-bind -->
<div v-bind:id="rawId | formatId"></div>

Vue 2.x 中，过滤器只能在 mustache 绑定和 v-bind 表达式（从 2.1.0 开始支持）中使用，因为过滤器设计目的就是用于文本转换。为了在其他指令中实现更复杂的数据变换，你应该使用计算属性。

过滤器函数总接受表达式的值作为第一个参数。
new Vue({
  // ...
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
过滤器可以串联：
{{ message | filterA | filterB }}
过滤器是 JavaScript 函数，因此可以接受参数：
{{ message | filterA('arg1', arg2) }}
这里，字符串 'arg1' 将传给过滤器作为第二个参数， arg2 表达式的值将被求值然后传给过滤器作为第三个参数。

```

