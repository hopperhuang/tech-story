# this 用法全面解析（一）

在学习js的过程中，不少朋友会对this感到困惑，今天就来总结一下this的用法。
顾名思义，this--这，this很容易被误解为调用this函数或者是方法。然而，this实指的调用this的context（上下文对象）

1. 在一般的函数中调用this

我写了一段在函数中调用this的代码，在node.js v7.1.0中执行，代码如下：

```
function A(){
      console.log(this);
    }
    A();
```
返回的结果当然是gloabal；



```
  { global: [Circular],
  process: 
   process {
     title: 'node',
     version: 'v7.1.0',
   ......}
```
 
整个gloabal对象包含的属性太多了，我就不一一列举出来了，可以见当我们在函数调用this的时候，this指向的是全局对象。mdn上也有解释，this默认指向的是全局对象，也就是浏览器中的window或者node中的global。除了接下来要说的两种情况，在对象中调用this，或者在new 方法中调用this。

2. 在对象中调用this

当我们在对象中调用this，this一般指向这个对象。我们来测试一下，代码如下：


```

let o = {
  showThis : function(){
    console.log("I'm o: "+this);
  }
}
o.showThis();

```

在node里面执行一下，得到结果:
I am o: [object Object]现在this指向的是一个o这个对象。这样看可能会疑惑，在浏览器里执行就很清楚了。但是没关系，我们在来写一段代码确认一下,代码如下：


```
let o = {
  a:'a',
  getA:function(){
    console.log(this.a);
  }
}

let a = 'b';
o.getA();
```

方法执行后，在控制台打印出来的是a，而不是b。这就说明了在对象里面调用this，this指向的是调用他的对象，而不是对象。

结论：

1. 在普通的函数中调用this,this指向的是默认的全局对象global或者是window
2. 在对象中调用this,this指向的是调用this的对象。


