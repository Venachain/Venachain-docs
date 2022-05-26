# JAVA-SDK 连接 Venachain 节点

首先需要与Venachain节点建立连接，以获取链上有关服务。Venachain支持建立http连接和websocket连接两种方式。

```java
//http短连接
Web3j web3j = Web3j.build(new HttpService("http://127.0.0.1:6791"));
```

```java
//ws长连接
WebSocketClient webSocketClient = new WebSocketClient(newURI("ws://127.0.0.1:6791"));
WebSocketService ws = new WebSocketService(webSocketClient, true);
ws.connect();
Web3j web3j = Web3j.build(ws);
```

说明：

1. 建立Websocket连接需要显式调用connect方法（与HTTP不同）。
2. Venachain节点需要在启动时打开websocket监听功能，即启动时加入参数：`--ws`。