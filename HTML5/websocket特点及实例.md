### Websocket的特点
- 建立在TCP协议之上，服务器端的实现比较容易
- 与HTTP协议有着良好的兼容性。默认端口是80和443，握手阶段采用HTTP协议，因此握手时不容易屏蔽，能通过各种HTTP代理服务器
- 数据格式比较轻量，性能开销小，通信高效
- 可以发送文本，也可以发送二进制数据
- 没有同源限制，客户端可以与任意服务器通信
- 协议标识符是ws或wss，服务器网址就是URL

```javascript
ws://127.0.0.1:80/some
```

### 客户端的简单示例

下面是一个网页脚本的例子：
```javascript
var ws = new WebSocket('wss://echo.websocket.org');

ws.onopen = function(evt){
  console.log('Connection open ...');
  ws.send('Hello WebSocket');
};

ws.onmessage = function(evt){
  console.log("Received Message:"+ evt.data);
  ws.close();
};

ws.onclose = function(evt){
  console.log('Connection closed');
}
```

### WebSocket客户端的API

#### WebSocket构造函数
WebSocket对象作为一个构造函数，用于新建WebSocket实例
```javascript
var ws = new WebSocket('ws://localhost:8080');
```
执行上面语句之后，客户端就会与服务端进行连接

#### webSocket.readyState
readyState属性返回实例对象的当前状态，共有四种
- CONNECTIONHG：值为0，表示正在连接
- OPEN：值为1，表示连接成功
- CLOSING：值为2，表示连接正在关闭
- CLOSED：值为3，表示连接已经关闭，或者打开连接失败

示例：
```javascript
switch (ws.readyState) {
  case WebSocket.CONNECTING:
    // do something
    break;
  case WebSocket.OPEN:
    // do something
    break;
  case WebSocket.CLOSING:
    // do something
    break;
  case WebSocket.CLOSED:
    // do something
    break;
  default:
    // this never happens
    break;
}
```

#### webSocket.onopen
示例对象的onopen属性，用于指定连接成功后的回调函数
如果要指定多个回调函数，可以使用addEventListener方法
```javascript
ws.addEventListener('open',event=>{
  ws.send('Hello Server');
})
```

#### webSocket.onclose
示例对象的onclose属性，指定连接关闭后的回调函数

```javascript
ws.onclose = function(event){
  var code = event.code;
  var reason = event.reason;
  var wasClean = event.wasClean;
};

ws.addEventListener('close',event=>{
   var code = event.code;
  var reason = event.reason;
  var wasClean = event.wasClean;
})
```

#### webSocket.onmessage
示例对象的onmessage属性，用于指定收到服务器收据后的回调函数
```javascript
ws.onmessage = function(event){
  var data = event.data;
}

ws.addEventListener('message',event=>{
   var data = event.data;
})
```
注意，服务器数据可能是文本，也可能是二进制数据
```javascript
ws.onmessage = function(event){
  if(typeof event.data === String) {
    console.log("Received data string");
  }

  if(event.data instanceof ArrayBuffer){
    var buffer = event.data;
    console.log("Received arraybuffer");
  }
}
```
除了动态判断收到的数据类型，也可以使用binaryType属性，显式指定收到的二进制数据类型
```javascript
// 收到的是 blob 数据
ws.binaryType = "blob";
ws.onmessage = function(e) {
  console.log(e.data.size);
};

// 收到的是 ArrayBuffer 数据
ws.binaryType = "arraybuffer";
ws.onmessage = function(e) {
  console.log(e.data.byteLength);
};
```

#### webSocket.send()
实例对象的send()方法用于向服务器发送数据
发送文本的例子
```javascript
ws.send('message')
```
发送Blob对象的例子：
```javascript
var file = document.querySelector('input[type="file"]').file[0];
ws.send(file)
```
发送ArrayBuffer对象的例子：
```javascript
// Sending canvas ImageData as ArrayBuffer
var img = canvas_context.getImageData(0, 0, 400, 320);
var binary = new Uint8Array(img.data.length);
for (var i = 0; i < img.data.length; i++) {
  binary[i] = img.data[i];
}
ws.send(binary.buffer);
```

#### webSocket.bufferedAmount
实例对象的bufferedAmount属性，表示还有多少字节的二进制数据没有发送出去。它可以用来判断发送是否结束。
```javascript
var data = new ArrayBuffer(10000000);
socket.send(data);

if (socket.bufferedAmount === 0) {
  // 发送完毕
} else {
  // 发送还没结束
}
```

#### webSOcket.onerror
实例对象的onerror属性，用于指定报错时的回调函数
```javascript
socket.onerror = function(event) {
  // handle error event
};

socket.addEventListener("error", function(event) {
  // handle error event
});
```
