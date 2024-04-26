Python Flask Web從入門到實戰配套資源

#  習題
#  ch1
## app.py
```Python
print("hello world!")
```


#  ch2
## app.py
```Python
# encoding utf-8
from flask import Flask,url_for
app = Flask(__name__)
@app.route('/')
def index():
    print(url_for('my_list'))
    return 'Hello World!'
@app.route('/list/')
def my_list():
    return 'list'
if __name__ == '__main__':
    app.run()
```


#  ch3
#  3.1
## app.py
```Python
from flask import Flask
app = Flask(__name__)
@app.route('/')
def hello_world():
    for i in range(1, 10):
        for j in range(1, i + 1):
            print(str(j) + str("*") + str(i) + "=" + str(i * j), end="\t")
        print()
    return 'Hello World!'
if __name__ == '__main__':
    app.run()
```
#  3.2
## app.py
```Python
from flask import Flask,render_template
app = Flask(__name__)
@app.route('/')
def hello_world():
    books = [
     {
     'name': '紅樓夢',
     'author': '曹雪芹',
    'price': 200
    },
     {
      'name': '水滸傳',
      'author': '施耐庵',
      'price': 100
    }
    ]
    return render_template('index.html',**locals())


if __name__ == '__main__':
    app.run()
```


#  ch4
#  4.1
## app.py
```Python
from flask import Flask,render_template,request
app = Flask(__name__)
@app.route('/')
def hello_world():
    return 'Hello World!'
@app.route('/login/',methods=['GET','POST'])
def login():
    if request.method=='GET':
        return render_template('index.html')
    else:
        print("這是POST請求！")
if __name__ == '__main__':
    app.run()
```
#  4.2
## app.py
```Python
from flask import Flask#導入Flask模組
import time
app = Flask(__name__)#Flask初始化
@app.route('/')#定義路由
def hello_world():#定義視圖函數
    return 'Hello World!'#返回值
def user_login(func):#定義視圖函數
    def inner():#定義inner()函數
        starttime=time.time()
        func()#執行func函數
        time.sleep(2)
        endtime=time.time()
        print("%s函數執行時間為%s" % (func.__name__, endtime - starttime))
    return inner#返回inner函數
@user_login
def news():#定義函數news
    print('這是新聞詳情頁!')#列印輸出
# show_news=user_login(news)#news作為參數傳遞給user_login()函數
# show_news()#執行show_news()函數
user_login(news)
news()
if __name__ == '__main__':
    app.run()

```


#  ch5
#  5.1
## app.py
```Python
from flask import Flask,render_template,request,send_from_directory
import time
import os
from os import path
from werkzeug.utils import secure_filename
import platform
from form import UploadForm
from werkzeug.datastructures import CombinedMultiDict
app = Flask(__name__)
if platform.system() == "Windows":
    slash = '\\'
else:
    platform.system()=="Linux"
    slash = '/'
UPLOAD_PATH = os.path.curdir + slash + 'images' + slash
@app.route('/',methods=['POST', 'GET'])
def hello_world():
    if request.method == 'GET':
       return render_template('upload.html')
    else:
        if not os.path.exists(UPLOAD_PATH):
            os.makedirs(UPLOAD_PATH)#沒有目錄則創建目錄
        form = UploadForm(CombinedMultiDict([request.form, request.files]))
        if form.validate():
             f = request.files['file']
             filename = secure_filename(f.filename)#取得檔案名字
             ext = filename.rsplit('.', 1)[1]  # 獲取文件尾碼
             unix_time = int(time.time())
             new_filename = str(unix_time) + '.' + ext  # 對檔進行重新命名
             file_url=UPLOAD_PATH+new_filename
             f.save(path.join(UPLOAD_PATH, new_filename))
             return "上傳檔成功！！"
        else:
            return "只支援jpg、gif格式的檔！"
#訪問上傳的檔
#流覽器訪問：http://127.0.0.1:5000/images/xxx.jpg/  就可以查看檔了
@app.route('/images/<filename>/',methods=['GET','POST'])
def get_image(filename):
    dirpath = os.path.join(app.root_path, 'images')#得到絕對路徑，比如J:\python project\例5-3-2\uploads
    #return send_from_directory(dirpath,filename,as_attachment=True)#為下載方式
    return send_from_directory(dirpath,filename)#為線上流覽方式
if __name__ == '__main__':
    app.run(debug=True)
```
## form.py
```Python
# -*- coding:utf-8 -*-
from wtforms import Form,FileField,StringField
from wtforms.validators import InputRequired
from flask_wtf.file import FileRequired,FileAllowed

class UploadForm(Form):
    file = FileField(validators=[FileRequired(),       #FileRequired必須上傳
                                   FileAllowed(['jpg','gif'])     #FileAllowed:必須為指定的格式的檔
                                   ])
```
#  5.2
## app.py
```Python
from flask import Flask,session,render_template,request
from datetime import timedelta
import os
from forms import Login
import config
app = Flask(__name__)
# 設置session
app.config['SECRET_KEY'] = os.urandom(24)
app.config['PERMANENT_SESSION_LIFETIME'] = timedelta(days=10) # 配置7天有效
@app.route('/',methods=('POST','GET'))
def set_session():
    form = Login()
    # 判斷是否是驗證提交
    if form.validate_on_submit():
        username=request.form.get('username')
        password = request.form.get('password')
        print(username)
        print(password)
        session['username']='zhangsan'
        session.permanent = True
        return '設置成功！'
    else:
        # 渲染
        return render_template('login.html',form=form)
    return 'Session設置成功!'
#獲取session
@app.route('/get_session')
def get_session():
    username=session.get('username')
    return username or 'Session為空！'
#清除sessin
@app.route('/del_session')
def del_session():
    session.pop('username')
    #session.clear #清空Session
    return  'Session被刪除！'

if __name__ == '__main__':
    app.run()
```
## config.py
``` Python
#coding:utf8
import os
SECRET_KEY = os.urandom(24)
CSRF_ENABLED = True
```
## forms.py
``` Python
# -*- coding:utf-8 -*-
#引入Form基類
from flask_wtf import Form
#引入Form元素父類
from wtforms import StringField,PasswordField
#引入Form驗證父類
from wtforms.validators import DataRequired,Length
#登錄表單類,繼承與Form類
class Login(Form):
    #用戶名
    name=StringField('name',validators=[DataRequired(message="用戶名不能為空")
        ,Length(6,16,message='長度位於6~16之間')],render_kw={'placeholder':'輸入用戶名'})
    #密碼
    password=PasswordField('password',validators=[DataRequired(message="密碼不能為空")
        ,Length(6,16,message='長度位於6~16之間')],render_kw={'placeholder':'輸入密碼'})
```


