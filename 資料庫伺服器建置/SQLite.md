![image](https://github.com/MyDearGreatTeacher/2024A2/assets/37649784/5ac08b6f-c6c2-4923-af2a-4db762c76427)# SQLite
- [SQLite 教程](https://www.runoob.com/sqlite/sqlite-tutorial.html)
## SQLite基本操作

# 使用Python存取SQLite 
### 使用sqlite3模組 ==> 建立A888168.db資料庫
```python
#!/usr/bin/python
import sqlite3
conn = sqlite3.connect(‘A888168.db')
print ("資料庫打開成功")
```
### 建立COMPANY資料表
```python
#!/usr/bin/python

import sqlite3

## 先連線到資料庫
conn = sqlite3.connect('A888168.db')
print ("資料庫打開成功")

## 建立cursor()物件 ==> 再使用execute()方法執行sql指令
c = conn.cursor()
c.execute('''CREATE TABLE COMPANY
       (ID INT PRIMARY KEY     NOT NULL,
       NAME           TEXT    NOT NULL,
       AGE            INT     NOT NULL,
       ADDRESS        CHAR(50),
       SALARY         REAL);''')

print ("資料表創建成功")
conn.commit()
conn.close()
```

### 新增資料到資料表
```python
#!/usr/bin/python
import sqlite3

conn = sqlite3.connect('A888168.db')

## 建立cursor()物件 ==> 再使用execute()方法執行sql指令
c = conn.cursor()
print ("資料庫打開成功")

## 輸入資料庫的資料 ==> 使用INSERT INTO 語句
c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (1, 'Paul', 32, 'California', 20000.00 )")
c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (2, 'Allen', 25, 'Texas', 15000.00 )")
c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (3, 'Teddy', 23, 'Norway', 20000.00 )")
c.execute("INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY) \
      VALUES (4, 'Mark', 25, 'Rich-Mond ', 65000.00 )")

conn.commit()
print ("資料插入成功")

conn.close()

```

### 資料庫查詢
```python
import sqlite3

conn = sqlite3.connect('A888168.db')
c = conn.cursor()
print ("資料庫打開成功")

cursor = c.execute("SELECT id, name, address, salary  from COMPANY")
for row in cursor:
   print "ID = ", row[0]
   print "NAME = ", row[1]
   print "ADDRESS = ", row[2]
   print "SALARY = ", row[3], "\n"

print ("資料操作成功")
conn.close()


```

### 
```python


```

### 
```python


```

### 
```python


```

### 
```python


```

### 
```python


```

### 
```python


```

### 
```python


```

### 
```python


```

### 
```python


```

### 
```python


```

### 
```python


```

