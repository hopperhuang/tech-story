# 命令模式

命令模式与策略模式比较类类似，我个人认识命令模式就是对一系列到操作进行封装，然后按照我们想要的顺序，触发相应的操作。而策略模式则是对某种处理方法进行封装。策略模式的重点放在在特定的数据进行处理，而米命令模式的重点则在于操作，而不是处理数据。

命令模式在js中比较简单，一般是有一个excute接口，和一个函数。然后将这个命令对象与一个操作进行对接，比如onclick,onmouseover之类的。
下面就来写一个宏命令：

```
			var marcoCommand = (function(){
				var commandList = [];
				return {
					add:function(command){
						commandList.push(command);
					},
					excute:function(){
						for(var index in commandList){
							commandList[index]();
						}
					},
					remove:function(command){
						for(var index in commandList){
							if(commandList[index] === command){
								commandList.splice(index,1);
							}
						}
					}
				}
			})();
			marcoCommand.add(function(){
				console.log(1);
			});
			marcoCommand.add(function(){
				console.log(2);
			});
			marcoCommand.excute();//控制台依次输出1,2
			
```

宏命令模式中，提供了一个commandList来存放将来要执行的命令。excute接口用于执行函数，add,remove接口用于增加你和移除函数。当然，你可以可以增加相应的方法，通过对commandList数组进行操作，来对函数的位置进行调换也是可以。
命令模式比较简单，但是在dom编程时，绑定时间操作的时候回比较好用。