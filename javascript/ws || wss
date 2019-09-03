### ws和wss区别
websocket使用ws或wss的统一资源标识符，类似于HTTP或HTTPS，其中wss表示在TLS之上的websocket,相当于websocket;
默认情况下，websocket的ws协议使用的是80端口；运行在TLS之上时，wss协议默认使用433端口。其实wss就是ws基于SSL的安全传输，与HTTPS一样的道理。

####nginx配置域名支持
```javascript
location /websocket{
  proxy_pass http://backend;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "upgrade";
}
```
