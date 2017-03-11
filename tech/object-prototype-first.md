#object 与原型链（一）

今天我们来谈谈object吧。

在js中有6种主要类型，分别是string,number,boolean,null,undefined和object。object类型就是我们今天要谈的。但是object的内置对象又太多了，包括String,Number,Boolean,Object,Function和Array。因为是与原型链一起谈，所以我们就谈谈Object对象就好了。

##创建Object对象的方法

###利用Object内置的构造函数

```
  //利用内置构造函数构造对象创造一个空对象，再赋予属性


  var a = new Object();
 a.b = 1;
 a.c = 2;
... ...
//这样我们就创造了一个对象

```

###声明形式

```

var  a = { 'b':1, 'c':2};
//就像生命一个变量一样，我们构造了一个a对象
```

###利用构造函数构造对象

```

//首先创建一个构造函数
//具体的利用new 方法创建对象的过程会在后面原型链的知识中讲到

function man(){
  this.sex = 'male';
}
var xiaoming = new man();
//这样xiaoming就是man的一个示例，在xiaoming自身中，保存这sex属性

```

###利用defineProperty和defineProperties方法

```
//首先我们创建一个空对象
var a = {};
//然后我们利用defineProperty方法或者defineProperties方法去定义这个空
对象的属性

Object.defineProperty(a,'key',{
  value : 1,
  enumerable:true,
  configurable:true,
  writable:true
});

```

defineProperty方法第一个参数接受一个对象的名称，第二个参数接受一个键的名称，第三个参数接受一个对象。这个对象需要我们定义4个属性：value,enumerable,cofigurable,writable属性。

*configurable

当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，也能够被删除。默认为 false。

*enumerable

当且仅当该属性的 enumerable 为 true 时，该属性才能够出现在对象的枚举属性中。默认为 false。

*value

该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。

*writable

当且仅当该属性的 writable 为 true 时，该属性才能被赋值运算符改变。默认为 false。 

一般情况下，我们会定义上面4个属性，但是，你可以通过定义属性的setter和getter来定义属性，这时候就不需要定要writable和value属性了。

看下面的例子：

```

var a = {};
Object.defineProperty(a,'key',{
  get:function(){
    return this.a;
  },
  set:function(value){
    return this.a = value;
  },
  enumerable:true,
  configurable:true

});
a.key = 2;
console.log(a.key);//输出2
```

通过setter和getter我们定义了key属性的读和写。

如果需要一次定义多个属性嗯，可以利用Object.defineProperties方法。


