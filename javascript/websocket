使用node.js配置简单服务器
1.安装node.js
2.安装ws模块,npm install --save ws
3.实现服务端
```javascript
var ws = require('ws').Server;

var server = new ws({
    host: "127.0.0.1",
    port: 8090
});
server.on('connection', function(ws) {
    console.log('connected!');
    ws.on('message', function(message) {
        console.log(message.toString());
    });

    ws.on('close', function() {
        console.log("disconnected...");
    });
});

console.log('websockect-server running...');
```
4.启动服务：把上面的代码保存成server.js,运行node server.js,服务器搭建完成
5.连接服务器：
```javascript
const wsuri = 'ws://127.0.0.1:8090/';
ws.onopen = function(){
  connection.innerHTML = '服务器正常<br/>'
}
ws.onmessage = function(e){
  var _str = e.data;
  var data = JSON.parse(_str.toString('utf-8'));
  console.log(data);
}
```
随便弄个html,插入上面的js,运行就成功了
