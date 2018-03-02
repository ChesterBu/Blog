# 原型链、继承

## new 的过程发生了什么？

```js
function A(name){
  this.name = name
}
let a = new A('hehe')

-----------------
let obj = new Object()

obj.__proto__ = A.prototype

A.call(obj, 'hehe')

return obj

```

- 用new Object() 的方式新建了一个对象 obj
- 将 obj 的原型指向构造函数，这样 obj 就可以访问到构造函数原型中的属性
- 使用 call，改变构造函数 this 的指向到新建的对象，这样 obj 就可以访问到构造函数中的属性
- 返回 obj

## 原型链和prototype属性

上面的 __proto__ 是什么？

就是原型链，原型链是内部 [ [Prototype ]]，指向它“父类”的prototype。

打开浏览器控制台，可以看到函数变量都有一个prototype属性（箭头函数没有）。

通过这一句a.__proto__ = A.prototype; 说明a的原型链就是指向函数A的prototype属性。

这里就有一个重要的认识了，虽然名字很像，但是原型链并不是prototype属性，同时原型链指向“父类”的prototype。几乎所有对象都有原型链（除了null和undefined），通过__proto__ 可以看到原型链指向什么（当然最好使用 Object.getPrototypeOf 取原型链）

通过实验可以发现，js中对象的链可以非常复杂。
一图胜千言。这里借两张图。

![prototype](https://github.com/ChesterBu/Blog/blob/master/img/jsImg/prototype.png?raw=true)
![prototype2](https://github.com/ChesterBu/Blog/blob/master/img/jsImg/prototype2.png?raw=true)

简而言之

- 函数对象，Function，Object，Array等等的原型链都指向Function.prototype

- 通过new操作符创建的对象原型链指向原来的函数的prototype属性

- Object.prototype属性的原型链指向null（到null就停止了）

- 而Function.prototype（Array，Date，String等等），以及函数对象的prototype，原型链都指向Object.prototype

所以，原型链一共就分为两类：

- 原型指向Function.prototype的函数们
- 原型指向Object.prototype的对象们

## constructor

- 所有函数的prototype.constructor都指向自己
- 因此所有new出来的对象也都有一个reference找到自己的构造函数

## 小结

- 每个对象都有__proto__属性，但只有函数对象才有prototype属性

- 创建函数时，js会为函数自动添加prototype属性，它的值是空对象，而把函数当作构造函数调用时就会启动new的过程

- 每个js对象一定对应一个原型对象，并从原型对象上继承属性和方法（此处涉及设计模式中原型模式的内容）

- 对象的__proto__属性的值就是他所对应的原型对象

- 所以__proto__才是原型链

## 继承

- [JavaScript深入之继承的多种方式和优缺点](https://github.com/mqyqingfeng/Blog/issues/16)