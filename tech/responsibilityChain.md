# 职责链模式

职责链模式实用与将任务一级一级传递下去的情景。比如，我要进行三项认证，我可以输入参数后，交给职责链去进行认证，而不需要一次一个参数的输入的去验证。

下面用AOP去实现职责链模式：

```
			Function.prototype.after = function(afterFunction){
				var self = this;
				return function(){
					self.apply(this,arguments);
					return afterFunction.apply(this,arguments);
				};
			};
			Function.prototype.before = function(beforeFunction){
				var self = this;
				return function(){
					beforeFunction.apply(this,arguments);
					return self.apply(this,arguments);
				};
			};
			function first(a,b,c){
				console.log(a);
			}
			function second(a,b,c){
				console.log(b);
			}
			function third(a,b,c){
				console.log(c);
			}
			var chain = second.before(first).after(third);
			chain(1,2,3);
			
```
通过aop，我们实现了职责链。只需要在chain方法中一次传递一个参数，我们就将三个方法依次进行操作，依次输出。

aop方法非常实用，在之前的装饰者模式中我们就会用到了。aop方法便于往前，往后添加方法，组成一条链式的函数，一次性进行多个操作，而不用多次输入参数。