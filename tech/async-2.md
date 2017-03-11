#Promise -- async--await异步流程控制（二）

在上一章，我们讲述了js中处理异步流程的一些方法，并介绍了async--await的一些好处，在接下来的第二章中，我将讲述一下，async--await的使用情景、在利用express框架搭建本博客的过程中，async--await的实际利用，还有就是在对async--await用法的一些思考。

* 使用情景
在做流程控制的时候，我们需要清楚我们要控制的 I/O读写，是要并发还是继发。只有目标明确，才能确定我们再要怎么利用async--await。

1. 继发读取文件

比如我们要读取3个文件，这3个文件是要一个读完之后再读取下一个的，这时候我们就可以这样写：

```

var fs = require("fs");
var files = ['fileA','fileB','fileC'];
async function readFiles(){
  for(var i = 0 ; i < files.length; i++){
    var content = await fs.readfile(files[i],function(err,buffer){
      if(!err){
        return buffer.toString()
      }
    });
    console.log('read a file!');
  }
}
readFile();
```

再上面的流程中，文件是一个一个读取的，当函数运行到await时，控制权会交给异步操作，函数中的for循环会停下来，只有await 后面的异步操作完成了，函数才会继续，完成for循环中其他的语句，并进入下一个循环。

2. 并发操作

再实际工作中，一次读取一个文件有时候会显得效率很低，这时候我们需要一次读取多个文件，这个就时并发操作。

```

并发场景中，我们可以这样组织语言。
var fs = require("fs");
var files = ['fileA','fileB','fileC'];
files.map(async function(file){
  await fs.readFile(file);
  console.log('read a file');
});
```

要记住，await控制的是async函数里面的流程，所以map方法，是同时发生的，也就是说3个读取文件的操作是同时发生的。读取文件与后面的console语句才是继发。

3. 并发与继发并存

比如我现在有6个文件，我想先读取3个，在读取其他三个，怎么办呢？？我们可以这样组织代码：

```

var fs = require('fs');
var group_1 = ['file1','file2','file3'];
var group_2 = ['file4','file5','file6'];
async function readFiles(){
  //先读取第一组文件
  var c1  =  await Promise.all(group_1.map(async function(file){
    var content = await fs.readFile(file,function(err,buffer){
      return buffer
    });
    return buffer.toString();
  }));
  c1.then().catch();
//再读取第二组文件
  var c2 = await Promise.all(group_2.map(function(file){
    var content = await fs.readFile(file,function(err,buffer){
      return buffer;
    });
    return buffer.toString();
  }));
  c2.then().catch();
  console.log('read all files');

}
readFiles();
```

在上面的代码中，我们嵌套了两层async--await，第一层用于控制两两组文件读取的先后顺序，第二层用于控制map方法中，读取文件时的先后操作。

* 实际使用

在构建本博客的过程中，我要实现以下的操作，读取一边文章的时候，要显示这遍文章的评论数量。在得到post数据的情况下，我可以根据postId去查找出相关评论的数量，然后将这个数量添加到post的counts属性中去。

编写这个monglass插件，我们需要异步控制的过程就是，先读取评论数量，再将数量，添加到counts属性中。所以我选择async  await的方法来实现。

原来的作者用promise...then来实现，但是，我觉得语义不清晰，流程不够明确，于是我采用了Promise async await的方法来实现。

```

Post.plugin('addCommentsCount', {
  afterFind: function (posts) {
    /*（这是原来作者的写法）
    return Promise.all(posts.map(function (post) {
      return CommentModel.getCommentsCount(post._id).then(function 
(commentsCount) {
        post.commentsCount = commentsCount;
        return post;
      });
    }));
（这是原来作者的写法）
    */
    return Promise.all(posts.map(async function(post){
      var count = await CommentModel.getCommentsCount(post._id);
      post.commentsCount = count;
      return post;
    }));
  },
  afterFindOne: function (post) {
    if (post) {
  /*（这是原来作者的写法）
      return CommentModel.getCommentsCount(post._id).then(function 
(count) {
        post.commentsCount = count;
        return post;
      });
（这是原来作者的写法）
 */
      return (async function(){
        var count = await CommentModel.getCommentsCount(post._id);
        post.commentsCount = count;
        return post;
      })();
    }

return post;

  }
});
```

* 一些疑惑

有些朋友会疑惑，为什么async..await要跟all一起用呢？？那是因为，promise.all写写法便于数据的传递，而且写法美观。比如我们要读取一组文件，读取完成后，我们想将数据进行统一的加工处理。

```

var fs = require('fs')
var files = [......];
promise.all(files.map(function(file){
  return await fs.readFile(file,function(err,buffer){
    return buffer.toString();
  });
}))
.then(function(contents){

})
.catch(function(){

});
```

这样写的话，由promise.all传递到then方法的数据contents是一个string 数
组，我们可以直接对这个数组进行想要的操作。

如果不和promise.all配合。我们则需要这样写：

```

var fs = require('fs')
var files = [......];
var contents = files.map(function(file){
  return await fs.readFile(file,function(err,buffer){
    return buffer.toString();
  });
})
for(var i = 0; i < contents.length; i++){
  contents[i].then().catch()
}
```

因为cotents是一个数组，这个数组每一个元素都是promise对象。这样写起来不够美观，语义上也不够直接，不提倡这种做法。


