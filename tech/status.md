# 状态模式

状态模式适用于几种状态切换的情景。

具体的实现如下：

```
//status patten
function Light() {

}
Light.prototype.status = (function(){
  //状态管理器
  var current = {};
  //需要执行的行为
  var first = function () {
    console.log('first');
    //将状态切切换到下一个状态。
    current.status = second;
  };
  var second = function () {
    console.log('second');
    current.status = third;
  };
  var third = function () {
    console.log('third');
    current.status = first
  };
  current.status = first;
  return current;
})();
Light.prototype.next = function(){
  this.status.status();
};
var light = new Light();
light.next();//'first'
light.next();//'second'
light.next();//'third'

```

状态模式主要由三部分组成，一个是它的状态管理器，一个是不同状态的不同行为。重点是要在执行行为的时候，将状态管理器的状态切换到下一个状态。

个人认为，状态模式的与数据结构中的链表的思想十分相似，都是可以指向下一个对象，都是可以将状态在多个对象之间循环，大家可以理解一下。
链表的实现：
```
var list = (function(){
  var current = 0;
  var chain = [];
  var max = function(){
    return chain.length;
  };
  var getCurrent = function () {
    return chain[current];
  };
  var next = function(){
    current++;
    current = (current >= max())? 0 : current;
  };
  var add = function(object){
    return chain.push(object);
  };
  var del = function(object){
    for(var index = 0; index < chain.length;index++){
      if(chain[index] === object){
        chain.splice(index,1);
        current = (current >= max())? current-1:current;
      }
    }
  };
  return {
    add:add,
    getCurrent:getCurrent,
    del:del,
    next:next
  };
})();
//console.log(list);
var xiaoming = {name:'xiaoming'};
var xiaohong = {name:'xiaohong'};
var xiaoguang = {name:'xiaoguang'};
list.add(xiaoming);
list.add(xiaohong);
list.add(xiaoguang);
list.next();
list.next();
list.del(xiaoguang);
console.log(list.getCurrent());
```
