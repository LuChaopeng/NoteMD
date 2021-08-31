1. WebSocket服务端监听（ws库）
   在使用ws库的事件监听函数时，ws.on('message', function (msg) {｝)的参数msg让我很迷，直接log是<buffer>，却可以以模板字面量输出。查了文档得知这个msg是个buffer对象

   `Event: 'message' `

   `data {Buffer|ArrayBuffer|Buffer[]} isBinary {Boolean}`

   并得知buffer是node.js的内容，可以用toString()转换为字符串编码

2. 客户端监听

   在浏览器监听websocket时，`ws.onmessage`或`ws.addEventListener("message")`的回调函数参数`msg`**是一个事件**，要获得服务器发来的内容，是在`msg.data`里找。

3. WebSocket服务示例（前端HTML5浏览器环境；后端使用node.js的ws库）

   服务器 `server.js`

   ```js
   /*引用ws库*/
   const WebSocket = require('ws');
   const WebSocketServer = WebSocket.Server;
   /*在服务器的3000端口开启WebSocket服务*/
   const wss = new WebSocketServer({
       port: 3000
   });
   /*监听客户端的连接*/
   /*回调函数的参数"ws"指的连接服务器的客户端itself*/
   wss.on('connection', (ws)=> {
       console.log("客户端已连接");
       /*监听客户端send的消息*/
       ws.on('message', function (msg) {
           /*将参数msg<buffer>转为string*/
           console.log(msg.toString());
           /*将客户端消息返还客户端*/
           ws.send(msg.toString());
       })
   })
   ```

   客户端 `index.html`

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   
   <script>
       /*创建ws服务端，连接指定服务器*/
       const ws = new WebSocket('ws://localhost:3000/');
       /*监听open（连接至服务器）事件*/
       ws.addEventListener("open",()=>{
           console.log("已连上服务器");
           ws.send("发送给服务器再由服务器返回");
       })
       /*监听服务器返回的消息，打印参数msg<Event>的data*/
       ws.addEventListener("message",(msg)=>{
           console.log(msg.data);
       })
   </script>
   </body>
   </html>
   ```

   

