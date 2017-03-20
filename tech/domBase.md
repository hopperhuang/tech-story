#DOM初探

本文章只讲述关于DOM的基本知识，因为DOM在兼容性，DOM1、DOM2的情况实在太复杂，单靠一篇文章根本无法讲述完毕，笔者会再以后特别开一个关于DOM的主题细细讲述。今天就只讲述DOM的一些基本知识。

* 节点属性

```

//每个节点都有三个基本属性，表示这个节点是什么节点，节点表示的代码，和节点的内容
//首先我们获取一个节点
var body  = document.body;

//节点的基本属性有nodeType,nodeValue,nodeName
//nodeType节点返回的是改节点代表的代码
console.log(body.nodeType);//控制台输出1

```
具体关于nodeType的只是可以点[这里](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nodeType)查看，mdn上有更加详细的表述，笔者在这里就不细说了。

```
//nodeName属性
//nodeName属性，范围的是节点的名字
var body = document.body;
console.log(body.nodeName);//控制台输出'BODY',nodeName属性会返回节点的名字，也就是标签的名字

//nodeValue属性
var text = document.createTextNode('test');
console.log(test.nodeValue);//控制台输出'test';

//nodeValue属性返回的是节点的值，一般情况下属性节点和文本节点会范围自身的值，其他节点会返回null
```
更多关于nodeValue返回值的问题可以看[这里](https://developer.mozilla.org/en-US/docs/Web/API/Node/nodeValue)

* 子节点

通过childNodes属性和hasChildNode()方法，可以知道子节点的详细情况

```
var body = document.body;
var child = body.childNodes;//访问childNodes属性可以获得一个NodeList
var first = child/[0]; // 通过下标，可以访问到相应的节点
var para = document.getElementById('para');//获取了一个id为para的节点
body.hasChild(para);//返回一个布尔值，判断特点的子节点是否在节点里面
```

* 属性节点

通过attributes属性和hasAttribute(),方法可以知道节点属性

```
var para = document.getElementById('para');//获取了一个id为para的节点

//访问属性节点列表
var attr = para.attributes;//返回一个属性节点列表
var firstAttr = attr/[0];//通过下标属性可以访问到具体的属性节点
var id = attr/['id'];//直接访问具体的属性
para.hasAttribute('id');//判断节点是否具有特定的属性
para.getAttribute('id');//通过getAttribute方法获取特定的属性
para.setAttribute('id','newid');//通过setAttribute方法设置特定的属性
```


* 访问元素节点中的内容

访问元素节点的内容一般会用得到innerHTML方法和nodeValue属性
我们假设下面有这么一段html代码：

```
<p id = 'para'>test</p>

//获取para节点
var para = document.getElementById('para');

//通过innerHTML方法可以直接获取para节点内的html内容
content = para.innerHTML;

//通过nodeValue方法获取para里面的文本，但是这样，我们先要获取包含文本的文本节点
var first = para.firstChild
var text = first.nodeValue;

//这两种方式除了可以获取相关属性，还可以改写相关属性
//但是这两者的不同之处在于innerHTML可以插入标签，而nodeValue只可以插入文本

para.innerHTML = '<em>test<em>';//你可以看到加粗的字体
first.nodeValue = '<em>test<em>';//你可以看到para里面的文本内容办成了'<em>test<em>'
```

* 访问dom节点的方法
访问dom节点的方法一般有3个，分别是getElementById(),getElememtsByTagName,getElementsByName();

```
var para = document.getElementById('id');//通过id属性获取节点
var paras = document.getElementsByTagName('p');//通过标签名称获取元素节点，返回一个nodeList,
var getNames = document.getElementByName('abc');//通过标签的name属性获取元素节点，返回一个nodeList
```

* 访问兄弟节点的方法

```
var para = document.getElementById('para');
para.nextSibling;//访问与para相邻的下一个节点
para.previousSiblig;//访问与para相邻的上一个节点
```

* 访问首尾节点的方法

```
var firstChild = document.body.firstChild;//首个节点
var lastChild = document.body.lastChild;//最后一个节点
```

* 访问节点的样式

dom为js访问dom中的样式提供了width接口

```

var width = document.getElementId('para').style.width;//通过style接口和'.'方法，可以访问并更改具体的样式；
```

* 操作节点的方法

```
//创建节点
var para = document.createElement('p');
//创建文本节点
var text = document.createTextNode('test');
//新增节点
para.appendChild(text);
//删除节点
para.removeChild(text);

//在特定节点前新增节点
var anotherText = document.createTextNode('another node');
para.insertBefore(anoterText,text);//可以看到'another node'被插入到'test'前面

//克隆节点
var another = para.cloneChild();//复制了一个节点

```

* 事件方法

设置元素的事件方法

```
//一是改变元素的on属性
var para = document.getElementById('para');
para.onclick = function(){alert('click');};//通过改写On属性可以为元素添加方法

//二是通过addEventListener方法
para.addEventListener('click',function(e){alert('click');},false);
//addEventListenr方法接收三个参数，第一个参数是要绑定的事件，第二个参数是回调函数，第三个参数确定时间是否冒泡

//移除监听的方法
var func = function(e){console.log('click');};
para.addEventListener('click',func,false);//监听click事件
para.removeEventListener('click',func);//移除click方法上func回调函数
//注意，如果添加监听回调函数的时候是一个匿名函数的话，该事件无法移除

//阻止默认行为
//像某些时间是有默认行为的，比如一个submit按钮被点击的时候就会提交表单
//如果想要阻止表单的提交，我们就可以这些做,在写回调函数时这样写
function(e){
	e.preventDefault();
}
//通过preventDefault方法，我们阻止了默认事件的发生

