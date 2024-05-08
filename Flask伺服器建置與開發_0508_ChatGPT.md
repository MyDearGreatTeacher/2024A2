# Flask伺服器建置與開發_0508_ChatGPT
  - 打造你的GPT:A888168GPT
  - [Building AI Applications with ChatGPT APIs](https://www.packtpub.com/product/building-ai-applications-with-chatgpt-apis/9781805127567)
## 參考資料
- [LibreChat](LibreChat)
- [chatgpt-clone](https://github.com/xtekky/chatgpt-clone/tree/main)

## 程式架構
- config.py
- app.py
- templates(目錄l只有一個首頁)
  - index.html

#### config.py ==> 填上openai的 KEY
#### 主要程式 ==>  app.py
```python
"""
file name: app.py
Description: Building a ChatGPT Clone with Flask framework
__copyright__ = "Copyright 2024, MartinYTech"
__author__=  Martin Yanev
__modified__= 03/03/2024
"""

from flask import Flask, request, render_template
import openai
import config

openai.api_key = config.API_KEY

app = Flask(__name__)

conversation_history = []


@app.route("/")
def index():
    return render_template("index.html")


@app.route("/get")
def get_bot_response():
    userText = request.args.get('msg')
    model_engine = "gpt-3.5-turbo"

    # Append user message to conversation history
    conversation_history.append({"role": "user", "content": userText})

    response = openai.ChatCompletion.create(
        model=model_engine,
        messages=conversation_history  # Pass the entire conversation history to OpenAI
    )

    # Append AI response to conversation history
    ai_response = response["choices"][0]["message"]["content"]
    conversation_history.append({"role": "assistant", "content": ai_response})

    return ai_response


if __name__ == "__main__":
    app.run()
```
#### index.html
```html
<!DOCTYPE html>
<html>
<head>
    <title>OpenAI GPT Chat</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <style>
        body {
            background-color: #35372D;
            color: #ededf2;
        }
        .container {
            margin-top: 20px;
        }
        #chat {
            height: 400px;
            overflow-y: scroll;
            background-color: #444654;
        }
        .list-group-item {
            border-radius: 5px;
            background-color: #444654;
        }
        . submit {
            background-color:#21232e;
            color: white;
            border-radius: 5px;
        }
        .input-group input {
            background-color: #444654;
            color: #ededf2;
            border: none;
        }
}

    </style>
</head>
<body>
    <div class="container">
        <h2>OpenAI GPT Chat</h2>
        <hr>
        <div class="panel panel-default">
            <div class="panel-heading">Chat Messages</div>
            <div class="panel-body" id="chat">
                <ul class="list-group">
                </ul>
            </div>
        </div>
        <div class="input-group">
            <input type="text" id="userInput" class="form-control">
            <span class="input-group-btn">
                <button class="btn btn-default" id="submit">Submit</button>
            </span>
        </div>
    </div>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
    <script>
        $("#submit").click(function(){
            var userInput = $("#userInput").val();
            $.get("/get?msg=" + userInput, function(data){
                $("#chat").append("<li class='list-group-item'><b>You:</b> " + userInput + "</li>");
                $("#chat").append("<li class='list-group-item'><b>OpenAI:</b> " + data + "</li>");
            });
        });
    </script>
</body>
</html>
```
