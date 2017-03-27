# 模板方法

模板方法的实质就是定义一套行为，然后通过统一的接口去执行我们定义的行为。然后子对象，根据这套定义好的行为去改写，实现属于自己的行为。举个例子，我们定义好了一套叫吃饭的动作。在这套动作里面有，吃饭，夹菜，夹肉，喝水，这几个动作。模板动作是定义好了，但是里面的内容呢？？根据每人的特点，每人都有不同行为特点。比如，吃饭，可以吃泰国香米，可以吃广东水稻，可以吃东北大米。夹肉，可以是鸡肉，鸭肉，鱼肉。等等等。
好吧，回到原来的问题--模板方法。模板方法就是定义一套需要执行的函数，通过统一的接口去调用这套函数。子类在扩展父类的时候，根据自身需求去改写这套动作。
例子如下：

```
			Play.prototype.dress = function(){
				console.log('不穿衣服要裸奔？');
			};
			Play.prototype.bring = function(){
				console.log('不带装备你打个毛？');
			};
			Play.prototype.start = function(){
				console.log('不上场，你是要看饮水机吗？')
			};
			Play.prototype.play = function(){
				this.dress();
				this.bring();
				this.start();
			};
			function PlayFootball(){}
			PlayFootball.prototype = Object.create(Play.prototype);
			PlayFootball.prototype.dress = function(){
				console.log("穿上球衣");
			};
			PlayFootball.prototype.bring = function(){
				console.log('带上足球和球鞋');
			};
			PlayFootball.prototype.start =function(){
				console.log('绿茵场上，请开始你的表演。');
			};
			var playfootball = new PlayFootball();
			playfootball.play();
			//控制台依次输出：床上球衣，带上足球和球鞋，绿茵场上，请开始你的表演。
```

上面的方法就是模板方法的实现。PlayFootball方法继承了Play方法，继承之后，又根据自身特性重写了里面的方法。但是，有一个借口是没有重写的，就是play,通过play借口，可以调用子类和父类上同名的方法。在上面的例子中，我们在父例加入了提示语。除了用console作提示语，在开发过程中，我们还可以用throw，抛出错误的方法来提醒自己，要改写相关的方法。

模板方法不一定要用子类和父类的形式去实现，还可以用子对象和父对象的方法去实现，具体用什么形式去实现，看自己的业务需求吧。