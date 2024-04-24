## flask網站應用程式開發
- 已安裝Anaconda
- 步驟一 : 安裝 Flask 套件安裝flask : pip install flask
![flask](./flask_1.PNG)
- 步驟二 : 撰寫程式 app.py (使用notepad++)
```
from flask import Flask
app = Flask(__name__)
@app.route("/")
def hello():
    return "Hello, World!"
```
- 步驟三 : 終端機指令執行  $ flask run
![flask](./flask_2.PNG)
- 步驟四 : 打開瀏覽器
![flask](./flask_3.PNG)
- 資料來源: https://devs.tw/post/448
