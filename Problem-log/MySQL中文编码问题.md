## MySQL 中文乱码或报错问题

### 原因
数据库编码不是utf-8编码

### 修改方法
将数据库编码修改为utf-8编码

### 具体操作步骤
#### Windows：
1. 在```mysql```的安装目录下找到```my.ini```文件或者```my-default.ini```文件。
2. 在```[mysqld]```标签下添加：```character-set-server=utf8```
3. 在```[client]```标签下添加：```default-character-set=utf8```
4. 重启```mysql```服务
5. 进入```MySQL```，输入```show variables like "%char%";```若所有的字符集均显示为utf8，则修改完成。

#### Linux：
1. 在```mysql```的安装目录下找到```my.cnf```文件。
2. 在```[mysqld]```标签下添加：```character-set-server=utf8```
3. 在```[client]```标签下添加：```default-character-set=utf8```
4. 重启```mysql```服务
5. 进入```MySQL```，输入```show variables like "%char%";```若所有的字符集均显示为utf8，则修改完成。