# 对象基础

在js中，创建对象的方法有很多，既可以用js自带的函数构造器去创建对象，而已可以用构造函数或者是字面量去创造一个对象

* 通过内置的构造函数去构建对象

```

//通过构造函数创建对象
//String 构造函数
var string = new String('abc');
console.log(typeof string);//控制台输出'object'而不是'string'

//Boolean构造函数
var boolean = new Boolean(false);
console.log(typeof boolean);//控制台输出'object'而不是'boolean'
Boolean(boolean);//控制台输出true,因为boolean是一个object而不是一个boolean对象

//Object构造函数
var object = new Object();
console.log(typeof object);/控制台输出'object';

//Array构造函数
var array = new Array();
console.log(typeof array);//控制台输出'object';

//Function 构造函数
var f = new Function('a','b','console.log(a+b);');
f(1,2);//控制台输出3

//Number 构造函数
var number = new Number(11);
console.log(typeof number);//控制台输出'object'

//Date构造函数
var now = new Date();//获得现在的时间
var someday = new Date(2017,2,17);//生成一个2017年3月17日的日期对象
console.log(typeof someday);//控制台输出'object'

//正则表达式
var regular = /./; //匹配任意字符一个
regular.test('a');//控制台输出true
console.log(typeof regular);//控制台输出'object'

```

* 通过字面量的方式创建一个对象

```

var object = {};
var array = [];
var func = function(){};
``

上面的方式就是通过字面量的方式来创建对象，也是我们用得比较多方式。

* 通过构造函数的方式构建对象

```
function SuperHero(name){
	this.name = name;
}
var xiaoMing = new SuperHero('xiaoming');
console.log(xiaoMing.name);//控制台输出'xiaoming'

//在我们创建的xiaoming对象中有个constructor属性,这个属性指向创建他的函数。
console.log(xiaoMing.constructor);//在控制台打印一下，可以看到这个属性指向刚刚创建他的SuperHero函数

```

更多关于对象和对象原型链属性的知识会在以后的文章做介绍，对象的基础知识就先讲到这里。


