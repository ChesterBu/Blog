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

## 纯函数

- 纯函数是这样一种函数，即相同的输入，永远会得到相同的输出，而且没有任何可观察的副作用。

## 副作用可能包括

- 副作用是在计算结果的过程中，系统状态的一种变化，或者与外部世界进行的可观察的交互。

- 副作用可能包含，但不限于：
- - 更改文件系统
- - 往数据库插入记录
- - 发送一个 http 请求
- - 可变数据
- - 打印/log
- - 获取用户输入
- - DOM 查询
- - 访问系统状态
- - ...

- 概括来讲，只要是跟函数外部环境发生的交互就都是副作用——这一点可能会让你怀疑无副作用编程的可行性。函数式编程的哲学就是假定副作用是造成不正当行为的主要原因。

## 追求“纯”的理由

### 可缓存性（Cacheable）

- 首先，纯函数总能够根据输入来做缓存。实现缓存的一种典型方式是 memoize 技术：

```js
var memoize = function(f) {
  var cache = {};
  return function() {
    var arg_str = JSON.stringify(arguments);
    cache[arg_str] = cache[arg_str] || f.apply(f, arguments);
    return cache[arg_str];
  };
};
```

- 值得注意的一点是，可以通过延迟执行的方式把不纯的函数转换为纯函数：

```js
var pureHttpCall = memoize(function(url, params){
  return function() { return $.getJSON(url, params); }
});
```

- 这里有趣的地方在于我们并没有真正发送 http 请求——只是返回了一个函数，当调用它的时候才会发请求。这个函数之所以有资格成为纯函数，是因为它总是会根据相同的输入返回相同的输出：给定了 url 和 params 之后，它就只会返回同一个发送 http 请求的函数。

### 可移植性／自文档化（Portable / Self-Documenting）

- JavaScript 的设定中，可移植性可以意味着把函数序列化（serializing）并通过 socket 发送。也可以意味着代码能够在 web workers 中运行。总之，可移植性是一个非常强大的特性。
- 命令式编程中“典型”的方法和过程都深深地根植于它们所在的环境中，通过状态、依赖和有效作用（available effects）达成；纯函数与此相反，它与环境无关，只要我们愿意，可以在任何地方运行它。
- 你上一次把某个类方法拷贝到新的应用中是什么时候？我最喜欢的名言之一是 Erlang 语言的作者 Joe Armstrong 说的这句话：“面向对象语言的问题是，它们永远都要随身携带那些隐式的环境。你只需要一个香蕉，但却得到一个拿着香蕉的大猩猩...以及整个丛林”。

### 可测试性（Testable）

### 合理性（Reasonable）

### 并行代码

- 最后一点，也是决定性的一点：我们可以并行运行任意纯函数。因为纯函数根本不需要访问共享的内存，而且根据其定义，纯函数也不会因副作用而进入竞争态（race condition）。
- 并行代码在服务端 js 环境以及使用了 web worker 的浏览器那里是非常容易实现的，因为它们使用了线程（thread）。不过出于对非纯函数复杂度的考虑，当前主流观点还是避免使用这种并行。

## 高阶函数

- 函数是可以接受并且返回任何类型的值。一个函数如果可以接受或返回一个甚至多个函数，它被叫做高阶函数。

## 组合函数

- 在数学中， f ∘ g 是函数的组合，所以读作 “函数 f 与函数 g 的复合函数”，或者更通用的说法是 “在 g 之后调用 f”。所以 (f ∘ g)(x) 相当于先以 x 为自变量调用函数 g，再以结果为自变量调用函数 f，简写成 f(g(x))。

- 接受两个函数的:

```js
function compose2(fn2,fn1){
    return function composed(origValue){
        return fn2(fn1(origValue))
    }
}

```

- 通用型:

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

## 递归

