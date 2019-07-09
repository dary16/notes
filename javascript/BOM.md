### cookie、localStorage、sessionStorage区别 ###

| 特性 | cookie | localStorage | sessionStorage |
|------|-------|------------|:-------------|
|生命周期|由服务器生成，可以设置过期时间|除非被清空，否则一直存在|页面关闭就没有|
|大小  | 4K  | 5M  |  5M  |
|与服务端通信|每次都会携带在header中，影响请求性能|不参与|不参与|

对于cookie，要注意安全性，不建议用于存储，对于不怎么改变的数据尽量使用localStorage存储，否则可以用sessionStorage存储

### 跨域 ###

定义：协议、域名、端口有一个不同就叫做跨域

解决跨域的方法：
- JSONP:利用script标签没有跨域限制来解决，兼容性不错，但只限于get请求
- CORS:需要浏览器和后端同时支持，服务端设置Access-Control-Allow-Origin就可以开启CORS,设置哪些域名可以访问资源
- Nodejs中间件代理跨域
- WebSocket协议跨域：实现了浏览器与服务器的全双工通信，同时允许跨域通讯，是server push技术的一种很好的体现。原生Websocket API使用起来不太方便，可以使用Socket.io,很好的封装了WebSocket接口，提供了更简单、灵活的接口
- nginx代理跨域：服务器端调用HTTP接口只是使用HTTP协议，不会执行JS脚本，不需要同源策略，也就不存在跨域的问题

### 浏览器性能问题 ###

#### 重绘和回流 ####
- 重绘和回流是渲染步骤中的一小节，但是对于性能影响很大
- 重绘是当节点需要更改外观而不会影响布局的，比如改变color就叫重绘
- 回流是布局或者几何属性需要改变成为回流
- 回流必定会发生重绘，而重绘不一定会引发回流

#### 一下操作可能会导致性能问题 ####
- 改变window大小
- 改变字体
- 添加或删除样式
- 文字改变
- 定位或者浮动
- 盒模型

#### 减少重绘和回流 ####
- 使用visibility替换display:none;
- 把DOM离线修改后，然后再把它显示出来
- 不要把DOM节点的属性值放在一个循环里当成循环里的变量
- 不要使用table布局，可能很小的改动会造成整个table重新布局
- css选择符从右往左匹配查找，避免DOM深度过深
- 将频繁运行的动画变为图层


#### 渲染机制 ####

步骤：
- 处理HTML并构建DOM树
- 处理CSS并构建CSSOM树
- 将DOM和CSSOM合并成一个渲染树
- 根据渲染树来布局，计算每个节点的位置
- 调用GPU绘制，合成图层，显示在屏幕上