# JS基本知识

今天回顾一下js的基本知识

1. 数据基本类型
在js中，数据基本类型有:number、string、undefined、null、boolean

```

var a = 1;
typeof a; //输出'number'

var b = 'China';
typeof b; //输出'string'

var c ;
typeof c ;//输出'undefined'

var d = null;
typeof d ;//输出'object',在js里面null代表一个空对象，所以还是对象

var e = true;
typeof e ;//输出'boolean'
```

2.js中的操作符

* 基本操作符

'*'代表的是相乘

```
var a = 1;
var b = 2;
var c = a*b;//输出c = 2

var a =2;
var b = 'stirng';
var c = a*b ;//输出NaN 数字与字符相乘会输出NaN

var a = 2;
var b = '1';
var c = a*b ;//输出2 数字与字符表示的数字相乘时，会将字符转化为数字再相乘
```

'/'代表的是除法

```
var a = 4;
var b = 2;
var c = a/b; //输出c = 2

var a = 4;
var b = 'string';
var c = a/b //输出NaN 数字与字符相除会输出NaN

var a = 4;
var b = '2';
var c = a/b //输出2 输出与字符表示的数字相除时，会将字符先转化为数字
```

'+'代表的是加法
```

var a = 1;
var b = 2 ;
var c = a + b; //输出c =3

var a = 1;
var b = 'string';
var c = a + b//输出 c = '1stirng' 当数字与字符相加时，会将数字转化为字符，然后进行拼接
```

'-'代表减法

```

var a = 2;
var b = 1;
var c = a - b;//输出c = 1

var a = 2;
var b = 'string';
var c = a - b;//输出NaN 数字与字符相减时，输出NaN

var a = 2;
var b = '1';
var c = a - b;//输出1 数字与字符表示的数字相减时，会先讲字符转化成数字再进行操作
```

* 赋值操作

在js中，赋值操作会用到'='

```

var a = 3;//将3赋给a
var b = a++ //将a的值赋予b，然后a自增1，所以b = 3
var b = ++a //将a自增，然后将a的值赋予b，所以b的值是4,也就是a自增1次后的值

var a = 1;
console.log(a++);//输出1，先输入a，a再自增1
var a = 1;
console.log(++a)//输出2,a自增，然后输出a，所以输出2

//自减操作也是同样的道理
var a =3;
var b = a--//输出3
var a =3;
var b = --a//输出2
var a = 1;
console.log(a--);//输出1
var a = 1;
console.log(--a);//输出0

```
除了'='赋值外，还有'*='、'/='、'+='、'-='、'%='这5个操作


```

// 这5个操作都是先操作再赋值
// *=操作 
var a = 2;
a *= 2;
console.log(a);//输出4

// /=操作
var a = 4;
a /= 4;
console.log(a);//输出1

// += 操作
var a = 1;
a += 2;
console.log(a);//输出2

//-= 操作
var a = 2;
a -= 1;
console.log(a);//输出1

//%=操作
var a = 4;
a %= 3;
console.log(a);//输出1，这里进行了取余操作
```

* 特殊操作

* typeof

typeof 操作符返回一个字符串，指示操作数的类型。在es6以前只会返回以下这几个结果：'string'、'number'、'undefined'、'boolean'、'object'、'function'。

```
var testString = 'abc';
console.log(typeof testString);//输出'String'

var testNumber = 1;
console.log(typeof testNumber);//输出'number'

var testUndefined;
console.log(typeof testUndefined);//输出'undefined'

var testBoolean = true;
console.log(typeof testBoolean);//输出'boolean'

var testObjcet = {};
console.log(typeof testObject);//输出'object'

var testFunction = function(){};
console.log(typeof testFunction);//输出'function'

* delete 操作符
testNumber = {a:1};
console.log(testNumber.a);//输出1
delete testNumber.a;
console.log(testNumber.a);//输出undefined

* 逻辑运算符

在js中，逻辑运算符有&&、||、!

* &&操作

```

//&&操作符代表的是且操作
t = true;
f = false;
//&&操作遵循以下规则
t&&f; // 输出false
f&&t; // 输出false
t&&t; // 输出true
f&&f; // 输出false

```

* ||操作符

```
// ||操作符代表的是或操作
t = true;
f = false;
// ||操作符遵循以下规则
t||f; //输出t
f||f; //输出f
f||t; //输出t
t||t; //输出t

```

* !操作符

```
// !操作符代表的是取反
var t = true;
var f = false;

!t;//输出false;
!f;//输出true;

```

* 比较运算符

js中的比较运算符有'=='、'==='、'!='、'!=='、'>='、'<='

```
//比较两个变量是否相等，不比较对象的类型
var a = 1;
var b = '1';
console.log(a == b);//输出true

//比较两个变量是否相等，同时比较对象的类型
console.log(a === b);//输出false

//比较两个对象是否不相等，不比较对象的类型
console.log(a != b);//输出false

//比较两个对象是否不相等，同时比较对象的类型
console.log(a !== b);//输出true

//比较变量之前的大小
//在js中，如果变量是字符串,会先将字符串转换成相应的ascii码再进行比较
var a = 'a';
var b = 'b';
console.log(a < b);//输出true

3. js中的语句

* switch 语句

```

//switch语句中一般要有switch、case(用于选择情况)、break(用于暂停函数)、default语句(用于匹配其他情况)
		function testSwitch(a){
				switch(a){
					case 1:
						console.log(1);
						break;
					case 2:
						console.log(2);
						break;
					default:
						console.log('u enter others value');
						break;
				}
			}
		testSwitch(2);//输出2
```

* do...while语句

```

//do..while语句语法结构如下
var i = 1;
do {
	i++;
	console.log(i);
}while(i < 5);

//控制台依次输出2、3、4、5
```

* while语句

```

// while语句语法如下
var i = 1;
while(i < 5){
	console.log(i);
	i++;
}
//依次输出1、2、3、4

```

* if..else 语句

```

var a = 1;
if(a === 1){
	console.log(1);
}
else if(a === 2){
	console.log(2);
}
else{
	console.log('other value');
}
```
