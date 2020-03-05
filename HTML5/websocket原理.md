### Websocket的通信原理和机制

既然是基于浏览器端的web技术，那么它的通信肯定少不了http，websockt本身也是一种应用层协议，但是它不能脱离http而单独存在。具体来讲，我们在客户端构建一个websocket实例，并且为它绑定一个需要连接的服务器地址，当客户端连接服务端的时候，会向服务端发送一个类似下面的http报文
客户端请求
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
服务器回应
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
- Connection必须设置Upgrade,表示客户端希望连接升级
- Upgrade字段必须设置Websocket，表示希望升级到Websocket协议
- Sec-WebSocket-Key 是随机字符串，服务器端会用这些数据来构造出一个SHA-1的信息摘要。把“Sec-WebSocket-Key”加上一个特殊字符串“258EAFA5-E914-47DA-95CA-C5AB0DC85B11”，然后计算SHA-1摘要，之后进行BASE64编码，将结果作为“Sec-WebSocket-Accept”头的值，返回给客户端。如此操作，可以尽量避免普通HTTP请求被误认为WebSocket协议
- Sec-WebSocket-Version 表示支持的Websocket版本。RFC6455要求使用的版本是13，之前草案的版本均应当弃用
- Origin 字段是可选的，通常用来表示在浏览器中发起此Websocket链接所在的页面，类似于Referer.但是，与Referer不同的是，Origin只包含了协议和主机名称
- 其它一些定义在HTTP协议中的字段，如Cookie等，也可以在Websocket中使用

返回的状态码为101，表示同意客户端协议转换请求，并将它转换为websocket协议。以上过程都是利用http通信完成的，称之为websocket协议握手，通过这握手之后，客户端和服务端就建立了websocket连接，以后的通信走的都是websocket协议了。所以总结为websocket握手需要借助于http协议，建立连接后通信过程使用websocket协议。同事需要了解的是，该websocket连接还是基于我们刚才发起http连接的那个TCP连接。一旦建立连接之后，我们就可以进行数据传输了，websocket提供两种数据传输：文本数据和二进制数据

基于以上分析，websocket能够提供低延迟，高性能的客户端和服务端的双向数据通信。它颠覆了之前web开发的请求处理相应模式，并且提供了一种真正意义上的客户端请求，服务器推送数据的模式，特别适合实时数据交互应用开发。
