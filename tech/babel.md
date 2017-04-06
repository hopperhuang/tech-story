# 利用babel进行转码

* 安装bebel-cli
babel是用来将es6转换成es5，以便在浏览器执行的模块。在使用babel进行转码时，我们首先要安装babel-cli。一般我们会在全局环境安装一次，在项目环境安装一次。

```
npm install babel-cli -g
cd myproject 
npm install babel-cli --save-dev
```

* 安装preset

安装babel-cli 后，还要安装想要转码的presets(也就是我们想要转码的类型)，比如在我的项目里面，我要要转码的是es2015,所以在我的项目里面我就安装了babel-preset-es2015

安装preset:

```
npm install babel-preset-es2015 --save-dev
//下面这个是react的
npm install babel-preset-react --save-dev 
//下面这个是es7的
npm install babel-preset-stage-0 --save-dev

```

* 配置.babelrc文件

安装好上面的模块后，我们还要配置好.babelrc文件。在项目的根目录下，创建一个babelrc文件，然后加入下面规则，因为上面说到我是要用到es2015,react和es7的，所以我在我的.babelrc文件加入下面的规则

```
{
	"presets":["es2015","react","stage-0"],
	"plugins":[]
}
```

* 转码

配置好上面的环境后，我们就可以在命令行利用babel命令进行转码。

基本语法如下：

```
babel index.js //这个语法会直接将转码后的代码输出到命令行
babel index.js -o translate.js //这个语法会生成新的转码文件
babel src -d lib //这个语法会将src文件夹里面的js文件转码，然后放置到lib文件夹里面。
```

为了方便，我们一般在package.json这样写：

```
{
...
  "scripts": {
    "build": "babel src -d lib"
  }
}
```

配置好后，只要在命令行输入
```
npm run build
```
就可以自动进行转码，不用每次手动输入这么麻烦。

* 加入babel-polyfill

Babel默认只转换新的JavaScript句法（syntax），而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。

所以，如果想要使用新的语法，我们就要加入babel-polyfill模块。安装模块：

```
npm install babel-polyfill --save
```

安装后，我们需要在使用新语法的文件的头部引入babel-polyfill

```
import 'babel-polyfill';
```

* 实现实时转码（仅限开发环境使用）

实现实时转码，我们一般会引入babel-register模块,安装模块：

```
npm install babel-register --save-dev
```

然后再需要的时候，引入babel-rigister和需要专门的文件

```
require("babel-register");
require("./index.js");
```


