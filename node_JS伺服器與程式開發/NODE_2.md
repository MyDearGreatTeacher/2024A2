# 單元2:簡單的node.js程式開發
- [Node.js Tutorial](https://www.w3schools.com/nodejs/default.asp)
- [Node.js 教程](https://www.runoob.com/nodejs/nodejs-tutorial.html)
## 第一支程式 A888168_1.js
```javascript
var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.end('Hello World! This is A888168 龍大大');
}).listen(8111);

console.log('Server running at http://127.0.0.1:8111/');
```
- 執行程式 node A888168_1.js
- 打開瀏覽器 ==>127.0.0.1:8111
## 第2支程式 
- 建立你的函數庫 ==> A888.js
- 建立你的程式 A888168_2.js

```javascript
exports.myDateTime = function () {
  return Date();
};
```


```javascript
var http = require('http');
var dt = require('./myfirstmodule');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.write("The date and time are currently: " + dt.myDateTime());
  res.end();
}).listen(8080);
```
