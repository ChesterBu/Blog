# 函数式编程

## 声明式编程和命令式编程有什么区别？

先统一一下概念，我们有两种编程方式：命令式和声明式。
我们可以像下面这样定义它们之间的不同：

- 命令式编程：命令“机器”如何去做事情(how)，这样不管你想要的是什么(what)，它都会按照你的命令实现。

- 声明式编程：告诉“机器”你想要的是什么(what)，让机器想出如何去做(how)。

正好对应过程式（pp）面向对象（oop）和函数式（fp）三种

- pp就是我，第一人称，我怎么做，每一步其实都可以翻译成独立的一套指令，对机器最为友好，机器最喜欢这种范式了，人怎么做机器就怎么做，所以这是指令式编程
- oop就是第三人称，他，他怎么做，oop需要先找到主语，然后才是谓语动词，java里面所有的方法都要被放在一个具体的class里面，用的时候，都要先找到这个主语，也就是class/object，名词，然后才能用这个名词的动作
- fp就是第二人称，你，你去做，fp讲究把代码全部封装成函数，函数就是动词，一个纯粹只有动词的语句就是祈使句，就是命令句式，这种句式一般是很不礼貌的，居高临下的，骂人基本上都是这种句式，所以使用这种句式的时候，请注意委婉表达，最好加上称谓否则很容易翻脸，很多国人初到国外时候，外语不太好，操起动词就说
- 所以fp宣称自己是命令式编程，而pp和oop是指令式编程，指令式最典型的就是pp了，oop介于两者之间，但是oop一般不claim自己是指令式还是命令式，oop最大特点是封装，虽然fp也封装，但是oop的封装是封装成对象，就特别容易理解

[知乎：声明式编程和命令式编程有什么区别？](https://www.zhihu.com/question/22285830)

## 高阶函数

- 函数是可以接受并且返回任何类型的值。一个函数如果可以接受或返回一个甚至多个函数，它被叫做高阶函数。

## 组合函数

- 在数学中， f ∘ g 是函数的组合，所以读作 “函数 f 与函数 g 的复合函数”，或者更通用的说法是 “在 g 之后调用 f”。所以 (f ∘ g)(x) 相当于先以 x 为自变量调用函数 g，再以结果为自变量调用函数 f，简写成 f(g(x))。

- 接受两个函数的

```js
function compose2(fn2,fn1){
    return function composed(origValue){
        return fn2(fn1(origValue))
    }
}

```

- 通用型

```js
function compose(...fns) {
    return function composed(result){
        // 拷贝一份保存函数的数组
        var list = fns.slice();
        while (list.length > 0) {
            // 将最后一个函数从列表尾部拿出
            // 并执行它
            result = list.pop()( result );
        }
        return result;
    };
}

function compose(...fns) {
    return function composed(result){
        return fns.reverse().reduce( function reducer(result,fn){
            return fn( result );
        }, result );
    };
}

```

- 但是，这种实现局限处在于外层的组合函数（也就是，组合中的第一个函数）只能接收一个参数。其他大多数实现在首次调用的时候就把所有参数传进去了。如果组合中的每一个函数都是一元的，这个方案没啥大问题。但如果你需要给第一个调用传递多参数，那么你可能需要不同的实现方案。

```js
function compose(...fns) {
    return fns.reverse().reduce( function reducer(fn1,fn2){
        //每次都将composed当作fn1再传进去，再包
        return function composed(...args){
            return fn2( fn1( ...args ) );
        };
    } );
}
```

## 柯里函数

- 柯里函数是一种一次只接收单个参数的函数。

## 偏函数

- 偏函数严格来讲是一个减少函数参数个数（arity）的过程；这里的参数个数指的是希望传入的形参的数量。

```js
function partial(fn,...presetArgs) {
    return function partiallyApplied(...laterArgs){
        return fn( ...presetArgs, ...laterArgs );
    };
}
```

## 柯里化

- 接收多个实参的函数拆解成连续的链式函数（chained functions），每个链式函数接收单一实参（实参个数：1）并返回另一个接收下一个实参的函数。
- 柯里化将一个多参数（higher-arity）函数拆解为一系列的单元链式函数。

```js
function curry(fn,arity = fn.length){
    return (function nextCurried(prevArgs){
        return function curried(nextArg){
             let args = prevArgs.concat([nextArg])
             if(args.length >= arity){
                 return fn(...args)
             } else {
                 return nextCurried(args)
             }

        }
    })([]);
}
```

- 松散柯里化允许你传入超过形参数量（arity，原函数确认或指定的形参数量）的实参。如果你将函数的参数设计成可配的或变化的，那么松散柯里化将会有利于你。例如，如果你将要柯里化的函数接收 5 个实参，松散柯里化依然允许传入超过 5 个的实参（curriedSum(1)(2,3,4)(5,6)），而严格柯里化就不支持 curriedSum(1)(2)(3)(4)(5)(6)。

```js
function looseCurry(fn,arity = fn.length) {
    return (function nextCurried(prevArgs){
        return function curried(...nextArgs){
            let args = prevArgs.concat( nextArgs );

            if (args.length >= arity) {
                return fn( ...args );
            }
            else {
                return nextCurried( args );
            }
        };
    })( [] );
}

```

- 反柯里化:你也会遇到这种情况：拿到一个柯里化后的函数，却想要它柯里化之前的版本 —— 这本质上就是想将类似 f(1)(2)(3) 的函数变回类似 g(1,2,3) 的函数。

```js
function uncurry(fn) {
    return function uncurried(...args){
        let ret = fn;
        for (let i = 0; i < args.length; i++) {
            ret = ret( args[i] );
        }

        return ret;
    };
}
```
