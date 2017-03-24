# 工厂模式

工厂模式就是要像工厂一样构造我们想要的对象，只要我们向工厂下单，工厂就会生成目标产品，只要工厂有，我们就可以向工厂要，不需要特定到其他地方去获取产品。
代码例子如下：

```
			Factory = (function(){
				var factory = {};
				factory.bike = function(){
					this.name = 'bike';
				};
				factory.car = function(){
					this.name = 'car';
				};
				factory.plane = function(){
					this.name = 'plane';
				};
				return {
					get:function(type){
						return new factory[type];
					},
					add:function(type,constructorFunc){
						factory[type] = constructorFunc;
					}
				};
			})();
			var bike = Factory.get('bike');
			console.log(bike.name);//bike
			Factory.add('motor',function(){
				this.name = 'motor';
			});
			var motor = Factory.get('motor');
			console.log(motor.name);//motor
		
```
可以看到，从上面的Factory里面，只要输入类型，我们就可以获得各种各样的对象。
出了生产内置的对象外，我们还可以增加生产的类型，比如在上面的例子中，我们就增加了一个生成motor的构造方法。

下面结构一下工厂模式。
工厂模式主要由一个内置的构造函存储器和一个构造函数管理器构成。继续用上面的例子来分析吧。

```
			Factory = (function(){
				//内置的构造函数储存器就像是一个工厂，里面定义了各种各样的构造函数。
				var factory = {};
				factory.bike = function(){
					this.name = 'bike';
				};
				factory.car = function(){
					this.name = 'car';
				};
				factory.plane = function(){
					this.name = 'plane';
				};
				return {//这是一个构造函数管理器
					get:function(type){
					//生成我们想要的对象
						return new factory[type];
					},
					add:function(type,constructorFunc){
					//把想要的构造函数增加到构造函数管理器中去。
						factory[type] = constructorFunc;
					}
				};
			})();
			var bike = Factory.get('bike');
			console.log(bike.name);
			Factory.add('motor',function(){
				this.name = 'motor';
			});
			var motor = Factory.get('motor');
			console.log(motor.name);
			
```
在上面的构造函数管理器中，我们开发了add接口，add接口可以修改内置的函数工厂，这样我们就可以顺利的内置对象进行修改。

像之前单例模式的例子一样，我们把工厂，即创建功能和管理功能分离出来，用代理的方法去生成对象。
代码如下：
```
			function Proxy(factoryObject){
			//函数工厂引用外面的构造函数对象。
				var factory = factoryObject;
				var get = function(type){
					return new factory[type];
				}
				//增加函数工厂中的对象。
				get.add = function(type,callback){
					factory[type] = callback;
				}
				return get;
			}
			fac ={
				bike:function(){
					this.name = 'bike';
				},
				car:function(){
					this.name = 'car';
				}
			};
			FactoryProxy = Proxy(fac);
			var bike = FactoryProxy('bike');
			console.log(bike.name);
			FactoryProxy.add('plane',function(){
				this.name = 'plane';
			});
			plane = FactoryProxy('plane');
			console.log(plane.name);
			
```
这个做法的好处就是，当我们生成另一个函数工厂时，只要写好函数工厂，然后用代理器去产生新的函数工厂代理器即可，这个做法提高了函数的复用性。