- 所谓递归，是当一个函数调用自身，并且该调用做了同样的事情，这个循环持续到基本条件满足时，调用循环返回。
- 警告： 如果你不能确保基本条件是递归的 终结者，递归将会一直执行下去，并且会把你的项目损坏或锁死；恰当的基本条件十分重要！

## 容器

- 首先我们将创建一个容器（container）。这个容器必须能够装载任意类型的值；否则的话，像只能装木薯布丁的密封塑料袋是没什么用的。这个容器将会是一个对象，但我们不会为它添加面向对象观念下的属性和方法。是的，我们将把它当作一个百宝箱——一个存放宝贵的数据的特殊盒子。

```js
var Container = function(x) {
  this.__value = x;
}

Container.of = function(x) {
    return new Container(x);
};
```

- 们将使用 Container.of 作为构造器（constructor），这样就不用到处去写糟糕的 new 关键字了，非常省心。实际上不能这么简单地看待 of 函数，但暂时先认为它是把值放到容器里的一种方式。

在继续后面的内容之前，先澄清几点：

- Container 是个只有一个属性的对象。尽管容器可以有不止一个的属性，但大多数容器还是只有一个。我们很随意地把 Container 的这个属性命名为 __value。
- __value 不能是某个特定的类型，不然 Container 就对不起它这个名字了。
- 数据一旦存放到 Container，就会一直待在那儿。我们可以用 .__value 获取到数据，但这样做有悖初衷。

## 范畴

很多函数式编程的术语都源于范畴学，而范畴学的实质即是组合。初看范畴学，就像初次进行高台跳水或者乘坐过山车，慌张，恐惧，但是并不难完成。你只需明确下面几个范畴学基础要点：

- 一个范畴（category）是一个容纳了一系列对象及对象间箭头（->）的集合。
- 箭头只是形式上的描述，实际上，箭头代表了态射（morphismms）。在编程中，态射可以被认为是函数。
- 对于任何被箭头相连接的对象，如 a -> b -> c，必须存在一个 a -> c 的组合。
- 所有的箭头表示都代表了组合（即便这个对象间的组合只是一个同一（identity）箭头：a->c）。所有的对象都存在一个同一箭头，即存在同一态射（a -> a）。

## 函子

- 一旦容器里有了值，不管这个值是什么，我们就需要一种方法来让别的函数能够操作它。

```js
Container.prototype.map = function(f){
  return Container.of(f(this.__value))
}
```

- 这个 map 跟数组那个著名的 map 一样，除了前者的参数是 Container a 而后者是 [a]。它们的使用方式也几乎一致：

```js
Container.of(2).map(function(two){ return two + 2 })
//=> Container(4)

Container.of("flamethrowers").map(function(s){ return s.toUpperCase() })
//=> Container("FLAMETHROWERS")

Container.of("bombs").map(concat(' away')).map(_.prop('length'))
//=> Container(10)
```

- 为什么要使用这样一种方法？因为我们能够在不离开 Container 的情况下操作容器里面的值。这是非常了不起的一件事情。Container 里的值传递给 map 函数之后，就可以任我们操作；操作结束后，为了防止意外再把它放回它所属的 Container。这样做的结果是，我们能连续地调用 map，运行任何我们想运行的函数。甚至还可以改变值的类型，就像上面最后一个例子中那样。

- 等等，如果我们能一直调用 map，那它不就是个组合（composition）么！这里边是有什么数学魔法在起作用？是 functor。各位，这个数学魔法就是 functor。

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

- Functor 的作用就是应用一个函数到一个上下文中的值;

## 薛定谔的 Maybe

- 说实话 Container 挺无聊的，而且通常我们称它为 Identity，与 id 函数的作用相同（这里也是有数学上的联系的，我们会在适当时候加以说明）。除此之外，还有另外一种 functor，那就是实现了 map 函数的类似容器的数据类型，这种 functor 在调用 map 的时候能够提供非常有用的行为。现在让我们来定义一个这样的 functor。

