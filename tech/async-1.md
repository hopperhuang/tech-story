#Promise -- async--await异步流程控制（一）

在服务端程序中，经常涉及到读写I/O操作，这时就需要对异步流程作控制。在讲述Promise -- async -- await怎么作异步流程控制前，我们首先需要知道什么是异步。我们下面就先来简单说一下。

模拟一个场景，在一段服务器程序中，硬盘需要写入一个文件。如果写入文件的时候，整个服务端程序都停下来，等待这个文件写入完成再继续运行就叫同步。如果是再写入过程中，程序一直运行，与写入文件相关的操作则在写入文件完成后再继续，就叫做异步。

再JS中实现异步的解决防会采用回调函数。例如下面的例子。

```

var fs = require('fs');
fs.readFile('a.txt',function(err,buffer){
  buffer.toString();
});
```

再上面的例子中，我们读取了a.txt文件，读取完成成后对buffer对象进行toString()操作。

读写一个文件我们可以这样写，想要读取完一个文件之后再读取第二个文
件，然后再读取多个文件呢？？我们可能就会写出以下的代码：

```

var fs = require('fs');
fs.readFile('a.txt',function(err,buffer){
  fs.readFile('b.txt',function(err,buffer){
    fs.readFile('c.txt',function(err,buffer){
      ......
      ......
      ......
    });
  });
});
```

当我们需要读取多个文件的时候，我们就会写出上面的代码，没错，你已经陷入了我们Node.jser常说的回调陷阱了。这样一个嵌套一个的回调函数既不美观，而且让看起来也很混乱。

为了让读写的流程整洁，好看，美观，es6中又引入Promise对象。具体关于Promise对象的只是我们就不再这里详细讲述了。如果想要实现刚刚例子中，等待一个文件读取完成后再读取下一个文件的效果，利用Promise可以这样写。

```

var fs = require("fs");
var p = new Promise(function(resolve,reject){
  fs.readFile('a.txt',function(err,buffer){
    if(!err){
      resolve(buffer);
    }
  });
});

p()
.then(function(){
  fs.readFile('b.txt',function(err,buffer){
    if(!err){
      return buffer;
    }
  })
})
.then(function(){
  fs.readFile('c.txt',function(err,buffer){
    if(!err){
      return buffer;
    }
  })
})
......
......

```

这样一个个then操作，一步一步写下来。再实际工作中，如果需要读写的文
件多起来的话，这段代码就会变得很长，不容易管理。

而async--await 则可以让这一切变得很简单

```

async funcion readSomeFiles (){
  var file = ['a.txt','b.txt','c.txt'...]; //多个数组
  for(var i = 0; i < file.length){
    await fs.readFile(file[i]);
  }
}
readSomeFiles();
```
再上面的async函数中 ，await命令会等待它后面的异步操作完成之后，再进行函数中后面的操作，也就是说，在一段async函数中，当遇到await时，函数的执行权会交给这个异步操作，待这个异步操作完成后，函数的执行权才会交还给async函数。通过这么一个手段，async实现了异步流程控制，既精简、又美观，而且async..await更加符合语义。


