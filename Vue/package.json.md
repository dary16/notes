### bin ###
它是一个命令名和本地文件名的映射。在安装时，如果是全局安装，npm将会使用符号链接把这些文件链接到prefix/bin，如果是本地安装，会链接到./node_modules/.bin/

通俗点讲就是全局安装， 我们就可以在命令行中执行这个文件， 本地安装我们可以在当前工程目录的命令行中执行该文件。

### main ###
main很重要，它是我们项目的主要入口
```javascript
"main":"app.js"
```

### scripts ###
npm 允许在package.json文件里面，使用scripts字段定义脚本命令。优点：项目的相关脚本，可以集中在一个地方

npm 脚本的原理非常简单。每当执行npm run，就会自动新建一个 Shell，在这个 Shell 里面执行指定的脚本命令。因此，只要是 Shell（一般是 Bash）可以运行的命令，就可以写在 npm 脚本里面。
比较特别的是，npm run新建的这个 Shell，会将当前目录的node_modules/.bin子目录加入PATH变量，执行结束后，再将PATH变量恢复原样。
这意味着，当前目录的node_modules/.bin子目录里面的所有脚本，都可以直接用脚本名调用，而不必加上路径。比如，当前项目的依赖里面有 Mocha，只要直接写mocha test就可以了

### *通配符 ###
*表示任意文件名，**表示任意一层子目录。
```javascript
"lint": "jshint *.js"
"lint": "jshint **/*.js"
```

如果要将通配符传入原始命令，防止被 Shell 转义，要将星号转义。
```javascript
"test": "tap test/\*.js"
```

### 脚本传参符号： -- ###
```javascript
"server": "webpack-dev-server --mode=development --open --iframe=true ",
```

### 脚本执行顺序 ###
并行执行（即同时的平行执行），可以使用&符号
```javascript
$ npm run script1.js & npm run script2.js
```

继发执行（即只有前一个任务成功，才执行下一个任务），可以使用&&符号
```javascript
$ npm run script1.js && npm run script2.js
```

### 脚本钩子 ###

npm 脚本有pre和post两个钩子， 可以在这两个钩子里面，完成一些准备工作和清理工作

### 拿到package.json的变量 ###
npm 脚本有一个非常强大的功能，就是可以使用 npm 的内部变量。

首先，通过npm_package_前缀，npm 脚本可以拿到package.json里面的字段。比如，下面是一个package.json。

```javascript
{
  "name": "foo", 
  "version": "1.2.5",
  "scripts": {
    "view": "node view.js"
  }
}
```
我们可以在自己的js中这样：
```javascript
console.log(process.env.npm_package_name); // foo
console.log(process.env.npm_package_version); // 1.2.5
```

### 常用脚本 ###

```javascript

// 删除目录
"clean": "rimraf dist/*",

// 本地搭建一个 HTTP 服务
"serve": "http-server -p 9090 dist/",

// 打开浏览器
"open:dev": "opener http://localhost:9090",

// 实时刷新
 "livereload": "live-reload --port 9091 dist/",

// 构建 HTML 文件
"build:html": "jade index.jade > dist/index.html",

// 只要 CSS 文件有变动，就重新执行构建
"watch:css": "watch 'npm run build:css' assets/styles/",

// 只要 HTML 文件有变动，就重新执行构建
"watch:html": "watch 'npm run build:html' assets/html",

// 部署到 Amazon S3
"deploy:prod": "s3-cli sync ./dist/ s3://example-com/prod-site/",

// 构建 favicon
"build:favicon": "node scripts/favicon.js",
```


