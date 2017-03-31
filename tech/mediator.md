# 中介者模式

中介者模式适用于对象之间相互通讯的情景。比如，一个组件变化，其他组都要进行变化。一个组件发出信息，其他组建都要发出信息的情景。
比如下面的场景，一个玩家得分，同队伍的玩家就会得分，而不同队伍的玩家就会十分。通讯就在两个队伍之间，还有同队玩家之间进行。
我们用下面的代码来实现中介者模式。

```
			function Player(name){
				this.name = name;
			}
			Player.prototype.join = function(team){
				if(!this.team){
					this.team = team;
					manager.recieveOperationCommand['join'](team,this);
				}
				return false;
			};
			Player.prototype.remove = function(){
				if(!this.team){return false;}
				team = this.team;
				name = this.name;
				this.team = null;
				manager.recieveOperationCommand['remove'](team,name)
			}
			Player.prototype.jump = function(team){
				var name = this.name;
				var now = this.team;
				var target = team;
				manager.recieveOperationCommand['jump'](now,target,name,this);
			}
			Player.prototype.getPoints = function(){
				console.log('getPoints');
				var team = this.team;
				manager.recieveOperationCommand['getPoints'](team);
			};
			Player.prototype.losePoints = function(){
				console.log('you lose points');
			}
			var manager = (function(){
				var team = [];
				var operation = {};
				operation.join = function(type,player){
					if(team[type]){
						team[type].push(player);
					}	else {
						team[type] = [];
						team[type].push(player);
					}
				};
				operation.remove = function(type,name){
					var yourTeam = team[type];
					for(var index in yourTeam){
						if(yourTeam[index]['name'] === name){
							yourTeam.splice(index,1);
							break;
						}
					}
				};
				operation.jump = function(now,target,name,player){
					operation.remove(now,name);
					operation.join(target,player);
				};
				operation.getPoints = function(type){
						for(var index in team){
							if(index === type){
								for(var another in team[type]){
									console.log(team[type][another]['name'] + ' get points.');
								}
							}	else {
								for(var other in team[index]){
									console.log(team[index][other]['name'] + ' lose points');
								}
							}
						}
					
				}
				return {
					recieveOperationCommand:operation
				}
			})()
			
			var xiaoming = new Player('xiaoming');
			var xiaohong = new Player('xiaohong');
			var xiaoguang = new Player('xiaoguang');
			xiaoming.join('red');
			xiaoguang.join('red');
			xiaohong.join('blue');
			xiaoguang.jump('blue');
			console.log(xiaoguang);
			xiaoming.getPoints();
			//控制台依次输出  getPoints xiaoming getpoints xiaohong losepoints xiaoguang losepoints.
			

```
以上就是中介者模式的实现。

我们来解释一下中介者模式的实现，中介者模式主要由两部分构成。一是参与者，二是中介管理器。上面的例子中PLAYER就是参与者，而manager就是管理器。管理器中，又由3部分组成，一是存储部分，这部分保存着对参与者的引用，用于在接下来的操作中，对参与者进行操作。第二部分就是一个操作opertaiton对象，operation对象中保存着各种需要调用的操作。第三就是命令接收器，命令接收器接收命令，调用operation对象中的相关方法。

参与对象通过在特定方法中，访问管理器的命令接收接口，从而调用opertion的相关接口，操作储存后的参与者。这既是中介者模式的实现方法。

继续用上面的代码去解释：

```
			function Player(name){
			//创建参与者
				this.name = name;
			}
			//参与者的行为
			Player.prototype.join = function(team){	
				if(!this.team){
					this.team = team;
					//向中介管理器发送命令执行相关的操作。
					manager.recieveOperationCommand['join'](team,this);
				}
				return false;
			};
			Player.prototype.remove = function(){
				if(!this.team){return false;}
				team = this.team;
				name = this.name;
				this.team = null;
				//发送命令进行相关操作。
				manager.recieveOperationCommand['remove'](team,name)
			}
			Player.prototype.jump = function(team){
				var name = this.name;
				var now = this.team;
				var target = team;
				//访问命令接口。
				manager.recieveOperationCommand['jump'](now,target,name,this);
			}
			Player.prototype.getPoints = function(){
				console.log('getPoints');
				var team = this.team;
				//访问命令接口，发送命令。
				manager.recieveOperationCommand['getPoints'](team);
			};
			Player.prototype.losePoints = function(){
				console.log('you lose points');
			}
			var manager = (function(){
			//存放参与者对象
				var team = [];
				//operation对象上定义了各种各样的操作。
				var operation = {};
				operation.join = function(type,player){
					if(team[type]){
						team[type].push(player);
					}	else {
						team[type] = [];
						team[type].push(player);
					}
				};
				operation.remove = function(type,name){
					var yourTeam = team[type];
					for(var index in yourTeam){
						if(yourTeam[index]['name'] === name){
							yourTeam.splice(index,1);
							break;
						}
					}
				};
				operation.jump = function(now,target,name,player){
					operation.remove(now,name);
					operation.join(target,player);
				};
				operation.getPoints = function(type){
						for(var index in team){
							if(index === type){
								for(var another in team[type]){
									console.log(team[type][another]['name'] + ' get points.');
								}
							}	else {
								for(var other in team[index]){
									console.log(team[index][other]['name'] + ' lose points');
								}
							}
						}
					
				}
				//通过命令接收接口接收命令，然后调用operation对象进行操作。
				return {
					recieveOperationCommand:operation
				}
			})();
			
			var xiaoming = new Player('xiaoming');
			var xiaohong = new Player('xiaohong');
			var xiaoguang = new Player('xiaoguang');
			xiaoming.join('red');
			xiaoguang.join('red');
			xiaohong.join('blue');
			xiaoguang.jump('blue');
			console.log(xiaoguang);
			xiaoming.getPoints();
			//控制台依次输出  getPoints xiaoming getpoints xiaohong losepoints xiaoguang losepoints.
			
```