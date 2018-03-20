# JS深浅拷贝与Jquery深浅extend

## 数组的浅拷贝

- 如果是数组，我们可以利用数组的一些方法比如：slice、concat 返回一个新数组的特性来实现拷贝。

```js
var arr = ['old', 1, true, null, undefined];

var new_arr = arr.concat();
//var new_arr = arr.slice();
new_arr[0] = 'new';

console.log(arr) // ["old", 1, true, null, undefined]
console.log(new_arr) // ["new", 1, true, null, undefined]

```

## 奇计实现对象数组深拷贝

```js
var arr = ['old', 1, true, ['old1', 'old2'], {old: 1}]

var new_arr = JSON.parse( JSON.stringify(arr) );

console.log(new_arr);

```

- 是一个简单粗暴的好方法，就是有一个问题，不能拷贝函数

## 对象浅拷贝

```js
        let shallowCopy = function (obj) {
            if (typeof obj !== 'object') return
            let newObj = obj instanceof Array ? [] : {};
            for (let key in obj) {
                if (obj.hasOwnProperty(key)) {
                    newObj[key] = obj[key]
                }
            }
            return newObj
        }
```

## 对象深拷贝

```js
        let deepCopy = function (obj) {
            if (typeof obj !== 'object') return
            let newObj = obj instanceof Array ? [] : {};
            for (let key in obj) {
                if (obj.hasOwnProperty(key)) {
                    newObj[key] = typeof obj[key] === 'object' ? deepCopy(obj[key]) : obj[key]
                }
            }
            return newObj
        }
```

## 对象浅extend

```js
        function extend() {
            let target = arguments[0];
            for (let i = 1; i < arguments.length; i++) {
                let options = arguments[i];
                if (options != null) {
                    for (let name in options) {
                        let copy = options[name];
                        if (copy != undefined) {
                            target[name] = copy
                        }
                    }
                }
            }
        }
```

## 对象深extend

```js
        function isObject(data) {
            return Object.prototype.toString.call(data) === '[object Object]'
        }

        function deepExtend(deep, ...source) {
            let obj = {}
            for (let i = 0; i < source.length; i++) {
                if (!isObject(source[i])) {
                    console.error("Function[extend] parmas must be Object")
                    return false
                }
                for (let key in source[i]) {
                    if (deep === true && isObject(source[i][key])){
                        obj[key] = deepExtend(deep,source[i][key])
                        continue
                    }
                    if (source[i].hasOwnProperty(key)) {
                        obj[key] = source[i][key]
                    }
                }
            }
            return obj
        }
        var obj1 = {
            a: 1,
            b: {
                c: 2
            }
        }

        var obj2 = {
            b: {
                c: {
                    f:10
                },

            },
            d:{
                e:1
            }
        }

        var d = deepExtend(true, obj1, obj2)
        obj2.d.e = 2
        console.log(d);
```