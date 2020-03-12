### Babel转码器
babel是一个广泛使用的ES6转码器，可以将ES6代码转为ES5代码，从而在老版本的浏览器执行。这意味着，可以用ES6的方式编写代码，下面是一个例子：
```javascript
//转码前
input.map(item=>item+1);

//转码后
input.map(function(item){
  return item + 1;
});
```

上面的原始代码就用了箭头函数，Babel将其转换为普通函数，就能在不支持箭头函数的环境执行了
下面的命令在项目的目录中，安装Bable
```javascript
$ npm install --save-dev @babel/core
```

### 配置文件.babelrc
Babel的配置文件是.babelrc，存放在项目的根目录下。使用的第一步，就是配置这个文件。
该文件用来设置转码规则和插件，基本格式如下：
```javascript
{
  "presets":[],
  "plugins":[]
}
```
presets字段设定转码规则，官方提供一下的规则集，可以根据需要安装
```javascript
# 最新转码规则
$ npm install --save-dev @babel/preset-env
```
然后将这些规则加入.babelrc
```javascript
{
  "presets":[
    "@babel/env"
  ],
  "plugins":[]
}
```
注意：以下所有工具和模块的使用，必须先写好.babelrc

### 命令行转码
Babel提供命令行工具@babel/cli，用于命令行转码
它的安装命令如下：
```javascript
$ npm install --save-dev @babel/cli
```

基本用法如下：
```javascript
# 转码结果输出到标准输出
$ npx babel example.js

# 转码结果写入一个文件
# --out-file 或 -o 参数指定输出文件
$ npx babel example.js -o compile.js

# 整个目录转码
# --out-dir 或 -d 参数指定输出目录
$ npx babel src --out-dir lib
# 或者
$ npx babel src -d lib

# -s参数生成source map文件
$ npx babel src -d lib -s
```

### babel-node
@babel/node模块的babel-node命令，提供一个支持ES6的REPL环境。它支持Node的REPL环境的所有功能，而且可以直接运行ES6代码
安装这个模块
```javascript
$ npm install --save-dev @babel/node
```
然后，执行babel-node就进入 REPL 环境
```javascript
$ npx babel-node
> (x => x * 2)(1)
2
```

babel-node命令可以直接运行 ES6 脚本。将上面的代码放入脚本文件es6.js，然后直接运行。
```javascript
# es6.js 的代码
# console.log((x => x * 2)(1));
$ npx babel-node es6.js
2
```

### babel/register模块
@babel/register模块改写require命令，为它加上一个钩子。此后，每当使用require加载.js、.jsx、.es和.es6后缀名的文件，就会先用 Babel 进行转码。
```javascript
$ npm install --save-dev @babel/register
```
使用时，必须首先加载@babel/register。
```javascript
// index.js
require('@babel/register');
require('./es6.js');
```
然后，就不需要手动对index.js转码了
```javascript
$ node index.js
2
```
需要注意的是，@babel/register只会对require命令加载的文件转码，而不会对当前文件转码。另外，由于它是实时转码，所以只适合在开发环境使用

### polyfill
Babel 默认只转换新的 JavaScript 句法（syntax），而不转换新的 API，比如Iterator、Generator、Set、Map、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。

举例来说，ES6 在Array对象上新增了Array.from方法。Babel 就不会转码这个方法。如果想让这个方法运行，可以使用core-js和regenerator-runtime(后者提供generator函数的转码)，为当前环境提供一个垫片。

安装命令如下：
```javascript
$ npm install --save-dev core-js regenerator-runtime
```
然后，在脚本头部，加入如下两行代码。
```javascript
import 'core-js';
import 'regenerator-runtime/runtime';
// 或者
require('core-js');
require('regenerator-runtime/runtime);
```
Babel 默认不转码的 API 非常多，详细清单可以查看babel-plugin-transform-runtime模块的definitions.js文件。










