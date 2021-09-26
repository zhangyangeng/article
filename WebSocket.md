# 基础内容

用途：解决轮询问题

## 服务端方法

实例化ws：
```ts
import { Server } from "ws";
const ws = new Server();
```

连接ws：
```ts
ws.on("connection", websocket => {})
```

向客户端发送消息：
```ts
websocket.send("这个消息是服务器主动推送的");
```

接收客户端消息：
```ts
websocket.on("message", (msg)=> {
  console.log("接收到消息：" + msg.toString() + Date.now())
  websocket.send("我接到到了你的消息：" + msg + Date.now())
})
```

## 参考文章

[阮一峰教程](https://www.ruanyifeng.com/blog/2017/05/websocket.html)

[望星辰大海](https://www.cnblogs.com/tohxyblog/p/7112917.html)

[廖雪峰教程](https://www.liaoxuefeng.com/wiki/1022910821149312/1103303693824096)

[Angular+WebSocket实践](https://www.cnblogs.com/zhenguo-chen/p/10441026.html)

[3小时教你如何使用websocket实现一个聊天室](https://ke.qq.com/course/355307?taid=2788043660815339)

[Angular 4 + webSocket 双方通信小尝试](https://blog.csdn.net/zt15732625878/article/details/80560088)

# STOMP



## 参考文章

[__qqqqq](https://blog.csdn.net/jqsad/article/details/77745379)

[古兰精](https://www.cnblogs.com/goloving/p/14735257.html)

[记一次 Angular 基于 STOMP over WebSocket 实现流文本传输](https://segmentfault.com/a/1190000022799586)