#  ch6
#  6.1
## config.py
```Python
USERNAME = 'root'
PASSWORD = 'root'
HOST = '127.0.0.1'
PORT = '3306'
DATABASE = 'new'
DB_URI = 'mysql+pymysql://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOST, PORT, DATABASE)
SQLALCHEMY_DATABASE_URI = DB_URI
# 動態追蹤修改設置，如未設置只會提示警告
SQLALCHEMY_TRACK_MODIFICATIONS=False
#查詢時會顯示原始SQL語句
SQLALCHEMY_ECHO = True
```
## db_demo1.py
``` Python
#encoding:utf8
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
import config#導入設定檔
from datetime import datetime
app = Flask(__name__)
app.config.from_object(config)#設定檔產生實體
#初始化一個物件
db=SQLAlchemy(app)
#測試資料庫連結是否成功
class News(db.Model):
    __tablename__ = 'news'
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(80), unique=True)
    content = db.Column(db.String(320), unique=True)
    author=db.Column(db.String(40), unique=True)
    time = db.Column(db.DateTime, default=datetime.now)
db.create_all()
@app.route('/')
def index():
   news1=News(title="測試資料",content="測試資料的內容",author="zhangsan")
   db.session.add(news1)
   db.session.commit()
   return "添加資料成功"
if __name__ == '__main__':
    app.run(debug=True)
```
## models.py
``` Python
# -*- coding:utf-8 -*-
from db_demo1 import db
```

