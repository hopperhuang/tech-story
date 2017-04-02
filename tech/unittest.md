# 单元测试

在前端开发中，单元测试是很重要的一部分。接下来就来讲一下怎么配置单元测试文件

在单元测试中，我们一般会用到chai和mocha两个模块。mocha 是需要全局安装的，而chai一般只会安装在项目目录下，通常我们也会在项目下安装多一次mocha

配置测试环境操作如下：
首先，全局安装mocha: npm install mocha -g
然后进入项目目录，安装mocha 和 chai：
cd myproject
npm install mocha --save-dev
npm install chai --save-dev

配置好上面的来嗯个模块后，我们就可以进行基本的操作。测试的结构基本如下：describe..it...

比如，你在项目中创建了一个test.js文件。
在文件中写：

```
var chai = require("chai");
var expect = chai.expect;//引入chai中的断言库，这个就看个人风格了，我个人比较喜欢expect
describe("test mocha",function(){//定义本次要测试的内容
  it("first test",function(){//定义第一此测试
      expect(1).to.be.equal(1);
    });
  });

  ```

  然后我们在命令行里面执行 mocha test.js ，就可以看到测试的情况

  * 测试目录

  mocha会自动检测项目下的test文件夹，所以我们可以把test.js放到test文件夹中去，mocha会自动测试test文件夹里面的文件.
  运行 mocha , 可以看到即使没有输入文件名称，也会有测试结果，因为mocha指定了test为测试目录。

  * 一些有用的指令

  --recursive
  --watch

  mocha 会测试test文件夹第一层的文件,但是，如果文件夹里面还有文件夹,mocha就不会测试第二层，文件夹的js文件了。但是我们可以添加一个--recursive命令。
  运行 mocha --recursive ，你可以看到，test文件夹里面的所有文件都会执行。

  如果你不想每次写完测试文件都运行一次的话，可以加上--watch命令，--watch命令会让mocha，持续监控文件，文件一旦修改，控制台就会自动刷新结果。

  * 兼容es2015语法

  暂时来说，某些es2015的语法是不支持的，因此我们需要用到babel来转码

  首先在项目目录安装相关模块

  npm install babel-core --save-dev
  npm install babel-preset-2015 --save-dev

  //如果要用jsx
  npm install babel-preset-react --save-dev
  //如果要用es7
  npm install babel-preset-stage-0 --save-dev

  安装好后，我们还要安装配置好babel的文件，生成一个.babelrc的文件。并在文件里面写如：

  ```
  {
    'presets':['es2015']
  }
  ```
  如果你想要使用jsx和es7的语法，在数据里面添加想要使用的react或者stage-0，就可以了。

  最后，之需要在测试的时候加入指令, --compilers js:babel-core/register

  上面的指令告诉mocha利用babel-core编译js文件。


  为了避免每次都写一大串常的命令，我们会在项目的package.json配置文件里面写道


  ```
  "scripts": {
    "test": "mocha --watch --recursive --compilers js:babel-core/register"
  }
  ```

  更多关于测试和转码的知识在这里：
  1. [mocha](http://mochajs.org/)
  2. [chai](http://chaijs.com/)
  3. [babel](http://babeljs.io/)
  4. [how to use mocha with babel](http://jamesknelson.com/testing-in-es6-with-mocha-and-babel-6/)
  5. [ruanyifeng](http://www.ruanyifeng.com/blog/2015/12/a-mocha-tutorial-of-examples.html)