```js
var Maybe = function(x) {
  this.__value = x;
}

Maybe.of = function(x) {
  return new Maybe(x);
}

Maybe.prototype.isNothing = function() {
  return (this.__value === null || this.__value === undefined);
}

Maybe.prototype.map = function(f) {
  return this.isNothing() ? Maybe.of(null) : Maybe.of(f(this.__value));
}
```

- Maybe 看起来跟 Container 非常类似，但是有一点不同：Maybe 会先检查自己的值是否为空，然后才调用传进来的函数。这样我们在使用 map 的时候就能避免恼人的空值了

## “纯”错误处理

- 说出来可能会让你震惊，throw/catch 并不十分“纯”。当一个错误抛出的时候，我们没有收到返回值，反而是得到了一个警告！抛错的函数吐出一大堆的 0 和 1 作为盾和矛来攻击我们，简直就像是在反击输入值的入侵而进行的一场电子大作战。有了 Either 这个新朋友，我们就能以一种比向输入值宣战好得多的方式来处理错误，那就是返回一条非常礼貌的消息作为回应。我们来看一下：

```js
var Left = function(x) {
  this.__value = x;
}

Left.of = function(x) {
  return new Left(x);
}

Left.prototype.map = function(f) {
  return this;
}

var Right = function(x) {
  this.__value = x;
}

Right.of = function(x) {
  return new Right(x);
}

Right.prototype.map = function(f) {
  return Right.of(f(this.__value));
}

```

- Left 就像是青春期少年那样无视我们要 map 它的请求。Right 的作用就像是一个 Container（也就是 Identity）。这里强大的地方在于，Left 有能力在它内部嵌入一个错误消息。

- 假设有一个可能会失败的函数，就拿根据生日计算年龄来说好了。的确，我们可以用 Maybe(null) 来表示失败并把程序引向另一个分支，但是这并没有告诉我们太多信息。很有可能我们想知道失败的原因是什么。用 Either 写一个这样的程序看看：

```js
var moment = require('moment');

//  getAge :: Date -> User -> Either(String, Number)
var getAge = curry(function(now, user) {
  var birthdate = moment(user.birthdate, 'YYYY-MM-DD');
  if(!birthdate.isValid()) return Left.of("Birth date could not be parsed");
  return Right.of(now.diff(birthdate, 'years'));
});

getAge(moment(), {birthdate: '2005-12-12'});
// Right(9)

getAge(moment(), {birthdate: 'balloons!'});
// Left("Birth date could not be parsed")
```

- 通俗点来讲，一个函数在调用的时候，如果被 map 包裹了，那么它就会从一个非 functor 函数转换为一个 functor 函数。我们把这个过程叫做 lift。一般情况下，普通函数更适合操作普通的数据类型而不是容器类型，在必要的时候再通过 lift 变为合适的容器去操作容器类型。这样做的好处是能得到更简单、重用性更高的函数，它们能够随需求而变，兼容任意 functor。

## Endofunctors（自函子）

- 一个 endofunctor（自函子）是一个能将一个范畴映射回相同范畴的 functor。

- 一个 functor 能够完成任意范畴间映射: F a -> F b

- 一个 endofunctor 能够完成相同范畴间的映射：F a -> F a

## Functor小结

Functor 是能够对其进行 map 操作的对象。更进一步地，一个 functor 能够将一个范畴映射到另一个范畴。一个 functor 甚至可以将某一范畴映射回相同范畴（例如 endofunctor）。

## Applicative

- 数据类型 f 是 Applicative的前提条件是它必须是一个 Functor 。
- 在 Applicative typeclass 中定义了两个函数：
- - pure ：将一个值 a 放入上下文中；
- - <*>) ：将一个在上下文中的函数 f (a -> b) 应用到一个在上下文中的值 f a ，并返回另一个在上下文中的值 f b 。
- Applicative 的作用则是应用一个上下文中的函数到一个上下文中的值：

## Monad(单子)

