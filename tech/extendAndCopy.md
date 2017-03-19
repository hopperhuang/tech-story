#超类继承与复制

## 超类继承

在之前的文章中已经介绍过继承的方法，因为在js中并没有类的说法，构造类只能通过构造函数和继承。有时候在调用一个方法时，我们需要同时调用父类的方法，这时就需要超类继承了。今天我们就来讲解一下超类继承。

超类继承的关键在于子类对象，为访问父类对象提供一个借口，接下来，我们写一个超类继承的方法：

```

function extend(child,parent){
	child.prototype = Object.create(parent.prototype);//将子类的原型指向父类的原型
	child.prototype.constructor = child;//将子类构造函数的constructor属性重新指向子类本书
	child.uber = parent.prototype;// 在子类构造函数中，提供一个uber借口，子类可以通过uber借口访问到父类的属性
}

//Shape 构造函数
function Shape(){}
Shape.prototype.name = 'shape';
Shape.prototype.toString = function(){
	if(this.constructor.uber){
		return this.constructor.uber.toString() + ','+ this.name;
	}	else {
		return this.name;
	}
}

//TwoDeeShape 构造函数
function TwoDeeShape(){}

//创建超类继承
extend(TwoDeeShape,Shape);
//重写name属性
TwoDeeShape.prototype.name = '2dshape';

//Triangle构造函数
function Triangle(){}
//超类继承
extend(Triangle,TwoDeeShape);
//重写name 属性
Triangle.prototype.name = 'triangle';

var triangle = new Triangle();
console.log(triangle.toString());//控制台输出shape,2dshape,triangle

//上面的代码实现了超类继承，可以看到toString函数执行时，调用父类的name对象
//在写超类继承时，有几点是很关键的。
//一是为访问父类提供接借口,并接接口连接到父类的原型链上child.uber = parent.prototype这个语句实现了这个功能
//二是重写了constructor对象
//三是在需要调用父类的行为和方法中，增加一个是否连接到父类的判定,if(this.constructor.uber)
//从上面的这个判定语句，可以看到重新constructor对象的重要性,构造函数生成的实例triangle可以通过constructor属性访问到其构造函数的父类
//而我们将之类的uber指向了父类的prototype，在调用时，父类可以直接访问其prototype上的construtor属性,从而判断，父类是否还有父类
//四是我们会将需要调用的属性写在prototype上，因为每次调用父类，实际上是访问父类的prototype，所以要把需要的属性写在prototype上

```

上面介绍了构造函数之间的超类继承，下面来看看对象之间的超类继承:

```
var shape = {};
shape.name = 'shape'
shape.toString = function(){
	if(this.uber){
		return this.uber.toString() + ',' + this.name
	}	else {
		return this.name;
	}
};

//根据shape对象，创建一个twoDeeShape对象

var twoDeeShape = Object.create(shape);
twoDeeShape.name = '2dshape';
twoDeeShape.uber = shape;//为访问父对象提供接口

//根据2dshape创建一个triangle对象

var triangle = Object.create(twoDeeShape);
triangle.name = 'triangle';
triangle.uber = twoDeeShape;//为访问父类对象提供接口

console.log(triangle.toString());//控制台输出shape,2dshape,triangle

//上面的方法实现了对象见的超类继承和调用
```

总结并比较一个构造函数和对象之间的超类继承的共同点，一就是都为访问超类提供了uber接口，uber接口都写在自身，比如child.uber = parent.prototype语句和 a.uber = b语句。二就是uber接口都是指向构造它的原型对象。
不同点在于，调用的时候，构造函数通过constructor.uber来判定是否有超类继承，因为construtor属性，构造函数的prototype和实例都可以调用。而对象则直接判断uber属性是否存在。

## 复制

上面讲完了超类继承，接下来就讲一下复制。

复制就是把一个对象的属性赋予另一个对象，复制又分为简单复制和深度复制。简单复制和深度复制的差别在于复制对象的时候。我们假设有o1,o2两个对象，o2对象的a属性是一个对象，简单复制则是直接吧o1.a指向o2.a，而深度复制则是在o1.a新建一个对象，只是这个对象的属性与o2.a完全相同，两者并没有任何关联。下面就来看看简单复制和深度复制。

简单复制:

```
function lightCopy(from,to){
	var to = to;
	for(var index in from){
		to[index] = from[index];
	}
	return to;
}

var xiaoming= {name:'xiaoming'};
var student = {skills:['dohomework','play']};
var xiaomingStudent = lightCopy(student,xiaoming);
console.log(xiaomingStudent.name);
console.log(xiaomingStudent == xiaoming);
console.log(xiaoming.skills == student.skills);\
//从上面代码可以看到xiaoming.skills是对student.skills的引用

```

深度复制

```

function deepCopy(from,to){
	for(var index in from){
		if(typeof from[index] === 'object'){
			to[index] = (from[index].constructor === Array)? []:{};
			return deepCopy(from[index],to[index]);
		}	else {
			to[index] = from[index];
		}
	}
}

var xiaoming= {name:'xiaoming'};
var student = {skills:['dohomework','play']};
deepCopy(student,xiaoming);
console.log(xiaoming.skills);//控制台输出['dohomework','play']
console.log(xiaoming.skills == student.skills);//控制台输出false

//从控制台的输出可以看到，xiaoming.skills并不是对student.skills属性的引用。
```
