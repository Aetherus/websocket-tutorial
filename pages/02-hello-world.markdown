---
layout: page
title: Hello World
permalink: /hello-world/
---

废话少说，直接建项目。

## 服务器端

先来创建一个服务器端项目。
新建一个目录`ws-demo-server`，打开你的最佳拍档——命令行，cd到你新建的目录，开始安装依赖。

```
$ npm init  # 一路回车
$ npm install --save ws
```

创建文件index.js，并输入以下内容：

```
const WebSocket = require('ws');

const wsServer = new WebSocket.Server({port: 8080});

wsServer.on('connection', function(ws) {
  setInterval(function() {
    ws.send('Hello, World!');
  }, 2000);
});
```

这里我们首先创建了一个WebSocket服务器，并让它监听8080端口。
当有客户端尝试连接服务器时，会触发服务器端的`'connection'`事件，
我们只要给这个事件一个回调函数就能处理这个事件了，不用关心握手的细节。
`'connect'`事件的回调函数只有一个参数——一个WebSocket对象。
所有发送给这个ws对象的消息都会被当前客户端接收，但不会被其他客户端接收！
我这里每两秒钟给这个客户端发送一条`'Hello, World'`消息。

## 浏览器端

我偷个懒，不建项目了，直接写个网页拉倒。

```
<!doctype html>
<html>
  <head>
    <meta charset="utf-8"/>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
    <script>
      $(function() {
        var ws = new WebSocket('ws://localhost:8080/');
        ws.addEventListener('message', function(event) {
          $('<p/>').text(event.data).appendTo('#container');
        })
      });
    </script>
  </head>
  <body>
    <div id="container"></div>
  </body>
</html>
```

非常简单直白，我相信你自己看代码就能懂了。和服务器端一样，你不用操心握手的细节，只要指定URL即可。

