# Mysql資料庫伺服器建置與測試
- [MySQL新手入門超級手冊-第三版(適用MySQL 8.x與MariaDB 10.x)](https://www.tenlong.com.tw/products/9786263241787?list_name=srh)
  - [程式碼](https://www.gotop.com.tw/books/download.aspx?bookid=AED004300)
- [MySQL 教程](https://www.runoob.com/mysql/mysql-tutorial.html)
- [online Mysql](https://onecompiler.com/mysql)
- Navicat 資料庫管理系統 https://navicat.com/en/products
# 範例資料庫(1):
- https://www.runoob.com/sql/sql-tutorial.html
```sql
/*
 Navicat MySQL Data Transfer

 Source Server         : 127.0.0.1
 Source Server Version : 50621
 Source Host           : localhost
 Source Database       : RUNOOB

 Target Server Version : 50621
 File Encoding         : utf-8

 Date: 05/18/2016 11:44:07 AM
*/

SET NAMES utf8;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
--  Table structure for `websites`
-- ----------------------------
DROP TABLE IF EXISTS `websites`;
CREATE TABLE `websites` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` char(20) NOT NULL DEFAULT '' COMMENT '網站名稱',
  `url` varchar(255) NOT NULL DEFAULT '',
  `alexa` int(11) NOT NULL DEFAULT '0' COMMENT 'Alexa 排名',
  `country` char(10) NOT NULL DEFAULT '' COMMENT '國家',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

-- ----------------------------
--  Records of `websites`
-- ----------------------------
BEGIN;
INSERT INTO `websites` VALUES ('1', 'Google', 'https://www.google.cm/', '1', 'USA'), 
('2', '淘寶', 'https://www.taobao.com/', '13', 'CN'), 
('3', '菜鳥教程', 'http://www.runoob.com/', '4689', 'CN'), 
('4', '微博', 'http://weibo.com/', '20', 'CN'), 
('5', 'Facebook', 'https://www.facebook.com/', '3', 'USA');
COMMIT;

SET FOREIGN_KEY_CHECKS = 1;

```
# 範例資料庫(2):access_log 資料表
- https://www.runoob.com/sql/sql-tutorial.html
```sql
/*
 Navicat MySQL Data Transfer

 Source Server         : 127.0.0.1
 Source Server Version : 50621
 Source Host           : localhost
 Source Database       : RUNOOB

 Target Server Version : 50621
 File Encoding         : utf-8

 Date: 05/18/2016 14:15:39 PM
*/

SET NAMES utf8;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
--  Table structure for `access_log`
-- ----------------------------
DROP TABLE IF EXISTS `access_log`;
CREATE TABLE `access_log` (
  `aid` int(11) NOT NULL AUTO_INCREMENT,
  `site_id` int(11) NOT NULL DEFAULT '0' COMMENT '網站id',
  `count` int(11) NOT NULL DEFAULT '0' COMMENT '訪問次數',
  `date` date NOT NULL,
  PRIMARY KEY (`aid`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;

-- ----------------------------
--  Records of `access_log`
-- ----------------------------
BEGIN;
INSERT INTO `access_log` VALUES ('1', '1', '45', '2016-05-10'), ('2', '3', '100', '2016-05-13'),
('3', '1', '230', '2016-05-14'), ('4', '2', '10', '2016-05-14'), ('5', '5', '205', '2016-05-14'),
('6', '4', '13', '2016-05-15'), ('7', '3', '220', '2016-05-15'), ('8', '5', '545', '2016-05-16'),
('9', '3', '201', '2016-05-17');
COMMIT;

SET FOREIGN_KEY_CHECKS = 1;

```
