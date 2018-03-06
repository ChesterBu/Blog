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

## 闭包

1. 一个没有闭包的编程语言可以用对象来模拟闭包。
1. 一个没有对象的编程语言可以用闭包来模拟对象。

- 思考：

```js
function person(name,age) {
    return happyBirthday(){
        age++;
        console.log(
            "Happy " + age + "th Birthday, " + name + "!"
        );
    }
}

var birthdayBoy = person( "Kyle", 36 );

birthdayBoy();      // Happy 37th Birthday, Kyle!

var birthdayBoy = {
    name: "Kyle",
    age: 36,
    happyBirthday() {
        this.age++;
        console.log(
            "Happy " + this.age + "th Birthday, " + this.name + "!"
        );
    }
};

birthdayBoy.happyBirthday();
// Happy 37th Birthday, Kyle!

```

- 闭包和对象是状态的同构表示（及其相关功能）。(同构：下次你听到谁说 “X 与 Y 是同构的”，他们的意思是，“X 和 Y 可以从两者中的任意一方转化到另一方，并且无论怎样都保持了相同的特性。”)

## 函子

- Functor 是 可以被 map over 的类型.

- 平常我们可以把 a 到 b 的映射可以叫做 map, 映射的方式就是函数了.
- 那么类似的对于函数或者其他可以做这种 map 操作的类型或一种计算方式, 叫做 Functor.
- 而这种 map 就叫做 fmap, 给定 a 集合到 b 集合的映射方式(也就是一个函数), 就能找到 对 a 的一种计算(computation, 任何可变换的类型, 这就是 Functor) 的变换 -- 对 b 的对应计算方式.
- 如果该计算是一个函数, 那么这个操作叫做 lifting. 非常形象的, 根据 a 到 b 的映射 lift(举) 到另一个层面上.

```js
函子的函数签名是这个样子
myFunctor :: (a -> b) -> f a -> f b
意思是“给我一个传入a返回b的函数和一个包含a（一个或多个）的容器，我会返回一个包含b（一个或多个）的容器”
```

- Functor 的作用就是应用一个函数到一个上下文中的值：

## 递归

- 所谓递归，是当一个函数调用自身，并且该调用做了同样的事情，这个循环持续到基本条件满足时，调用循环返回。
- 警告： 如果你不能确保基本条件是递归的 终结者，递归将会一直执行下去，并且会把你的项目损坏或锁死；恰当的基本条件十分重要！

## Monad(单子)

- Monad的这种抽象方式是为了简化程序中不确定状态的判断而提出的，能够让程序员从更高的层次顺序描述程序逻辑的每一个动作，而不必关注每一个动作是否会出现异常，也不必关注第一个动作内是否需要逻辑判断，是否要跳转。

- Monad是函数式编程中的一种抽象数据类型，其特别之处在于，它是用来表示计算而不是数据的。在以函数式风格编写的程序中，单子可以用来组织包含有序操作的过程，或者用来定义任意的控制流（比如处理并发、异常、延续）。单子的构造包括定义两个操作bind和return，还有一个必须满足若干性质的类型构造器M。

- 作为 Monad, 它还必须具备一个方法 '>>='(这个符号好眼熟的说, 看看 haskell 的 logo, 你就知道 Monad 是有多重要), 也就是 bind 方法.bind 方法的意思很简单, 就是给这个盒子加一个操作, 比如往盒子在加放射性原子,如果猫活着,就是绿巨猫, 如果猫是死的,那就是绿巨死猫.

- Monad 应用的是一个接收一个普通值但是返回一个在上下文中的值的函数：

- - return：它的作用是将一个值 a 放入上下文中。（unit）
- - '>>='：函数的功能则是应用一个（接收一个普通值 a 但是返回一个在上下文中的值 m b 的）函数 (a -> m b) 到一个上下文中的值 m a ，并返回另一个在相同上下文中的值 m b 。（）
