# js中的Function 

js是函数式编程，在js中，占有很重要的地位，本章就来讲讲函数。在js既然内置函数也有我们编写的函数。
看下面的例子:

```
//这就是内置函数
var a = parseInt('1');//输出 a = 1

//这就是我们自己编写的函数
function f(){
	console.log('a');//控制台输出a
}

```

上面的例子就是函数的基本形式。
下面就来系统的介绍函数。

* 内置函数

在js中，内置函数大致有下面这些：

```
//parseInt函数，把一个对象转化成为一个整数
var a = parseInt('1');//输出a = 1;

//parseFloat函数，把一个对象转化为一个浮点数
var a = parseFloat('1.1');//输出a = 1.1

//isNaN()//判断一个变量是否非数字
var a  = 'a';
var b = isNaN(a);//true

//isFinited()//判断一个变量是否无限大
var a = 1;
var b = isFinited(a);//false

//encodeURI
//encodeURI() 是对统一资源标识符（URI）进行编码的方法。它使用1到4个转义序列来表示每个字符的UTF-8编码（只有由两个代理字符区组成的字符才用四个转义字符编码）。
encodeURI("@baidu.com");//输出'@baidu.com'

//encodeURIcomponent
//encodeURIComponent()是对统一资源标识符（URI）的组成部分进行编码的方法。它使用一到四个转义序列来表示字符串中的每个字符的UTF-8编码（只有由两个Unicode代理区字符组成的字符才用四个转义字符编码）
encodeURIComponent('@baidu.com');//输出'%40baidu.com'

//从上面的例子可以看到encodeURI方法和encodeURIComponent方法之间的不同在于,encodeURIComponent会对字符串中的特殊字符进行转码，这是因为当目标字符串作为uri连接中的一部分是，这些特殊字符有着特别的意义，例如"?"，在uri中就带表一个查询字符,但是我们的本意"?"只是作为一个文本，并不是作为一个查询字符，所以就需要对其进行转码。

//同理decodeURI和decodeURIComponent就是对字符集进行反转码
//两者的不同之处与encodeURI和encodeURIComponent之间的不同之处是一样的，后者会对部分特殊字符进行反转码，而前者则不会

//eval方法
//eval方法的作用就是执行参数文本代表的js语句
//js一般用作动态生成函数，但是过度使用eval()方法会对性能造成影响
eval('a = 1');
console.log(a);//控制台输出1

//alert方法
//弹出警告框
alert('a');//浏览器弹出'a'的警告
```

* 自定义函数

在js中，我们可以这样定义函数：

```

var f = function(){};

//或者像下面那样
function f(){}
```

* 函数自执行

```
//在下面的代码中定义完就会自动执行
//自执行函数的结构一般如下(function(){})();
(function(arg){
	console.log(arg);
})('arguments');
//控制台输出'arguments'

//利用自执行函数我们可以创建块级作用域
(function(){
	var a = 1;
})();
console.log(a);
//控制台输出undefined
//像上面那样，我们就创建了一个块级作用域,代码块外的作用域无法方法代码块中的变量
```

* 回调函数

```

function f(a,callback){
	cabllback(a);
}
f('a',function(a){
	console.log(a);
});
//控制台输出'a'

```

* 自调用函数

函数还可以调用自身

```
var a = 1;
function a(){
	if(a < 5){
		return a();
	}
}
a();
//函数执行完毕后，a = 5;因为这个函数调用了自身5次

//如果函数式一个匿名函数怎么办呢？
var a = 1;
(function(){
	if(a < 5){
		return arguments.callee();
	}
})();
//同样，函数执行完毕后，a = 5
//调用函数自身可以用arguments.callee方法，这个方法在碰上匿名函数时适用
```

* constructor属性
//在函数的原型链中，会有一个constructor属性
//我们可以看看这个属性的用法

```
var a = [];
console.log(a.constructor);//控制台输出array 

//一般情况下，constructor属性会指向构造这个对象的方法

function Man(){
	this.sex = 'male'
}
var xiaoMing = new Man();
console.log(xiaoming.constructor);//控制台输出[Function Man]

//上面的代码似乎是说明xiaoMing这个对象是由Man函数生成的。

//再看看下面的代码

function SuperHero(){}
function Man(){}
SuperHero.prototype = new Man();

var xiaoMing = new SuperHero();
console.log(xiaoMing.constructor);//控制台输出Man

//从上面的代码可以看到小明的构造函数是SuperHero,但是constructor属性却指向Man
//所以，不能简单以为contructor就是对象的构造函数

//constructor属性存在于函数的prototype属性中，这一点是要记住的
//我们可以用constructor属性来构造新的对象

function Man(){
	this.sex = 'male';
}
var xiaoMing = new Man();
var xiaoGang = new xiaoMing.constructor();//利用constructor属性，我们构造了一个新的对象。

```

## [作用域与闭包，请点这里](./closure.md)

