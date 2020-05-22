# websocket协议笔记

1. ##### websocket介绍

   ```js
   websocket是一种网络通信协议。因为HTTP协议有一个缺陷：通信只能由客户端发起，不能由服务端主动推送消息。从而websocket诞生了。之前服务器只能通过“轮询”的方式向客户端发送消息：即定每隔一段时间向服务器发送消息，然后服务器响应。
   websocket的最大特点就是，服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等的全双工对话，属于服务器推送技术的一种。
   ```

2. ##### 基于nodejs的websocket

   ```js
   <!--服务器端-->
   //nodejs中搭建websocket服务
   1、npm 安装ws  选择安装路径后，直接npm install ws（不行就用cnpm）
   2、引入var WebSocketServer = require('ws').Server;
   3、添加监听端口 var wss = new WebSocketServer({ port: 8080 });
   4、建立连接：
   wss.on('connection', function (ws) {
   	//console.log('client connected'); 
   	//接收客户端发来的连接请求，连接成功以后，执行回调函数，才能接收消息。
   	ws.on('message', function (message) { //接收消息
   		console.log(message);
           ws.send(message);  //向客户端发送消息
   	});
   });
   
   <!--客户端-->
   var ws = new WebSocket("ws://localhost:8080");
   var sendMsg = $("#inputMessage").val();
   var ws = new WebSocket("ws://localhost:8080");
   ws.onopen = function (e) {
   	if(sendMsg && sendMsg != ""){
       	ws.send(sendMsg); //注意：回调函数，必须要等到建立起链接后才能发送数据
       }else{
       	alert("提交有效数据")
       }
   ws.onmessage = function (evt) {
       var receiveMessage = evt.data;
       $("#showMessage").append($("<p> " + receiveMessage + " </p>"));
       	ws.close(); //每次发送接收完数据后就关闭
       };
   }
   ws.onclose = function() {  //监听关闭链接的回调函数
   	console.log("连接已关闭...");
   };
   
   监听事件类型：
   ws.onopen	连接建立时触发
   ws.onmessage	客户端接收服务端数据时触发
   ws.onerror	通信发生错误时触发
   ws.onclose	连接关闭时触发
   eg: 
   ws.onclose = function() {  //监听关闭链接的回调函数
   	console.log("连接已关闭...");
   };
   
   方法种类：
   ws.send()   使用连接发送数据,注意，需要将对象转换为字符串发送到服务端
   ws.close()  关闭连接
   ```

3. 虚位以待！