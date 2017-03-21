#编程模式

下面来讲下js中的变成模式.

1. 命名空间

在js中，因为没有类的原因，变量的声明很多时候都是公有的。过多的声明公有变量，会导致的问题就是全局变量容易被污染或者被覆盖。这时候就需要命名空间。
下面来写一个扩展命名空间的函数:

```
function nameSpace(waitToExtend,wantToExtend){
	var parts = wantToExtend.split('.');
	var waitToExtend = waitToExtend;
	for(var index in parts){
		if(waitToExtend[parts[index]]){
			waitToExtend = waitToExtend[parts[index]];
		}	else {
			waitToExtend[parts[index]] = {};
			waitToExtend = waitToExtend[parts[index]];
		}
	}
}

var myapp = {dom:{a:{}}};
nameSpace(myapp,'dom.element');
console.log(myapp.dom);//可以见到myapp.dom对象里面有个另个属性分别是a和element
```

2. 初始化分支

在开发过程中，我们需要注意浏览器的特性，就需要对浏览器的特性就行嗅探。每次运行都对浏览器的特性进行嗅探会导致浏览器性能的下降，这时候我们就可以预先定义方法。

```
var myapp = {};
myapp.event = {};
myapp.event.addEvent = null;
if(typeof window.addEventListener === 'function'){
	myapp.event.addEvent = function(dom,action,callback,useCapture){
		document.getElementById(dom).addEventListener(action,callback,useCapture);
	};	
}	else if(typeof document.attachment === 'function') {
	myapp.event.addEvent = function(dom,atcion,callback){
		document.getElementById(dom).attachEvent('on' + action,callback);
	};	
}	else {
	myapp.event.addEvent = function(dom,action,callback){
		document.getElementById(dom).['on'+action] = callback;
	}
}
```

3. 延迟定义

继续上面的问题，难道我们的方法就一定要提前定义吗，当然不是，还可以延迟定义。我们可以选择这样的做法，在第一次使用方法的时候，对函数进行初始化定义，以后再次使用方法时，就加载这个已经定义好的函数。

```
var myapp = {};
myapp.event = {};
myapp.event.addEvent = function(element,action,callback){
	if(typeof window.addEventListener === 'function'){
		this.addEvent = function(element,action,callback){
			element.addEventListener(action,callback,false);
		}
		this.addEvent(element,action,callback);
	}	else if(typeof document.attachment === 'function'){
		this.addEvent = function(element,action,callback){
			element.attachment('on'+action,callback);
		}
		this.addEvent(element,action,callback);
	}	else {
		this.addEvent = function(element,action,callback){
			element[on + action] = callback;
		};
		this.addEvent(element,action,callback);
	}
};
```
