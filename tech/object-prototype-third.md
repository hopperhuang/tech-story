#object 与原型链（三）

前面两章讲述了object对象的创建、属性的枚举和遍历，在第三章我们来讲一下对象的关联和原型链。

##一个对象被创建就会关联原型链属性。

比如:

var a = {a:1}, 这个对象就会关联Object的原型链属性。
var a =[1,2], 这个数组就会关联Array的原型链属性。
var a = 'abc'，这个字符就会关联String的原型链属性。

除了继承关联对象的原型链外，对象还可以关联其他对象的原型链。

看下面的代码：

```

function man(){
  this.sex = 'male';
}
man.prototype.legs = 4;

var xiaoming = new man();

console.log(xiaoming.proto); //输出man { legs: 4 }

console.log(xiaoming instanceof man );// true 
console.log(xiaoming instanceof Object) // true 
```

可以看到xiaoming 对象既关联了man.prototype 也 关联了Object.prototype

##既然知道了object可以关联原型链，那么怎么创建关联呢？

* 利用构造函数

继续利用上面的例子：

```

function man(){
  this.sex = 'male';
}
man.prototype.legs = 4;

var xiaoming = new man();

```

我们解析一下整个对象创建的过程

1. 首先这个构造函数会创建一个空对象；
2. 将函数内的this指向这个空对象，相关属性赋值；
3. 为这个对象关联构造函数的prototype属性。
4. 检测构造函数内是否又return 语句，有则返回return 语句后的对象，没有则返回这个刚创建的对象。

```

console.log(xiaoming);//man { sex: 'male' }
console.log(xiaoming.proto); //  man { legs: 4 }
```

可以看到xiaoming对象本身保存这sex属性，原型链中则与man的原型链观关联起来了。

* Object.create()方法

利用Object.create()方法，可以将对象的原型链，关联到一个对象。

看下面的代码：

```

var man = {
 sex:'male',
 legs:4
};

var xiaoming = Object.create(man);
console.log(xiaoming); // {}
console.log(xiaoming.proto); //{ sex: 'male', legs: 4 }
```

可以看到xiaoming对象的proto属性指向man；

Object.create()方法的原理就是，创建一个对象，并将对象的原型链质量目标对象。

##判断对象与构造函数之间的关联

有时候我们需要判断对象和构造函数之间的关联。通常我们会用到 instanceof方法。

看下面的代码：

```

var f = function(){

};

var f2 =function(){

}

f.prototype.a = 1;

f2.prototype = Object.create(f.prototype);

var a = new f2();

console.log(a instanceof f2);//true
console.log(a instanceof f); // true
```

instanceof 方法回答的问题是，a 对象的原型链上 是否关联了 f构造函数的prototype 属性。instanceof 有局限性，它仅仅可以用来判断对象与构造函数之间的关联。

在es6中，js还为我们提供了 Object.prototype.isPrototypeOf方法，用于判断一个对象，是否在另一个对象的原型链上面。

看下面的代码：

```

var a = {
  a:1
}
var b = Object.create(a);
var c = Object.create(b);
console.log(b.isPrototypeOf(c)); //true
console.log(a.isPrototypeOf(c)); //true

```
