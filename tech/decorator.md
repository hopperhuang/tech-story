# 装饰者模式

装饰者模式的核心概念在于对函数放进行装饰，将原来的方法不断升级，方法升级后出了可以产生原来的效果，还可以产生新的效果。
例子如下：

```
			tree = {
				get: function() {
					console.log('get a tree');
				},
				redb: function() {
					var old = this.get;
					this.get = function() {
						old();
						console.log('put redballs on');
					}
				},
				blueball:function(){
					var old = this.get;
					this.get = function(){
						old();
						console.log('put blueballs on');
					}
				}
			}
			tree.redb();
			tree.blueball();
			tree.get(); //最后控制台输出'get a tree' 'put redballs on' 'put blueballs on'
			
```

从上面的代码可以看到我们的get方法一次又一次被重写。

* 解构装饰者模式

装饰者模式的关键在于对于原来的想要装饰的方法的引入，然后在原来方法的基础上，添加新的方法，然后重写。

```
			tree = {
				get: function() {
					console.log('get a tree');
				},
				redb: function() {
				//这里引用了原来的方法
					var old = this.get;
					this.get = function() {//这里重写原来的方法，然后在新方法里面调用了原来的方法
						old();
						console.log('put redballs on');
					}
				},
				blueball:function(){
					var old = this.get;
					this.get = function(){
						old();
						console.log('put blueballs on');
					}
				}
			}
			tree.redb();
			tree.blueball();
			tree.get(); //最后控制台输出'get a tree' 'put redballs on' 'put blueballs on'
			
```

* AOP实现装饰者模式

代码如下:

```
			Function.prototype.after = function(fn){
				var self = this;
				return function(){
					self.apply(this,arguments);
					return fn.apply(this,arguments);
				}
			};
			function tree(){
				console.log('tree');
			}
			function putRedball(){
				console.log('redBall');
			}
			function putBlueBall(){
				console.log('blueBall');
			}
			tree = tree.after(putBlueBall).after(putRedball);
			tree();//控制台输出tree,redball,blueball
			
```
实现AOP模式的关键依然是保存对就的函数的引用，并在新的方法里面调用原来的函数。

```
			Function.prototype.after = function(fn){
				var self = this;//对原来方法的引用
				return function(){//生成新的函数。
					self.apply(this,arguments);//调用原来的方法
					return fn.apply(this,arguments);//调用想要增加的方法
				}
			};
			function tree(){
				console.log('tree');
			}
			function putRedball(){
				console.log('redBall');
			}
			function putBlueBall(){
				console.log('blueBall');
			}
			tree = tree.after(putBlueBall).after(putRedball);
			tree();
			
```