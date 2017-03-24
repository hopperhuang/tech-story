#单例模式

单例模式适合用于某个特定的对象在全局中，只需要存在一个，不需要多个的情景。顾名思义，单例嘛，就是一个实例咯。

例子如下：

```
//我们现在假设全局中，我们只需要一个bike对象,不需要其他对象。

MakeBike = (function(){
	var instance;
	function Bike(){
		this.name = 'bike';
	}
	return {
		getBike:function(){
			if(instance){
				return instance;
			}	else{
				return instance = new Bike();
			}
		}
	}
})();

var firstBike = MakeBike.getBike();
var secondBike = MakeBike.getBike();
console.log(firstBike === secondBike);//控制台输出true 可以见到两个bike对象指向同一个对象

```

* 解构单例模式

我们来解构一下单例模式。单例模式一般由三部分组成，分别是单例储存器，一个构造函数，和一个单例管理器。单例储存器用于存放单例，构造函数则用于生成单例，单例管理器则用于判断单例是否存在，如果单例存在则直接返回单例，如果单例不存在则构造单例，然后返回单例。

继续利用上面的代码来解释。

```
MakeBike = (function(){
	//instance 变量用于存放单例。
	var instance; 
	//Bike构造函数用于构造单例
	function Bike(){
		this.name = 'bike';
	}
	return {
	//单例管理器，用于确定直接返回单例还是创建单例
		getBike:function(){
			if(instance){
				return instance;//单例存在的情况下直接返回单例
			}	else{
				return instance = new Bike();//单例不存在的情况下就创造一个单例，然后返回单例。
			}
		}
	}
})();
```
从上面可以看到，单例模式的构成就是单例 + 构造函数 + 单例管理器。

* 代理单例

试想一下，构造函数一定存在单例模式里面吗？要是我想创建不同的Bike对象怎么办，又要再写一个构造函数吗？？
不一定，我们可以将构造函数提炼出来，让代理器，去引用构造函数，间接生成单例。
看下面的代码：

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
同样，我们构造了一个单例。
我们让创建单例和管理单例的代码分离开来，通过管理器对构造器的引用对函数进行了聚合，这样的方式既清爽，又好维护。这样，我们既可以创建单例，也可以创建多个实例，不用特别写一个构造函数，想写其他单例函数时，通过代理也可以生成。