#  migrations
## env.py
``` Python
from __future__ import with_statement
from alembic import context
from sqlalchemy import engine_from_config, pool
from logging.config import fileConfig
import logging

# this is the Alembic Config object, which provides
# access to the values within the .ini file in use.
config = context.config

# Interpret the config file for Python logging.
# This line sets up loggers basically.
fileConfig(config.config_file_name)
logger = logging.getLogger('alembic.env')

# add your model's MetaData object here
# for 'autogenerate' support
# from myapp import mymodel
# target_metadata = mymodel.Base.metadata
from flask import current_app
config.set_main_option('sqlalchemy.url',
                       current_app.config.get('SQLALCHEMY_DATABASE_URI'))
target_metadata = current_app.extensions['migrate'].db.metadata

# other values from the config, defined by the needs of env.py,
# can be acquired:
# my_important_option = config.get_main_option("my_important_option")
# ... etc.


def run_migrations_offline():
    """Run migrations in 'offline' mode.

    This configures the context with just a URL
    and not an Engine, though an Engine is acceptable
    here as well.  By skipping the Engine creation
    we don't even need a DBAPI to be available.

    Calls to context.execute() here emit the given string to the
    script output.

    """
    url = config.get_main_option("sqlalchemy.url")
    context.configure(url=url)

    with context.begin_transaction():
        context.run_migrations()


def run_migrations_online():
    """Run migrations in 'online' mode.

    In this scenario we need to create an Engine
    and associate a connection with the context.

    """

    # this callback is used to prevent an auto-migration from being generated
    # when there are no changes to the schema
    # reference: http://alembic.zzzcomputing.com/en/latest/cookbook.html
    def process_revision_directives(context, revision, directives):
        if getattr(config.cmd_opts, 'autogenerate', False):
            script = directives[0]
            if script.upgrade_ops.is_empty():
                directives[:] = []
                logger.info('No changes in schema detected.')

    engine = engine_from_config(config.get_section(config.config_ini_section),
                                prefix='sqlalchemy.',
                                poolclass=pool.NullPool)

    connection = engine.connect()
    context.configure(connection=connection,
                      target_metadata=target_metadata,
                      process_revision_directives=process_revision_directives,
                      **current_app.extensions['migrate'].configure_args)

    try:
        with context.begin_transaction():
            context.run_migrations()
    finally:
        connection.close()

if context.is_offline_mode():
    run_migrations_offline()
else:
    run_migrations_online()
```

#  versions
## c05b7a85f37c_.py
``` Python
"""empty message

Revision ID: c05b7a85f37c
Revises: 
Create Date: 2019-02-07 12:20:17.980784

"""
from alembic import op
import sqlalchemy as sa


# revision identifiers, used by Alembic.
revision = 'c05b7a85f37c'
down_revision = None
branch_labels = None
depends_on = None


def upgrade():
    # ### commands auto generated by Alembic - please adjust! ###
    op.add_column('news', sa.Column('abstract', sa.String(length=200), nullable=True))
    op.add_column('news', sa.Column('tags', sa.String(length=320), nullable=True))
    # ### end Alembic commands ###


def downgrade():
    # ### commands auto generated by Alembic - please adjust! ###
    op.drop_column('news', 'tags')
    op.drop_column('news', 'abstract')
    # ### end Alembic commands ###
```

#  6.2
## config.py
```Python
USERNAME = 'root'
PASSWORD = 'root'
HOST = '127.0.0.1'
PORT = '3306'
DATABASE = 'new'
DB_URI = 'mysql+pymysql://{}:{}@{}:{}/{}?charset=utf8'.format(USERNAME, PASSWORD, HOST, PORT, DATABASE)
SQLALCHEMY_DATABASE_URI = DB_URI
# 動態追蹤修改設置，如未設置只會提示警告
SQLALCHEMY_TRACK_MODIFICATIONS=False
#查詢時會顯示原始SQL語句
SQLALCHEMY_ECHO = True
```
## db demo1.py
``` Python
#encoding:utf8
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
import config#導入設定檔
from datetime import datetime
app = Flask(__name__)
app.config.from_object(config)#設定檔產生實體
#初始化一個物件
db=SQLAlchemy(app)
#測試資料庫連結是否成功
class News(db.Model):
    __tablename__ = 'news'
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(80), unique=True)
    content = db.Column(db.String(320), unique=True)
    author=db.Column(db.String(40), unique=True)
    tags = db.Column(db.String(200))
    abstract= db.Column(db.Text)
    time = db.Column(db.DateTime, default=datetime.now)
db.create_all()
@app.route('/')
def index():
   news1=News(title="測試資料",content="測試資料的內容",author="zhangsan")
   db.session.add(news1)
   db.session.commit()
   return "添加資料成功"
if __name__ == '__main__':
    app.run(debug=True)
```
## manager.py
``` Python
from flask_script import  Manager
from flask_migrate import Migrate,MigrateCommand
from db_demo1 import db,app
manager=Manager(app)
Migrate(app,db)
manager.add_command('db',MigrateCommand)

if __name__=='__main__':
    manager.run()
```
## models.py
``` Python
# -*- coding:utf-8 -*-
from db_demo1 import db
```

