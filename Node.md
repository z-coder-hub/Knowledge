#### 字符串方法

charCodeAt() 获取字符串的Unicode码



#### nodejs搭建websocket

`npm i ws`

```javascript
const { WebSocketServer, WebSocket } = require('ws')

const wss = new WebSocketServer({ port: 端口, host: 域名 });


wss.on("connection", function connection(ws) {
    // 监听连接事件，ws为websocket的服务端
    
    WsList.push(ws)
    
    
    // 监听客户端发过来的消息
    ws.on("message", function message(msg) {
        let data = msg.toString()
        res.send(data)
    })
})
```



#### 配置广播

- 简单配置广播

  ```javascript
  const { WebSocketServer, WebSocket } = require('ws')
  
  const WsList = [];  // 全局作用域
  
  
  // 在wss.connection中监听message消息事件
  ws.on("message", function message(msg) {
      let data = msg.toString()
      
      // 配置广播
      WsList.forEach((item, index) => {
          // 可以理解为多个连接websocket的客户端
          // 将数据推送到这些客户端中
          // 而不是单独推送到自己
          item.send(data)
      })
  })
  ```

  

- 使用ws方法配置

  ```javascript
  const { WebSocketServer, WebSocket } = require('ws')
  
  
  
  // 在wss.connection中监听message消息事件
  ws.on("message", function message(msg) {
      let data = msg.toString()
      
      // 全广播（包括自己）
      wss.clients.forEach(function each(client) {
          if (client.readyState === WebSocket.OPEN) {
              client.send(data, { binary: isBinary });
          }
      });
      
      // 自己除外广播
      wss.clients.forEach(function each(client) {
        if (client !== ws && client.readyState === WebSocket.OPEN) {
          client.send(data, { binary: isBinary });
        }
      });
      
  })
  ```
  
  