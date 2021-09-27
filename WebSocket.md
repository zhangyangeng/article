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

## 心跳机制

参考自该 [文章](https://www.cnblogs.com/tugenhua0707/p/8648044.html)



## 参考文章

[阮一峰教程](https://www.ruanyifeng.com/blog/2017/05/websocket.html)

[望星辰大海](https://www.cnblogs.com/tohxyblog/p/7112917.html)

[廖雪峰教程](https://www.liaoxuefeng.com/wiki/1022910821149312/1103303693824096)

[Angular+WebSocket实践](https://www.cnblogs.com/zhenguo-chen/p/10441026.html)

[3小时教你如何使用websocket实现一个聊天室](https://ke.qq.com/course/355307?taid=2788043660815339)

[Angular 4 + webSocket 双方通信小尝试](https://blog.csdn.net/zt15732625878/article/details/80560088)

[angular 整合websocket](https://www.jianshu.com/p/b04c34df128d)

# STOMP

## 创建STOMP

原生WebSocket使用如下：
```ts
const client = Stomp.client(url);
```

## 连接服务端

使用 `connect()` 方法来进行连接

## 心跳检测

使用 `heartbeat` 来进行心跳检测
```ts
client.heartbeat.outgoing = 20000;	// 接收频率
client.heartbeat.incoming = 0;		// 发送频率
```

## 发送消息

使用 `send()` 方法来发送消息

## 订阅、接收消息




## 参考文章

[__qqqqq](https://blog.csdn.net/jqsad/article/details/77745379)

[古兰精](https://www.cnblogs.com/goloving/p/14735257.html)

[记一次 Angular 基于 STOMP over WebSocket 实现流文本传输](https://segmentfault.com/a/1190000022799586)

[StompJS使用文档总结：如何创建stomp客户端、如何连接服务器、心跳机制、如何发送消息、如何订阅和取消订阅、事务、如何调试](https://www.cnblogs.com/goloving/p/10746378.html)

[WebSocket和Stomp协议](https://www.jianshu.com/p/db21502518b9)

[一个stompjs的简单应用](https://github.com/chuxiaoguo/wrapper-stompjs)

[stompjs的API](https://stomp-js.github.io/api-docs/latest/index.html)

# 案例

## 聊天室

服务端想法：
- 正常创建 WebSocket 服务，并接收客户端的消息
- 每次创建的连接即为一个用户，当连接创建时需要将每个用户存储在数组中，当连接关闭时需要将该用户从数组中删除
- 服务端收到消息后需要将该条消息发送给每一个用户，因此需要遍历整个连接数组，给每个 item 发送消息

客户端想法：
- 用户进入聊天室时系统应该进行广播
- 用户点击按钮及时发送消息
- 用户进入聊天室时系统进行广播