#  migrations
## env.py
``` Python
from __future__ import with_statement
from alembic import context
from sqlalchemy import engine_from_config, pool
from logging.config import fileConfig
import logging

# this is the Alembic Config object, which provides
# access to the values within the .ini file in use.
config = context.config

# Interpret the config file for Python logging.
# This line sets up loggers basically.
fileConfig(config.config_file_name)
logger = logging.getLogger('alembic.env')

# add your model's MetaData object here
# for 'autogenerate' support
# from myapp import mymodel
# target_metadata = mymodel.Base.metadata
from flask import current_app
config.set_main_option('sqlalchemy.url',
                       current_app.config.get('SQLALCHEMY_DATABASE_URI'))
target_metadata = current_app.extensions['migrate'].db.metadata

# other values from the config, defined by the needs of env.py,
# can be acquired:
# my_important_option = config.get_main_option("my_important_option")
# ... etc.


def run_migrations_offline():
    """Run migrations in 'offline' mode.

    This configures the context with just a URL
    and not an Engine, though an Engine is acceptable
    here as well.  By skipping the Engine creation
    we don't even need a DBAPI to be available.

    Calls to context.execute() here emit the given string to the
    script output.

    """
    url = config.get_main_option("sqlalchemy.url")
    context.configure(url=url)

    with context.begin_transaction():
        context.run_migrations()


def run_migrations_online():
    """Run migrations in 'online' mode.

    In this scenario we need to create an Engine
    and associate a connection with the context.

    """

    # this callback is used to prevent an auto-migration from being generated
    # when there are no changes to the schema
    # reference: http://alembic.zzzcomputing.com/en/latest/cookbook.html
    def process_revision_directives(context, revision, directives):
        if getattr(config.cmd_opts, 'autogenerate', False):
            script = directives[0]
            if script.upgrade_ops.is_empty():
                directives[:] = []
                logger.info('No changes in schema detected.')

    engine = engine_from_config(config.get_section(config.config_ini_section),
                                prefix='sqlalchemy.',
                                poolclass=pool.NullPool)

    connection = engine.connect()
    context.configure(connection=connection,
                      target_metadata=target_metadata,
                      process_revision_directives=process_revision_directives,
                      **current_app.extensions['migrate'].configure_args)

    try:
        with context.begin_transaction():
            context.run_migrations()
    finally:
        connection.close()

if context.is_offline_mode():
    run_migrations_offline()
else:
    run_migrations_online()
```
#  versions
## c05b7a85f37c_.py
``` Python
"""empty message

Revision ID: c05b7a85f37c
Revises: 
Create Date: 2019-02-07 12:20:17.980784

"""
from alembic import op
import sqlalchemy as sa


# revision identifiers, used by Alembic.
revision = 'c05b7a85f37c'
down_revision = None
branch_labels = None
depends_on = None


def upgrade():
    # ### commands auto generated by Alembic - please adjust! ###
    op.add_column('news', sa.Column('abstract', sa.String(length=200), nullable=True))
    op.add_column('news', sa.Column('tags', sa.String(length=320), nullable=True))
    # ### end Alembic commands ###


def downgrade():
    # ### commands auto generated by Alembic - please adjust! ###
    op.drop_column('news', 'tags')
    op.drop_column('news', 'abstract')
    # ### end Alembic commands ###
```


#  ch7
#  7.1
## app.py
```Python
from flask import Flask
   import memcache
   app = Flask(__name__)
   mc = memcache.Client(['127.0.0.1:11211'],debug=True)#能連接多個伺服器
   mc.add('name','zhangshan',time=120)#一次只能設置一個值
   mc.add('age',20,time=120)
   @app.route('/')
   def hello_world():
       return 'Hello World!'
   if __name__ == '__main__':
       app.run()
```
#  7.2
## app.py
```Python
from flask import Flask
   import memcache
   app = Flask(__name__)
   mc = memcache.Client(['127.0.0.1:11211'],debug=True)#能連接多個伺服器
   mc.add('name','zhangshan',time=120)#一次只能設置一個值
   mc.add('age',20,time=120)
   name= mc.get('name')#獲取資料
   age=mc.get('age')
   print(name)
   print(age)
   @app.route('/')
   def hello_world():
       return 'Hello World!'
   if __name__ == '__main__':
       app.run()
```


#  ch8
## app.py
```Python

```
#  8.1
## app.py
```Python
from flask import Flask,render_template

app = Flask(__name__)


@app.route('/')
def hello_world():
    return render_template('index.html')


if __name__ == '__main__':
    app.run()
```
#  8.2
## app.py
```Python
from flask import Flask,render_template

app = Flask(__name__)


@app.route('/')
def hello_world():
    return render_template('index.html')


if __name__ == '__main__':
    app.run()
```

