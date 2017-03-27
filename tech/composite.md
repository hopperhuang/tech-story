# 组合模式

组合模式事实上就是一个树-叶结构。一个父对象里面有很多个子对象，而子对象里面又有子对象的怎么一个链式结构。
下面就来看看组合模式的例子：
```
			function folder(name){
				this.name = name;
				this.files = [];
			}
			folder.prototype.add = function(file){
				this.files.push(file);
			};
			folder.prototype.scan = function(){
				console.log(this.name);
				var files = this.files;
				for(var index in files){
					files[index].scan();
				}
			};
			function file(name){
				this.name = name;
			}
			file.prototype.scan = function(){
				console.log(this.name);
			}
			var learning = new folder('学习资料');
			var maths = new file('数学练习册');
			var chinese = new file('语文练习册');
			var english = new file('英语练习册');
			var bag = new folder('书包');
			bag.add(learning);
			learning.add(maths);
			learning.add(chinese);
			learning.add(english);
			bag.scan();//控制台依次输出：书包，学习资料，数学练习册，语文练习册，英文练习册。
			
```

* 解构组合模式

组合模式实际上就是构造一个树结构，整个组合的结构是类似这样的。
```
                                        树
             /\
                                   叶    叶
           /\ /\
                              叶  叶叶    叶
                              
```
这样像上面的树状图一样，一层一层扩展下去。
继续回看刚才的例子代码：

```
			function folder(name){
				this.name = name;
				this.files = [];
			}
			//add 命令就是树对象的基础上不断扩展叶对象。
			folder.prototype.add = function(file){
				this.files.push(file);
			};
			folder.prototype.scan = function(){
				console.log(this.name);
				var files = this.files;
				for(var index in files){
					files[index].scan();
				}
			};
			function file(name){
				this.name = name;
			}
			file.prototype.scan = function(){
				console.log(this.name);
			}
			var learning = new folder('学习资料');
			var maths = new file('数学练习册');
			var chinese = new file('语文练习册');
			var english = new file('英语练习册');
			var bag = new folder('书包');
			bag.add(learning);
			learning.add(maths);
			learning.add(chinese);
			learning.add(english);
			bag.scan();//控制台依次输出：书包，学习资料，数学练习册，语文练习册，英文练习册。
			
```

通过add方法，树对象不断地对自身进行扩展。还有一个关键点就是scan方法，我们为树对象和叶对象都部署了scan接口，通过scan接口，可以统一调用树对象和叶对象的对象，执行相应的动作。
我认为，组合模式比较适合用于树对象和叶对象需要利用统一接口依次调用的场景。
