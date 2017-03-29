# 享元模式

享元模式实用于需要创建大量对象的情景。
比如下面的场景。

```
			function Man(name,height,weight,hobby){
				this.sex = 'male';
				this.name = name;
				this.height = height;
				this.weight = weight;
				this.hobby = hobby;
			}
			Man.prototype.check = function(){
				console.log(this.sex);
				console.log(this.name);
				console.log(this.height);
				console.log(this.weight);
				console.log(this.hobby);
			};
			function Woman(name,height,weight,hobby){
				this.sex = 'female';
				this.name = name;
				this.height = height;
				this.weight = weight;
				this.hobby = hobby;
			}
			Woman.prototype.check = function(){
				console.log(this.sex);
				console.log(this.name);
				console.log(this.height);
				console.log(this.weight);
				console.log(this.hobby);
			}
			var data = [{name:'a',height:'a',weight:'a',hobby:'a'}];//假设这里有很多个data对象。
			for(var index in data){
				var man = new Man(data[index]['name'],data[index]['height'],data[index]['weight'],data[index]['hobby']);
				man.check();
			}
```
上面的方法通过一个for循环制造了很多个多想，并触发了每个对象上的check方法，成功实现了想要的效果。但是，这种创建大量对象的方法，会占用大量的内存，尤其在浏览器内存有限的情况下，不建议采用这种做法。

面对这样的情景，我么可以使用享元模式。
从上面的例子中，我们看到构造出来的独享中，很多属性是不同的。但是，有一个属性变化是比较少的，就是sex属性，sex属性只有两种，一就是male，二就是female。
因为这个小小的不同就需要两个构造函数。所以我们又合并这个不同，将构造函数变成Human，然后将Human构造函数传入sex属性，作为一个man的元和woman的元。
具体实现如下：

```
			Human.prototype.say = function(name){
				OutsideStatusFactory.setOutsideStatus.call(this,name);
				console.log(this.name);
				console.log(this.weight);
				console.log(this.height);
				console.log(this.hobby);
				console.log(this.sex);
			}
			flyWeightFactory = (function(){
				var flyWeightObject = {};
				return {
					create:function(type){
						if(flyWeightObject[type]){
							return flyWeightObject[type];
						}
						return flyWeightObject[type] = new Human(type);
					}
				}
			})();
			var check = []
			OutsideStatusFactory = (function(){
				var data = [];
				return {
					create:function(sex,name,height,weight,hobby){
						var flyWeight = flyWeightFactory.create(sex);
						data[name] = {name:name,height:height,weight:height,hobby:hobby}
						check[name] = function(){
							flyWeight.say(name);
						}
						return flyWeight
					},
					setOutsideStatus:function(name){
						var outsideStatus = data[name];
						this.name = data[name]['name'];
						this.weight = data[name]['weight'];
						this.height = data[name]['height'];
						this.hobby = data[name]['hobby'];
					}
				}
			})();
			console.log(OutsideStatusFactory);
			var man = OutsideStatusFactory.create('male','hopper',175,120,'football');
			check['hopper']();
					
```

* 解构享元模式

一，首先我们需要分离出元构造器。比如上面的例子中，我们观察到，变化比较少的一个属性就是sex属性，因此我们将两个构造方法合并成一个构造方法。并通过human方法，传入sex参数来创建不同的元。

二，定义元的公共方法，比如上面的say方法。say方法有一个特别之处，就是接受一个name参数，然后根据这个name参数去拉取需要的外部属性，然后再执行剩余的方法。

三，定义个元工厂管理器。这个元工厂负责创造并管理元。比如上面的例子，元工厂存放了两个对象，一个是sex为male的元，一个是sex为female的元。当元对象存在时，则返回元对象，元对象不存在时，则创建元对象。然后我们吧创建对象的行为委托给外部属性管理器，外部属性管理器创建对象时，从元工厂获取元，然后配合外部属性，组成一个完整的对象。

四，外部属性管理器。外部属性管理器承担了创建对象的职能和将外部属性添加到元对象的责任。

我们先来说说创建对象的职能，这个职能分了三部分，一是从工厂获取元，二是将外部数据添加到外部数据储存器，三是创建一个第三方对象，这个第三方对象承接了元对象的公共方法。以后访问特定对象的特定方法，只需要访问这个第三方对象就可以。

再者就是外部属性添加都元对象的只能。这个只能一般会被元对象的公共方法调用，调用之后，元对象与外部属性组成一个完成的对象，然后进行需要的操作。

继续上面的例子用代码来讲解：

```
			Human.prototype.say = function(name){
			//通过name属性获取外部数据。
				//从外部属性工厂拉取数据，组成一个完整的对象。
				OutsideStatusFactory.setOutsideStatus.call(this,name);
				//进行需要的操作。
				console.log(this.name);
				console.log(this.weight);
				console.log(this.height);
				console.log(this.hobby);
				console.log(this.sex);
			}
			flyWeightFactory = (function(){
			//存放元对象
				var flyWeightObject = {};
				return {
					create:function(type){
						if(flyWeightObject[type]){//判断元对象是否存在，存在则直接返回。
							return flyWeightObject[type];
						}
						return flyWeightObject[type] = new Human(type);//创建新的元对象并存放到储存器。
					}
				}
			})();
			var check = [];//第三方容器，用于接收刚创建的元对象和绑定好的方法。
			OutsideStatusFactory = (function(){
				var data = [];//存放外部数据
				return {//创建对象职能
					create:function(sex,name,height,weight,hobby){
					//从工厂拉取元
						var flyWeight = flyWeightFactory.create(sex);
						//将数据放到储存器
						data[name] = {name:name,height:height,weight:height,hobby:hobby}
						//第三方接收创建好的元对象的功能方法。这个第三方有时候可能是一个dom元素
						check[name] = function(){
							flyWeight.say(name);
						}
						return flyWeight
					},
					setOutsideStatus:function(name){//通过name属性获取外部属性
						var outsideStatus = data[name];
						this.name = data[name]['name'];
						this.weight = data[name]['weight'];
						this.height = data[name]['height'];
						this.hobby = data[name]['hobby'];
					}
				}
			})();
			var man = OutsideStatusFactory.create('male','hopper',175,120,'football');
			var woman = OutsideStatusFactory.create('female','miaomiao',170,90,'hopper');
			check['hopper']();//male,hopper,175,120,football
			check['miaomiao']();//female,miaomiao,170,90,hopper
			
```