- Monad的这种抽象方式是为了简化程序中不确定状态的判断而提出的，能够让程序员从更高的层次顺序描述程序逻辑的每一个动作，而不必关注每一个动作是否会出现异常，也不必关注第一个动作内是否需要逻辑判断，是否要跳转。

- Monad是函数式编程中的一种抽象数据类型，其特别之处在于，它是用来表示计算而不是数据的。在以函数式风格编写的程序中，单子可以用来组织包含有序操作的过程，或者用来定义任意的控制流（比如处理并发、异常、延续）。单子的构造包括定义两个操作bind和return，还有一个必须满足若干性质的类型构造器M。

- 作为 Monad, 它还必须具备一个方法 '>>='(这个符号好眼熟的说, 看看 haskell 的 logo, 你就知道 Monad 是有多重要), 也就是 bind 方法.bind 方法的意思很简单, 就是给这个盒子加一个操作, 比如往盒子在加放射性原子,如果猫活着,就是绿巨猫, 如果猫是死的,那就是绿巨死猫.

- Monad 将一个 “接受一个普通值并回传一个被封装的值” 的函数应用到一个被封装的值上，这一任务由函数 >>= （读作 “bind”）完成。

- Monad 应用的是一个接收一个普通值但是返回一个在上下文中的值的函数：
- - 要成为 Monad 的前提条件是它必须是 Applicative
- - return：它的作用是将一个值 a 放入上下文中。（unit）
- - '>>='：函数的功能则是应用一个（接收一个普通值 a 但是返回一个在上下文中的值 m b 的）函数 (a -> m b) 到一个上下文中的值 m a ，并返回另一个在相同上下文中的值 m b 。

- 链式操作只是 Monad 为我们带来的主要好处之一；另一个本文并未涉及到的主要好处是，Monad 可以为我们自动处理上下文，而我们只需要关心真正的值就可以了。

## 小结

- Functor ：使用 fmap 应用一个函数到一个上下文中的值；
- Applicative ：使用 <*> 应用一个上下文中的函数到一个上下文中的值；
- Monad ：使用 >>= 应用一个接收一个普通值但是返回一个在上下文中的值的函数到一个上下文中的值。

## 何为自函子上的幺半群？

```js
promises/A is a Monad
//
// To be a Monad, it must provide at least:
// - A unit (aka return or mreturn) operation that creates a corresponding
//   monadic value from a non-monadic value.
// - A bind operation that applies a function to a monadic value
//
// And it must satisfy the 3 Monadic Laws:
// 1. unit a >>= f == f
// 2. m >>= unit == m
// 3.(m >>= f) >>= g == m >>= (\x -> f x >>= g)
```

### 自函子

- 函子即所谓的 Functor，是一个能把值装在里面，通过传入函数来变换容器内容的容器：简化的理解里，前文中的 Promise.resolve 就相当于这样的映射，能把任意值装进 Promise 容器里。而自函子则是【能把范畴映射到本身】的 Functor，可以对应于 Promise(A).then() 里仍然返回 Promise 本身。

### 幺半群

- 幺半群即所谓的 Monadic，满足两个条件：单位元与结合律。

- 单位元是这样的两个条件：

- - 首先，作用到单位元 unit(a) 上的 f，结果和 f(a) 一致：

```javascript
const value = 6
const f = x => Promise.resolve(x + 6)

// 下面两个值相等
const left = Promise.resolve(value).then(f)
const right = f(value)
```

- - 其次，作用到非单位元 m 上的 unit，结果还是 m 本身：

```javascript
const value = 6

// 下面两个值相等
const left = Promise.resolve(value)
const right = Promise.resolve(value).then(x=> Promise.resolve(x))
```

- 至于结合律则是这样的条件：(a • b) • c 等于 a • (b • c)：

```javascript
const f = a => Promise.resolve(a * a)
const g = a => Promise.resolve(a - 6)

const m = Promise.resolve(7)

// 下面两个值相等
const left = m.then(f).then(g)
const right = m.then(x => f(x).then(g))

```
