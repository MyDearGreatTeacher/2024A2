# 單元2:簡單的node.js程式開發
- [Node.js Tutorial](https://www.w3schools.com/nodejs/default.asp)
- [Node.js 教程](https://www.runoob.com/nodejs/nodejs-tutorial.html)
- [Node.JS - 30 天入門學習筆記 系列](https://ithelp.ithome.com.tw/users/20103526/ironman/1081)
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

```JAVASCRIPT
var https = require('https');

https.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.write('Hello World!');
  res.end();
}).listen(8080);
```
## 第二支程式: 顯示中文  A888168_2.js
```javascript
var http = require('http');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.write('<head><meta charset="utf-8"/></head>');  
  res.write('<h1>Node.js</h1>');  
  res.write('<b>親愛的，你慢慢飛，小心前面帶刺的玫瑰...</b>');  
  res.end('<br> Hello World! This is A888168 龍大大');
}).listen(8111);

console.log('Server running at http://127.0.0.1:8111/');
```

## 第3支程式 ==> 使用內建模組(Modules)
- Node.js 有三大類的模組
  - 1.Core Modules (原生模組)
    - http：它包含可以用來建立http server 的一些類別, 方法, 及事件。
    - url：它包含可以解析url的一些方法。
    - querystring：它包含可以處理由client端傳來querystring的一些方法。
    - path：它包含可以處理一些檔案或資料夾路徑的方法。
    - fs：它包含檔案的存取／操作的一些類別，方法及事件。
    - util：它包含一些可供程序者使用的效能函式。
    - 載入Core Modules ==> `var module = require(‘module_name’);`
  - 2.Local Modules (自建模組)
    - 在 Node.js 中，每個 JavaScript 檔案都被視為一個模組。
    - 當你的專案程式碼持續擴充時，你可以將程式碼拆分成多個模組，以便更好地管理、協作和除錯。
    - 自建模組應該根據功能寫成獨立的 JavaScript 檔案。例如，你可以建立一個名為 Log.js 的登入模組。 
  - 3.Third Party Modules (第三方模組)
    - 這些模組由其他開發者或社群提供，並不是 Node.js 的核心模組。
    - 可以使用 npm（Node.js 的套件管理工具）來安裝和管理第三方模組。
    - 範例:如果你想使用 Express 框架，你可以執行` npm install express` 來安裝它。

- 建立你的函數庫 ==> A888.js


```javascript
exports.myDateTime = function () {
  return Date();
};
```

- 建立你的程式 A888168_2.js  ==> 這是錯誤的程式 ==> 使用兩次res.end
```javascript
var http = require('http');
var dt = require('./A888');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.write('<head><meta charset="utf-8"/></head>');  
  res.write('<h1>龍大大Node.js程式開發</h1>');  
  res.end('<br> Hello World! This is A888168 龍大大');
  res.write("我與程式奮鬥的時間: " + dt.myDateTime());
  res.end();
}).listen(8111);

console.log('Server running at http://127.0.0.1:8111/');
```
- 執行後的錯誤訊息
```javascript
PS C:\Users\KSU\A888168_0529> node .\A888168_3.js
Server running at http://127.0.0.1:8111/
node:events:497
      throw er; // Unhandled 'error' event
      ^

Error [ERR_STREAM_WRITE_AFTER_END]: write after end
    at write_ (node:_http_outgoing:911:11)
    at ServerResponse.write (node:_http_outgoing:864:15)
    at Server.<anonymous> (C:\Users\KSU\A888168_0529\A888168_3.js:9:7)
    at Server.emit (node:events:519:28)
    at parserOnIncoming (node:_http_server:1140:12)
    at HTTPParser.parserOnHeadersComplete (node:_http_common:119:17)
Emitted 'error' event on ServerResponse instance at:
    at emitErrorNt (node:_http_outgoing:883:9)
    at process.processTicksAndRejections (node:internal/process/task_queues:83:21) {
  code: 'ERR_STREAM_WRITE_AFTER_END'
}

Node.js v20.14.0
```
- 把程式丟給copilot分析
```
你的 Node.js 程式碼看起來是一個簡單的 HTTP 伺服器，但是有一個小問題需要修正。
讓我來幫你解決這個問題。

在你的程式碼中，你使用了兩次 res.end()。
這是不正確的，因為一旦響應結束，就不能再對響應物件進行操作。
你應該只使用一次 res.end()。

以下是修正後的程式碼：

var http = require('http');
var dt = require('./A888');

http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  res.write('<head><meta charset="utf-8"/></head>');  
  res.write('<h1>龍大大Node.js程式開發</h1>');  
  res.write('<br> Hello World! This is A888168 龍大大');
  res.write("我與程式奮鬥的時間: " + dt.myDateTime());
  res.end();
}).listen(8111);

console.log('Server running at http://127.0.0.1:8111/');
AI 產生的代碼。請仔細檢視及使用。 深入了解常見問題集。
現在，你的伺服器應該可以正確運行了。如果你還有其他問題，歡迎隨時詢問！
```
## 第4支程式  ==> Third Party Modules (第三方模組)
- 安裝第三方模組 ==>npm install express
```JAVSCRIPT
//A888168_6.js 文件
var express = require('express');
var app = express();
 
app.get('/', function (req, res) {	
   res.send('Hello World');
})
 
var server = app.listen(8081, function () {
 
  var host = server.address().address
  var port = server.address().port
 
  console.log("應用實例，訪問位址為 http://%s:%s", host, port)

})
```
