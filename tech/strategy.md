# 策略模式

策略模式的本意就是将各种形式的算法和策略封装在同一个对象里面，根据不同的参数，调用不同的算法去策略去处理问题。
你是否写过类似下面的代码：

```
	function caculate(level,salary){
		if(level === 's'){
			return salary * 4;
		}	if(level === 'a'){
			return salary * 3;
		}	if(level === 'b'){
			return salary　* 2;
		}
	}
```
上面的caculate函数用于计算奖金，根据level和salary，分别采用不同的方法去计算奖金。但是，我们考虑一下，如果我们有其他的level的话，原来的函数就不使用了，这时，我们就需要修改原来的方法。这样的做法违反了开放-封闭原则，因此我们并不提倡这样的做法。在JS中，实现策略模式很简单，我们可以生成一个策略对象，将各种各样的策略添加到这个策略对象里面，根据需要单参数调用策略对象即可。
下面使用策略模式重写上面的函数：

```
			var strategy ={};
			strategy.s = function(salary){
				return 4 * salary;
			};
			strategy.a = function(salar){
				return 3 * salary;
			};
			strategy.b = function(salar){
				return 2 * salary;
			};
			var caculate = function(level,salary){
				return strategy[level](salary);
			};
			
```
在上面的代码中，我们将不同的策略添加到同意策略对象里面，caculate方法，根据level来调用不同的策略。如果需要增加策略的时候，我们直接在strategy对象上添加即可，策略模式多大增强了代码的可维护性和可复用性。

在现实的开发中，策略模式很适合用于表单验证。看下面的代码。

```
			var strategy = {};
			strategy.number = function(number){
			
			};
			strategy.name = function(name){
				
			}
			strategy.email = function(email){
				
			}
			var validtor = (function(s){
				var stratey = s;
				return function(testObject){
					for(var index in testObject){
						strategy[idnex](testObject[index]);
					}
				};
			})(strategy);
			validtor({number:1,
				name:'hopper',
				email:'xxx@abc.com'
			});
			
```

上面的代码中，我们在stragety方法中封装了需要用于验证的方法，并用validator去代理了策略独享。策略添加时，validator的验证方法也会增加。这样，就可以一次性验证我们想验证的对象。
喜欢的话你也可以对上面的方法进行改进，写成AOP的形式，这样就可以先验证一个，再验证另一个，不会一次验证所有参数。