#  ch9
#  9.1
## app.py
```Python
from flask import Flask

app = Flask(__name__)
def  my_login(func):#定義裝飾器函數
    def inner():#定義inner()函數
        func()#執行func函數
        print("hello world")
    return inner#返回inner函數
@app.route('/')
@my_login
def hello_world():
    print("run")
    return 'Hello World!'
hello_world()
if __name__ == '__main__':
    app.run()
```
#  9.2
## app.py
```Python
from flask import Flask

app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'Hello World!'
import random
from PIL import Image, ImageDraw, ImageFont, ImageFilter
from io import BytesIO
from flask import make_response,Flask
import codecs

class RandomChar():
  """用於隨機生成漢字對應的Unicode字元"""
  @staticmethod
  def Unicode():
    val = random.randint(0x4E00, 0x9FBB)
    return chr(val)

  @staticmethod
  def GB2312():
    head = random.randint(0xB0, 0xCF)
    body = random.randint(0xA, 0xF)
    tail = random.randint(0, 0xF)
    val = ( head << 8 ) | (body << 4) | tail
    str = "%x" % val
    #
    return codecs.decode(str, 'hex_codec').decode('gb2312','ignore')

#預設字體為宋體，大多數系統都存在這種字體
class ImageChar():
  def __init__(self, fontColor = (0, 0, 0),
                     size = (100, 40),
                     fontPath = 'STZHONGS.TTF',#確保工程下有該檔
                     bgColor = (255, 255, 255, 255),
                     fontSize = 20):
    self.size = size
    self.fontPath = fontPath
    self.bgColor = bgColor
    self.fontSize = fontSize
    self.fontColor = fontColor
    self.font = ImageFont.truetype(self.fontPath, self.fontSize)
    self.image = Image.new('RGBA', size, bgColor)

  def rotate(self):
    img1 = self.image.rotate(random.randint(-5, 5), expand=0)#默認為0，表示剪裁掉伸到畫板外面的部分
    img = Image.new('RGBA',img1.size,(255,)*4)
    self.image = Image.composite(img1,img,img1)

  def drawText(self, pos, txt, fill):
    draw = ImageDraw.Draw(self.image)
    draw.text(pos, txt, font=self.font, fill=fill)
    del draw

  def randRGB(self):
    return (random.randint(0, 255),
           random.randint(0, 255),
           random.randint(0, 255))

  def randPoint(self):
    (width, height) = self.size
    return (random.randint(0, width), random.randint(0, height))

  def randLine(self, num):
    draw = ImageDraw.Draw(self.image)
    for i in range(0, num):
      draw.line([self.randPoint(), self.randPoint()], self.randRGB())
    del draw

  def randChinese(self, num):
    gap = 0
    start = 0
    strRes = ''
    for i in range(0, num):
      char = RandomChar().GB2312()
      strRes += char
      x = start + self.fontSize * i + random.randint(0, gap) + gap * i
      self.drawText((x, random.randint(-5, 5)), char, (0,0,0))
      self.rotate()
    print(strRes)
    self.randLine(8)
    return strRes,self.image

@app.route('/code/')
#################################
#返回驗證碼資料流程和字元
def get_code():
    ic = ImageChar(fontColor=(100,211, 90))
    str_text,code_img = ic.randChinese(4)
    buf = BytesIO()
    if code_img.mode != "RGB":
        code_img = code_img.convert("RGB")

    code_img.save(buf,'JPEG',quality=70)
    buf_str = buf.getvalue()

    # 把二進位作為response發回前端，並設置首部欄位
    response = make_response(buf_str)
    response.headers['Content-Type'] = 'image/gif'
    return response
if __name__ == "__main__":
    app.run(host="localhost",port=5000,debug=True)

```

#  ch10
#  10.1
## app.py
```Python
from flask import Flask,render_template

app = Flask(__name__)


@app.route('/')
def hello_world():
    return render_template('index.html')


if __name__ == '__main__':
    app.run()
```
#  10.2
## app.py
```Python
from flask import Flask,render_template

app = Flask(__name__)


@app.route('/')
def hello_world():
    return render_template('index.html')


if __name__ == '__main__':
    app.run()
```

#  ch13
#  13.1
## app.py
```Python
from flask import Flask,request

app = Flask(__name__)


@app.route('/')
def hello_world():
    return 'Hello World!'
@app.route('/article_detail/<int:id>/')
def article_detail(id):
    print('url: %s , path: %s ' % (request.url,request.path))
    return 'sucess'


if __name__ == '__main__':
    app.run()
```

#  ch14
#  14.1
## app.py
```Python
from flask import Flask,render_template

#定義app物件
app=Flask(__name__)
# 設置秘鑰
app.config['SECRET_KEY'] = '123456'
#開啟保護
from flask_wtf import CSRFProtect
CSRFProtect(app)
@app.route('/')
def index():
    return render_template('index.html')
if __name__=="__main__":
    app.run()
```
