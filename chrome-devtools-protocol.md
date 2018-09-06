在前面的文章简单的介绍了一下Chrome调试模式的启动方式，但前面的API只能做到简单的打开，关闭标签操作，当我们需要对某个标签页进行详细的操作时，则需要用到页面管理API。首先我们还是来回顾下获取页面信息：

访问 `http://127.0.0.1:9222/json`，即可获取如下所示的页面信息

```
    {
        "description": "",
        "devtoolsFrontendUrl": "/devtools/inspector.html?ws=127.0.0.1:9222/devtools/page/6d4f925f-7220-47cd-a4f9-800686445ffb",
        "faviconUrl": "http://tianfang.cnblogs.com/favicon.ico",
        "id": "6d4f925f-7220-47cd-a4f9-800686445ffb",
        "title": "天方 - 博客园",
        "type": "page",
        "url": "http://tianfang.cnblogs.com/",
        "webSocketDebuggerUrl": "ws://127.0.0.1:9222/devtools/page/6d4f925f-7220-47cd-a4f9-800686445ffb"
    }
```

其中webSocketDebuggerUrl字段就是页面管理API的地址，从url格式就可以看出它是一个websocket形式的协议，首先我们看看链接地址：
```
    ws://127.0.0.1:9222/devtools/page/92615aad-5862-48d5-983d-248468e9741a
```

通过简单的分析可以看出，它的变量只有两个：访问端口和页面Id，我们甚至不需要访问webSocketDebuggerUrl字段，直接根据Id也可以非常容易的构建这个地址的。大多数程序都有websocket的支持库的，这里为了测试，我直接找的一个在线websocket工具：`http://www.blue-zero.com/WebSocket/`，连接上后，就可以进行命令的收发了。

 

交互协议

Chrome远程调试模式是遵循Chrome DevTools Protocol的，这个协议在https://chromedevtools.github.io/devtools-protocol/中有详细的描述，它一般分为三个版本：

[The latest (tip-of-tree) protocol (tot)](https://chromedevtools.github.io/devtools-protocol/tot/)

[v8-inspector protocol (v8)](https://chromedevtools.github.io/devtools-protocol/v8/)

[stable protocol (1-2)](https://chromedevtools.github.io/devtools-protocol/1-2/)

其中稳定版的是stable protocol，不过它的功能相对最新版要少些，如果不担心稳定性的问题的话，我们大多数的时候还是可以直接用(tot)版本的。

 

消息格式

Chrome DevTools Protocol的协议消息格式比较简单，是文本形式的，用json表示对象，它主要分为如下三种：请求、响应和通知。

请求：

请求是客户端主动向chrome发送的请求，具体格式可参考前面的协议，这里我们以Network.enable为例，我们发送的请求如下：

    {"id":4,"method":"Network.enable","params":{"maxTotalBufferSize":10240}}

它内容主要包括id，method， params三个部分：id是自己建立的命令编号，method是发送的请求，params则是请求参数。

响应：

响应则是对请求的应答，并返回结果：

    {"id":4,"result":{}}

响应内容包括两个部分：id和result，id为请求命令编号，result为请求结果。

通知：

通知为chrome主动通知给客户端的消息。例如，启用network.enable后，就会上报一些网络通知：

    {"method":"Page.frameStoppedLoading","params":{"frameId":"7216.1"}} 
    {"method":"Network.loadingFailed","params":{"requestId":"7216.88","timestamp":2832.149269,"type":"Document","errorText":"","canceled":true}}

  通知主要包括method和params两个部分。　　

 

常用指令

Chrome DevTools Protocol的指令分为三十多个大类，每类又有若干个指令，这里不能一一介绍，只选择几个简单而常用的指令介绍一下：

跳转到指定页面：Page.navigate
执行JS函数：Runtime.evaluate
获取资源树：Page.getResourceTree
获取资源：Page.getResourceContent
其中Page.navigate是必备指令，用于跳转页面。而Runtime.evaluate的效果等同于在Develop Tools的Console控制台执行指令，基本可以执行任何js指令，模拟输入，输出渲染后的html用它都可以轻松搞定，可以说是大杀器了。

    

获取资源树和获取资源指令则用于获取浏览器当前原始请求的数据，可以用它来构建Develop Tools的Source树。

    

可以说，利用Develop Tools实现的功能我们都可以通过Chrome DevTools Protocol实现，Chrome自己也内置了一个官方的实现，用Chrome直接访问页面信息的devtoolsFrontendUrl即可看到，和按F12调用出来的Develop Tools基本一模一样。
