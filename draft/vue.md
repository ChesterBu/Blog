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

## vue API

![API](https://github.com/ChesterBu/Blog/blob/master/img/vueImg/vueAPI.png?raw=true)
