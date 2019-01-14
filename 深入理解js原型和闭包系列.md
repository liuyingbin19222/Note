### 深入理解js原型和闭包系列

- http://www.cnblogs.com/wangfupeng1988/p/4001284.html
- 对象:
    在js中，一切皆对象,面向对象编程中用类来描述对象,生成对象在js中可以用函数来生成:
`
function getOneAnimal(){
    var animal = {
        name:'tiger',
        size:'large'
    }
    return animal
}
`
- 可以任意的扩展扩展:
`var obj = {
    a:'john',
    b:function(name){
        return name
    },
    c:''
}`
访问可以用obj.a,obj.b来访问;

- 在对象里面,有一个引用类型和值类型
引用类型:函数,object,数组;值类型:String,Number,Booleean,Undefined,NUll
new() 对象创造的是引用类型,typeof检测值类型,instanceof 检测的是引用类型。
值类型变化的是值，引用类型变化的是引用,
`
    var a = new String(2);
    var b = a;
    function changeA(a){
        a = 'new value';
        return a;
    }
`
引用类型指的是同一个对象,当某一个指向使其发生变化,通过另一个指向得到的值也会发生变化。

- 对象是属性的集合,对象中的函数也属于属性。
- 函数的prototype对象,每一个函数都有一个prototype对象,默认的的是consrtuctor,可以增加函数的属性;

```
var fn = function (){
    return '1988';
}
fn.prototype.getYear() = function(year){
    return year;
}
```
这样fn的prototype就有两个属性,contructor和getYear();
这样利用fn创建对象,就可以调用getYear()方法;

var fn = new fn();
fn.getYear(2009);
- 对于instanceof, 他的作用是检测原型链, ```console.log(fn instanceof Function);  console.log(fn instanceof Object); ```
返回的都是true。
那该如何辨别？

- 关于原型链的属性访问，首先在基本属性中查找,没有的话,在在__proto__中查找，着一条路径就是原型链。原型链体现出了继承的味道,针对于判断当前的着一个属性到底是基本属性,还是prototype属性,可以用hasOwnPrototype()方法来确定,在Function.prototype中没有着一个方法，但function继承了Object,object.protoType中有着一个方法,所以此处体现了继承。在之前的代码练习中, function.prototype有很多好玩的属性,call,apply等等,此种属性有很多的妙处,可以玩tricky算法技巧。
- 利用原型的灵活应用的方面是可以随意添加原型的属性,制定符合自己的需求;
- 另一块内容,执行上下文环境,
基本形式如下:
变量声明----undefined;
函数声明----赋值;
this---赋值
这是执行上下文环境需要执行的工作;

- 不同的代码段有不同的上下文执行环境,在函数体中:
arguments是赋值,
this 是赋值
变量声明是赋值

- this有四种使用情况:
1.首先是全局情况:```console.log(this == window);返回的是true```

2. 在一个对象中:

```
var obj = {
    name:'liuyingbin',
    age:function(){
        console.log(this.name+'age is 21');
        return '21'
    }
}
```
this.name中this指的是obj这一个对象.   
3.函数被call或aply调用,this指的是传入对象的值.
4.构造函数中:
```
function getNewProject(){
    this.name = "yingbin";
    this.age = '21';
}
var fn = new getNewProject();
console.log(fn.name);

```
在构造函数中,使用new 一个对象，则this调用的是构造函数中的属性;

- 上下文环境,模式是一个上下文执行栈,首先是全局环境栈，在是函数栈,
```
var a,
    fn,
    bar = function(c){
        return c;
    }
    fn = function(b){
        const c = 8;
        return c + b;
    }
    bar(3);
```
全局环境 -> bar()执行环境入栈,fn()函数入栈,fn出栈,bar()函数出栈,最后是全局环境;

- 作用域:在js代码中,有全局作用域,函数作用域,变量的声明要在函数首部声明。js是没有块级作用域的,作用域是用来隔离变量的,防止变量污染。

- 作用域链,在当前作用域下,变量a未定义,则找全局作用域,未找到，则继续找函数作用域,未找到的话，显示undefined,寻找的路径就是作用域链。

- 闭包的两种形式:
函数作为返回值,函数作为参数。
```
function fn(){
    var max = 100;
    return function bar(x){
        if(x > max){
            console.log(x);
        }
    }
}
```
函数作为参数来传递:
```
(function(f){
  var max = 100;
  f(15);
}(fn));
```
这是max的值是10,而不是100，根据作用域链的定义,变量的值要在创建的作用域中寻找,而不是‘父作用域’中。








































































