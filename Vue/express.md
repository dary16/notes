1.安装部署express


npm install express body-parser  --save
然后在项目的根目录添加app.js 作为启动express服务器的代码
```javascript
const express = require('express')
const app = express()
app.use('/',(req,res) => {
  res.send('hello express!')
})
app.listen(3000,() => {
    console.log('app listening on port 3000.')
```
2、对vue进行打包

执行：npm run build

打包后的文件存放于dist文件夹中，vue经过webpack打包之后生成dist文件夹，里面有个index.html，他是前端页面和服务端的对接页面。

3、修改app.js

在express中加入app.use(express.static(path.join(__dirname, 'dist')))；app.js 代码如下：
```javascript
const express = require('express')
const path = require('path')
const app = express()
 
app.use(express.static(path.join(__dirname, 'dist')))
app.listen(3000,() => {
    console.log('app listening on port 3000.')
})
```
4、启动express

在启动express之前，需要修改packge.json 里面的配置：

```javascript
 "scripts": {
    "dev": "node build/dev-server.js",
    "build": "node build/build.js",
    "server": "nodemon app.js",
    "start": "node app.js"
  },
```
然后执行：npm run start
此时就可以通过127.0.0.1:3000访问到vue的欢迎界面了。
