# 编程模式

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

4. 配置对象

在编写函数的过程中，我们可以配置默认对象，这样就不用每次都传入参数

```
//定义个bikemaker，配置好参数
function BikeMaker(color,speed){
	this.color = color || 'red';
	this.speed = speed || 20;
}

//生成默认的bike 
var redBike = new BikeMaker();
console.log(redBike.color);//'red'

//生成我们想要的bike
var bikeUWant = new BikeMaker('black',30);
console.log(bikeUWant.color);//'black'
console.log(bikeUWant.speed);///30
```

5. 私有属性

在js中也有私有属性，我们可以把私有属性放在一个函数里面，这样外面的函数就无法访问这个私有属性
```

function(){
	var number = 1;
	function addOne(number){
		number + 1;
	}
}
console.log(number); //error
console.log(addOne);// error
```
可以看到nuber和addOne在函数外面是无法访问的。

6. 特权方法

想要访问私有变量也不是没有办法的，我们可以通过特权方法去访问。特权方法只允许我们从外部访问变量，并不允许我们从外部去改变私有变量。

```
function Privilege(){
 var number = 1;
 return {
 	getNumber:function(){
 		return number;
 	}
 };
}
Privilege().getNumber;//1
```

7. 私有方法和变量公有化

如果想要在外部访问私有变量和私有方法也不是不能的，我们可以将
