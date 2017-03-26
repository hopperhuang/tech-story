# 迭代器模式

迭代器模式通常用于处理数组或者对象，当我们希望对数组或对象中的每一个数据都进行相同的操作时，我们就会用上迭代器模式。
比如下面的代码：

```
var number = 0;
var arr = [1,2,3];
//我们将数组的每个数字都加到number里面
for(var index in arr){
	numbner + = arr[index];
}
console.log(number);//6

```

上面的方法就是迭代器模式的基础。
但是，我们试想一下，然后我们要对数组进行其他的操作呢？怎么办，难道又要再写一段类似上面的代码？很显然，上面代码的可复用性很低，很重要的原因是，上面的代码既承担了遍历数组的工作，也承担了对数组进行操作的工作。
既然知道了代码可复用性低的原因，我们就试着将代码的功能分离，提高代码的可用性，这就是迭代器模式出现的原因。

* 例子代码：

```
//我们写一个类似jquery中each方法的迭代器。
			function each(arr,callback){
				for(var index in arr){
					callback(index,arr[index]);
				}
			}
			function show(index,value){
				console.log(index+':'+value);
			}
			each([1,2,3],show);//控制台输出0:1,1:2,2:3
			
```
上面的代码通过制造出一个迭代器，专门用于迭代数组，同时迭代器接收一个callback参数接收处一个理数组中下标和值的函数。通过这么一个迭代器，我们分离了平时经常用到的for循环中纠缠在一起的迭代功能和处理功能。

* 外部迭代器

上面展示的方法是一个内部迭代器，我们可以可以生成一个外部迭代器去手动遍历数组和类数组对象。

```
			var Iterator = function(arrayLike){
				var current = 0;
				var length = arrayLike.length;
				var isDone = function(){
					return current >= length;
				};
				var next = function(){
					current += 1;
				};
				var getCurrent = function(){
					return arrayLike[current];
				};
				return {
					next:next,
					isDone:isDone,
					getCurrent:getCurrent
				};
			};
			
			iterator_first = Iterator([1,2,3]);
			function iteratorEach(iterator,callback){
				
				while(!iterator.isDone()){
					callback(iterator.getCurrent());
					iterator.next();
				}
			}
			iteratorEach(iterator_first,function(value){
				console.log(value);
			});		//控制台依次输出1,2,3
			
```

使用外部迭代器，我们可以手动调用next方法来遍历数组。外部迭代器出了可以遍历数组对象，还可以遍历类数组对象。比如：{0:'a',1:'b',2:'c','length':3},这种类似数组对象，也可以通过先转换成iterator对象，然后遍历对象中的每一项数据。

迭代器模式适用于当我们需要多次对不同对象进行相同的操作的情景。比如我们要判断浏览器中的某个特性，我们可以将测试这些特性的方法放到数组中，然后依次进行测试。
				