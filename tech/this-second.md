#this 用法全面解析（二）

在（一）中，我们在讲述了this的两个用法，一是在普通函数中调用this，二是在对象中调用this。接下来我们要讲述的是在new 函数中调用this，和this作为参数时出现的情景。

1. 在new 方法中的this

在对一个函数调用new 方法后，会先后执行如下的操作：

1. 生成一个对象；
2. 将函数中的this指向这个刚刚生成的对象；
3. 将函数的原型链绑与这个对象绑定；
4. 检测对象中是否有return 语句，如果有return 语句，则返回return 语句后的对象，如果没有return 语句，则返回生成的对象。

我们执行一下这段代码，看看生成的对象是什么：

```
function make(){
  this.a = 1;
  this.b = 2;
}
var object = new make();
console.log(object);
```

在node执行后，终端输出的是make { a: 1, b: 2 }生成的对象是一个make实例，同时也印证了我们的说法，在new 方法中调用this，就是将this指向这个刚刚生成的对象。

2. 当调用this的函数作为参数传入

当调用this的函数作为一个参数传入另一个函数时，this对象指向的是什么呢？？我们看看下面的代码：

```
  var o = {
  getThis:function(){
    console.log(this);
  }
};

function run(callback){
  callback();
}
run(o.getThis);
```

按照第二点的结论，对象中调用this，难道this指向的不是o对象吗？？
我们来看看执行的结果，

```

{ global: [Circular],
  process: 
   process {
     title: 'node',
     version: 'v7.1.0',
    ...}
```

在执行另外一个函数

```
var o = {
  getThis:function(){
    console.log(this);
  }
};

(o.getThis=o.getThis)()

```

执行结果依然是gloabl对象。
所以说，当一个调用this的函数,作为参数传入时，这个this对象指向的是global。

总结：

3. 在new 方法中的this，会将this 指向刚刚生成对象。
4. 将一个调用this的函数，作为参数传入时，这个this对象指向的时global。


