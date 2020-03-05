### Websocket的通信原理和机制

既然是基于浏览器端的web技术，那么它的通信肯定少不了http，websockt本身也是一种应用层协议，但是它不能脱离http而单独存在。具体来讲，我们在客户端构建一个websocket实例，并且为它绑定一个需要连接的服务器地址，当客户端连接服务端的时候，会向服务端发送一个类似下面的http报文
```javascript
GET ws://echo.websocket.org/?encodeing=text HTTP/1.1
Origin:http://websocket.org
Cookie:__utma=99as
Connection:Upgrade
Host: echo.websocket.org
Sec-Websocket-Key: uRovscZjNol/umbTt5uKmw==
Upgrade: websocket
Sec-WebSocket-Version: 13
```
可以看到，这是一个http get请求报文，该报文有一个uograde首部，它的作用是告诉服务端需要将通信协议切换到websocket，如果服务端支持websocket协议，那么它将会将自己的通信协议切换到websocket，同时发送给客户端类似于一下的一个相应报文头
```javascript
HTTP/1.1 101 WebSocket Protocol Handshake
Date: Fri,10 Feb 2020 9:02:34 GMT
Connection: Upgrade
Server: Kaazing Gateway
Upgrade: WebSocket
Access-Control-Allow-Origin: http://websocket.org
Access-Control-Allow-Credentials:true
Sec-WebSocket-Accept:rLKkld/Sksfdjk/DJSAKDjdf=
Access-Control-Allow-Headers:content-type
```

返回的状态码为101，表示同意客户端协议转换请求，并将它转换为websocket协议。以上过程都是利用http通信完成的，称之为websocket协议握手，通过这握手之后，客户端和服务端就建立了websocket连接，以后的通信走的都是websocket协议了。所以总结为websocket握手需要借助于http协议，建立连接后通信过程使用websocket协议。同事需要了解的是，该websocket连接还是基于我们刚才发起http连接的那个TCP连接。一旦建立连接之后，我们就可以进行数据传输了，websocket提供两种数据传输：文本数据和二进制数据

基于以上分析，websocket能够提供低延迟，高性能的客户端和服务端的双向数据通信。它颠覆了之前web开发的请求处理相应模式，并且提供了一种真正意义上的客户端请求，服务器推送数据的模式，特别适合实时数据交互应用开发。
