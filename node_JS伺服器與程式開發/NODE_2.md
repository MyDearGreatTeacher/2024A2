# 單元2:簡單的node.js程式開發
- [Node.js Tutorial]()

## 第一支程式 A888168_1.js
```javascript
var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.end('Hello World! This is A888168 龍大大');
}).listen(8080);
```
- 執行程式 node A888168_1.js
- 打開瀏覽器 ==>127.0.0.1:8080
## 第2支程式 A888168_2.js
