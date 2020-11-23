* 服务器推送技术的一种。
* 建立在 TCP 协议之上。
* 数据格式比较轻量，性能开销小，通信高效。
* 可以发送文本，也可以发送二进制数据。
* 没有同源限制，客户端可以与任意服务器通信。
* 协议标识符是ws（如果加密，则为wss），服务器网址就是 URL.

问题。发送http请求时，需要建立连接吗？TCP和UDP什么关系？http报文是UDP实现吗？

```js
let ws = new WebSocket("ws://localhost:8080");
switch(ws.readyState){
    case WebSocket.CONNECTING:break;
    case WebSocket.OPEN:break;
    case WebSocket.CLOSING:break;
    case WebSocket.CLOSED:break;
    default: break;
}

// 1. 连接成功后的回调函数。写法1
ws.onOpen = function(){
    // 向服务器发送数据
    ws.send("Hello Server");
}
// websocket连接成功后的回调函数，写法2
ws.addEventListener('open',function(){
    ws.send("Hello Server");
})

// 2. 连接关闭后的回调函数
ws.onclose = function(event){
    var code = event.code,
        reason = event.reason,
        wasClean = event.wasClean;
}

// 3. 收到服务器数据后的回调函数
ws.onmessage = function(event){
    var data = event.data;
}

// 4. 报错时的回调函数
ws.onerror = function(event){
    // handle error event;
}
```

