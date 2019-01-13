# JS数据类型

## 基本数据类型(大小固定，放在栈里)

### number

### string

### boolean

### null

- typeof null ---> object

### undefined

### symbol

----

## 引用数据类型(大小并不固定。存在堆里)

### object（函数类，数据结构类）

#### 作为对象

- Math
- Reflect

#### 作为构造函数

- Function

- Date
- RegExp

- Number
- Boolean
- String

- Proxy

- Set,WeakSet
- Map,WeakMap

#### 即作为对象，又作为构造函数

- Promise
- Object
- Array

#### 即作为对象，又作为函数

- Symbol(因为symbol值不是new出来的)

----

## 堆栈

- 栈：程序运行时系统分配的一小块内存，大小在编译期时由编译器参数决定。
- 堆：可以理解为当前可以使用的空闲内存，其大小是需要代码编写的人员自己去申请和释放。(在 JS 中，V8 下有自动垃圾回收机制不需要我们自己操作)

## JS万物皆对象？

```js
var str1 = "oujm";
var str2 = str1.substring(2);
//为什么str1能调用方法？
```

String ，Number ，Boolean

ECMAScript提供了这三个特殊的引用类型，这三个引用类型和其他的引用类型相似，但同时也具有于各自的基本类型相应的特殊行为，实际上，每当读取一个基本类型的时候，后台就会创建一个对应的基本包装类型的对象。

再来看上面那个?，str1 很明显是一个基本类型实例，问题就出在 str1.substring(2) 字符串怎么会有方法。其实，为了让我们更好的操作基本类型的实例对象，后台进行了一系列的操作：

1. 创建String的实例
2. 在实例上调用指定的方法
3. 销毁这个实例

```js
var tempStr = new String("oujm");
var str2 = tempStr.substring(2);
tempStr = null;
```

从这里能够看到，一般的引用类型和包装类型唯一的区别就在于对象的生命周期。包装类型的对象生命周期很短，只有代码执行的一瞬间，然后就被销毁了，所以这也就是为什么我们不能在运行的时候为基本类型的值添加属性和方法。

```js
var str1 = "oujm";
var str1.bf = "ethan";
console.log(str1.bf); // undefined

```

so

```js
var str1 = "oujm";
var str2 = new String("ethan");

console.log(str1.__proto__ === str2.__proto__); // true
console.log(str1 instanceof String); // false
console.log(str2 instanceof String); // true
```

由此我们可以知道，引用类型和基本包装对象的区别在于：生存期
引用类型所创建的对象，在执行的期间一直在内存中，而基本包装对象只是存在了一瞬间。
这样做的目的是为了效率，比如把整数封装为object再进行数学运算会慢上很多倍。
