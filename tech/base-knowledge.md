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

