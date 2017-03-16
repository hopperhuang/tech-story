# object 与原型链（四）

这是object与原型链中的最后一章了，我们在最后这章中将会分析一下，那些最佳实践，为什么是最佳的。

假设有下面的情景。

我们创建了一个构造函数f1

```

function f1(){
  this.a = 1;
  this.b = 2;
}
f1.prototype.c = 4;
```

但是，我们觉得还不够，我们不想让b的属性为2，所以我们写了另外一个构造函数f2

```

function f2(){
  f1.call(this);
  this.b = 3;
}

f2.prototype = new f1();
```

好了，我们这时候创建一个f2的示例，并且看看他的属性。

```

console.log(o.a);//1
console.log(o.b);//3
console.log(o.c);//4
```

没问题，一切都很正常。

可是我现在不想要b属性了。

```

delete o.b;

```
然后我们再来看一下b属性

```

console.log(o.b);//2
```

奇怪了，怎么b属性还是存在。

下面就来解释一下：

我们访问o的b属性，虽然o本身的b属性是被我们删除了，但是,o.proto属性中还有一个b属性。当我们访问对象的属性时，首先会在在对象本身查询是否有该属性，如果没有，则查询该对象的原型链是否有该属性，这个一级一级遍历下去，直到找到该属性。如果遍历完整条原型链都没有该属性，则返回undefined。

清楚属性的访问机制后，我们再来看看，为什么o对象的原型链中会有b属性呢？
原因在于这个语句：

```

f2.prototype = new f1();
```

这个语句把f2的原型链指向一个f1创建的实例，在这个实例中，本身保存再b属性,他的proto中，则保存着c属性。再利用f2创建o实例时，我们为o的原型链关联到f2的原型链。所以o的原型链本身就有了b属性了。

为了解决这个问题，我们可以用Object.create方法。将上面的语句改写成

```

f2.prototype = Object.create(f1.prototype);
```

这样既将f2的原型链关联到f1，f2的原型链本身也没有多余的属性，很清爽。

再看看下面的情景：

我们创建了一个构造函数，并生成了2个实例。

```

function family(){

}
family.prototype.counts = 2;
family.prototype.members = ['jack','rose'];

var f1 = new family()
var f2 = new family();
```

现在我们想再f1实例增加一个成员，先增加成员的数量

```

f1.counts += 1;
console.log(f1.counts); //3
console.log(f2.counts); //2
```

再插入成员

```

f1.members.push('mike');
console.log(f1.members); // [ 'jack', 'rose', 'mike' ]
console.log(f2.members); // [ 'jack', 'rose', 'mike' ]
```

奇怪了，我们只在f1插入了mike，为什么f2也有mike呢。为什么f1的counts
增加1，f2的counts却没有增加呢。

这个问题就跟js中对象的访问和关联有关。

```

f1.counts += 1
```

这个语句实际是f1.counts = f1.counts +1，这个语句会先再f1的原型链上读取counts属性，然后增加1，再然后在f1本身创建一个counts属性，所以f2的counts属性并枚与改变

而f1.members.push('mike')，f1.members属性会指向个数组，要清楚在js中，members属性只是对这个数组的引用，所以当我们对这个属性进行操作的时候，实际上是对这个数组进行操作。所以,f2.members也会改变。

在开发过程中，我们应注意上述两点问题。


