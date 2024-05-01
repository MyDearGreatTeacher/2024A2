# Flask伺服器建置與開發
- 本地端Flask
- 雲端平台
  - [render](https://render.com/)
  - [replit](https://replit.com/)

## 本地端Flask
- Windows
- 安裝Anaconda
- 步驟一 : 安裝 Flask 套件== >安裝flask : pip install flask
- 步驟二 : 撰寫程式 app.py (使用notepad++)
```
from flask import Flask
app = Flask(__name__)
@app.route("/")
def hello():
    return "Hello, World!"
```
- 步驟三 : 終端機指令執行 $ flask run
- 步驟四 : 打開瀏覽器 
- 資料來源: https://devs.tw/post/448
