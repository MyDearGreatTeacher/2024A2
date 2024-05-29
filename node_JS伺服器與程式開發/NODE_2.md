# 單元2:簡單的node.js程式開發
- [Node.js Tutorial]()

## 第一支程式
```javascript
var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.end('Hello World!');
}).listen(8080);
```
