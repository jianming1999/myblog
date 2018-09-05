MAC下运行命令开启调试服务， 这一步就是会建立一个用于调试网页的websocket服务

`open -a "Google\ Chrome" --args --remote-debugging-port=9222`

接下来可以访问到以下地址：

http://localhost:9222/   // 调试主界面，点击某个进入调试界面

http://localhost:9222/json/list   // 列出调试列表的json

http://localhost:9222/json/protocol  // 调出调试协议

http://localhost:9222/json/new?${url}  // 打开一个新的url

http://localhost:9222/json/activate/${id}  // 根据id定位到某一个窗口




客户端发送调试命令的示例

```javascript
var ws = new WebSocket("ws:localhost:9222/devtools/page/1D96784DE3E2E0A6B5BE76D10A2D1EFE");

ws.onopen = function(evt) { 
  console.log("Connection open ..."); 
  ws.send('{"id":2,"method":"Runtime.evaluate","params":{"expression":"chrome.loadTimes()","returnByValue":true}}');
};

ws.onmessage = function(evt) {
  console.log( "Received Message: " + evt.data);
  ws.close();
};

ws.onclose = function(evt) {
  console.log("Connection closed.");
};      
```

Android下端口转发命令：
`adb forward tcp:9222 localabstract:chrome_devtools_remote`
