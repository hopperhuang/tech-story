# object 与原型链（二）

在（一）中，我们将到对象的创建。在本文，我打算讲一下对象属性的枚举与遍历。

在探讨遍历之前，我们首先要知道对象的属性存在什么地方。在js中，对象的属性存在两个地方，第一个是对象自身，第二个是对象的原型链中。看下面的代码：

```

function Man(){
  this.sex = 'male';
}
Man.prototype.legs = 2;

var xiaoming = new Man();

console.log(xiaoming.sex); //输出male
console.log(xiaoming.legs);//输出2
console.log(xiaoming.proto)//Man { legs: 2 }
```

上面的代码通过看到对对象的属性，既存在与对象自身，也存在与对象的原型链当中。

然后，我们还要知道，属性是分为可枚举属性和不可楣举属性的。请看下面的代码：

```

var xiaoming = {};
Object.defineProperties(xiaoming,{
  sex:{
      value:'male',
      configurable:true,
      writable:true,
      enumerable:true
    },
  legs:{
      value:4,
      enumberable:false,
      configurable:true,
      writable:true
    }
});

console.log(Object.keys(xiaoming)); //输出[sex]
console.log(xiaoming.legs);//输出4

```

可以见到，Object.keys()方法返回的数组中只有sex而没有legs，xiaoming对象中存在legs这个属性，只不过不可枚举而已，所以Object.keys()就没有访问该属性。

好了，在知道上面的两点之后，我们就要来思考，遍历的时候，我们要访问对象的什么属性呢？是访问对象自身的还是原型链中的，还是两者都访问；是访问可枚举的属性，还是连不可枚举的属性也访问呢？在遍历前，我们要思考这么一个问题。

我自己就总结了一下JS与枚举和遍历相关的一些方法：

* 最常用的for...in 循环，可以访问对象自身和原型链中的可枚举属性

看下面的代码：

```
function Man(){
  this.sex = 'male';
}
Man.prototype.legs = 2;

var xiaoming = new Man();

for(var key in xiaoming){
  console.log(key);
}
//输出sex 和 legs
```

* Object.keys()方法，一个包含对象可枚举属性的数组

```
继续用上面的例子：
var xiaoming = {};
Object.defineProperties(xiaoming,{
  sex:{
      value:'male',
      configurable:true,
      writable:true,
      enumerable:true
    },
  legs:{
      value:4,
      enumberable:false,
      configurable:true,
      writable:true
    }
});

console.log(Object.keys(xiaoming)); //输出[sex]
```

* Object.getOwnPropertyNames()方法，返回一个包含对象所有属性的数组，无论属性是否可枚举

看下面代码：

```

var xiaoming = {};
Object.defineProperties(xiaoming,{
  sex:{
      value:'male',
      configurable:true,
      writable:true,
      enumerable:true
    },
  legs:{
      value:4,
      enumberable:false,
      configurable:true,
      writable:true
    }
});
console.log(Object.getOwnPropertyNames(xiaoming));//输出[ 'sex', 'legs' ]
```

* 遍历对象的时候，我们一般会用for...in 循环，但是对于一些已经部署了iterator接口的对象，我我们可以用for..of循环。Object对象没有部署iterator接口，所以默认情况下不使用for..of循环。如果想要对一个对象使用for ... of语句，应该先部署iterator接口，否则会抛出错误。（具体部署方法不再此文描述）
