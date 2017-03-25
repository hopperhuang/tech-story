# 代理者模式

代理者模式适用于函数不方便直接调用的场景。比如我们想要调用函数A，但是我们需要在调用函数A之前做某些处理，等这里处理完成之后再调用函数A，这时候我们就可以运动代理模式。让代理去做预处理，然后决定何时调用函数A。
回顾之前的代码：

```
			function Bike(){//构造函数
				this.name = 'bike';
			}
			function Proxy(constructor){//在代理函数里面引入需要的构造函数
				var instance ;
				return function(){//生成一个用于生成单例的函数
					if(instance){
						return instance;
					}	else {
						return instance = new constructor();//引用构造函数生成对象
					}
				};
			}
			BikeProxy = Proxy(Bike);//生成一个单例代理构造器。
			firstBike = BikeProxy();
			secondBike = BikeProxy();
			console.log(firstBike === secondBike);//true
			
```
我们来分析一下上面的这段代码。在代码中我们要构造一个单例的bike，但是我们又想要用Bike构造函数去构造其他的bike，而不只是一个bike。这时候我们就然后Proxy函数去代理Bike，判断是否需要引用Bike，生成一个单例。Proxy对象承担了调用Bike之前一部分的预处理工作，比如判断单例是否纯在。而Bike对象在Proxy函数外，又方便被其他函数调用，不至于我们需要用Bike函数时又要再写一个。

* 图片预加载处理

在处理图片预加载的时候我们也可以用带代理模式。

```
var img = (function(){
	var tupian = new Image();
	document.body.appendChild(tupian);
	return {
		setSrc:function(src){
			tupian.src = scr;
		}
	}
})();

yuchuli = function(src){
	var proxyImage = new Image();
	proxyImage.onload = function(){
		img.setSrc(src);
	};
	proxyImage.src = src;
	img.setSrc = ''//这里输入一个占位符。
}
yuchili();
```

在上面的函数中，我们想在图片完全载入前，显示一个占位符，所以我们就不直接设置图片的src属性为目标图片的地址，我们用另一个img去载入数据，等待载入完成后，再重新设置图片。在上面的操作中，不方面直接操作的是直接设置SRC属性，设置SRC前的行为是预加载图片。所以我们利用代理模式。

代理模式的构成是：需要代理的行为 + 代理器，代理器负责在调用调用行为时，进行预处理，并确定何时调用行为。
