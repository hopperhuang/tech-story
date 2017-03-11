#this 用法全面解析 （三）

在前面的（一）和（二）中，我们讲述了this的四种情况。那么this在执行的时候是否可以改变呢，将this指向其他对象。答案时可以的。
* 通过call、apply和bind方法改变this的指向

```

var o = {
  a:'1',
  getA:function(){
    console.log(this);
  }
};

var b = { a : '2'}
```

想改变this的指向，我们可以调用call和apply方法。
执行以下代码:

```

o.getA.call(b);
o.getA.apply(b);
```

在node里面执行一下，终端打印出来的时2，而不是1.可以件通过call和apply方法可以改变this的指向。

通过bind方法生成一个新的函数也可以。继续利用上上面的o对象,执行下面的代码：

```

var f = o.getA.bind(b);
f();
```

我们可以在终端中看到输出的时2，而不是1。

bind方法在函数柯里化时，也经常用到。

```

function bar(a,b){
  console.log(a,b);
}
var foo = bar.bind(null,2); 
foo(3);
```

上面的代码，通过bind方法，我们将2绑定到第一个参数并生成新的函数。在执行这个新的函数时，添加一个参数就可以产生与原函数相同的效果。

* 注意this在Node.js和浏览器中的不同。
我们说过，当调用this的函数作为参数传入对象时，this指向的是global或者window。但是在传入setTimeout时，执行结果在Node.js和浏览器中会
有不同。
分别在Node和浏览器中执行以下的代码：

```
var o = {
  a:'1',
  getA:function(){
    console.log(this);
  }
};

setTimeout(o.getA,100);
```

在node.js中，终端打印的是一个timeout对象

```
Timeout {
  _called: true,
  _idleTimeout: 100,
  _idlePrev: null,
  _idleNext: null,
  _idleStart: 72,
  _onTimeout: [Function: getA],
  _timerArgs: undefined,
  _repeat: null }
```

而在浏览器中，控制台输出的是window对象。这点不同时需要我们在使用this对象时需要注意的。

最后，我们全面总结一下,this的用法吧。

1. 在普通函数中调用this，this默认指向global或者window对象。
2. 在对象中调用this, this指向调用他的对象。
3. 在调用new 方法函数中的this,this指向新生成的对象。
4. 将调用this对象的函数，作为参数传入另一个函数时，this指向的时global或者window对象。
5. 可以通过call、apply、bind方法来改变this的指向。
6. 在使用this的过程中，我们需要注意node.js和浏览器的差异。


