# 观察订阅模式

观察订阅模式主要用于解决对象之前信息传递的问题，比如a对象要发布一个信息，b和c和d对象要知道，a就要分别通知b,c,d对象。但是通过观察订阅模式，在订阅器里面，只要b,c,d订阅了，a发布信息，b,c,d就会收到信息，然后处理信息。a对象就不用一个个通知bcd。当一个对象变化，引起其他多个对象变化的时候（其他对象变化不会引起这个对象变化），使用观察订阅模式。
例子代码如下:

```
			sub = (function(){
				var subscribers = [];
				return {
					add:function(event,callback){
						if(!subscribers[event]){
							subscribers[event] = [];
						}
						subscribers[event].push(callback);
					},
					remove:function(event,callback){
						if(!subscribers[event]){
							return false;
						}	else if(subscribers[event].length === 0){
							return false;
						}	else if(!callback){
							subscribers[event] = null;
						}	else {
							for(var index in subscribers[event]){
								if(subscribers[event][index] === callback){
									subscribers[event].splice(index,1);
								}
							}
						}
					},
					publish:function(event,message){
						var callbacks = subscribers[event];
						for(var index in callbacks){
							callbacks[index](message);
						}
					},
				};
			})();
			console.log(sub);
			function first(value){
				console.log(value)
			}
			function second(value){
				console.log(value);
			}
			sub.add('fangjia',first);
			sub.add('fangjia',second);
			sub.publish('fangjia','1000');//控制台输出两次1000
			
```

上面的例子就是观察/订阅模式的基本实现。当然，我们还可以强上面的模式。利用复制属性的方法将sub订阅器中的方法拷贝到其他对象中去以备调用。

* 解构观察/订阅模式

观察订阅模式主要由4部分组成，一个是订阅器（用于存放订阅的信息的方法），一个是增加器（用来增加订阅者），一个是删除器（用于删除相应的订阅者，或者事件），一个是发布器（用于向某事件发送信息）

下面就来结构一下观察订阅模式：

```
/*                         增加器（用于向订阅器增加事件）                 
 *          发布器 ----传递信息----->> 订阅器（存放着各种处理信息的方法）
 *                         删除器（用于删除事件） 
 */

			sub = (function(){
				//订阅器，存放着各种各样的处理信息的函数。
				var subscribers = []; 
				return {
					add:function(event,callback){
					//根据事件划分不同的明明空间用于储存订阅者
						if(!subscribers[event]){
							subscribers[event] = [];
						}
						subscribers[event].push(callback);
					},
					remove:function(event,callback){
					//根据参数删除特定事件的特定订阅者
						if(!subscribers[event]){
							return false;
						}	else if(subscribers[event].length === 0){
							return false;
						}	else if(!callback){
							subscribers[event] = null;
						}	else {
							for(var index in subscribers[event]){
								if(subscribers[event][index] === callback){
									subscribers[event].splice(index,1);
								}
							}
						}
					},
					publish:function(event,message){
					//调用订阅者，并将信息传递给订阅者执行
						var callbacks = subscribers[event];
						for(var index in callbacks){
							callbacks[index](message);
						}
					},
				};
			})();
			console.log(sub);
			function first(value){
				console.log(value)
			}
			function second(value){
				console.log(value);
			}
			sub.add('fangjia',first);
			sub.add('fangjia',second);
			sub.remove('fangjia');
			sub.publish('fangjia','1000');
			
```
