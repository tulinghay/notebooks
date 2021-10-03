# windows安装

下载：https://dev.mysql.com/downloads/mysql/

将文件解压到目标目录，新增MYSQL_HOME变量，值为解压根目录

新建my.ini文件，内容如下，需要修改一下basedir：

```sql
[client]
# 设置mysql客户端默认字符集
default-character-set=utf8
 
[mysqld]
# 设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=d:\\hdt\\mysql8.0
# 设置 mysql数据库的数据的存放目录，MySQL 8+ 不需要以下配置，系统自己生成即可，否则有可能报错
# datadir=C:\\web\\sqldata
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

管理员运行命令行：

```bash
#初始化数据库，会生成一个乱码的初始密码，如下N-Vf(/T,o1)*
#2021-09-29T08:11:18.727819Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: N-Vf(/T,o1)*
mysqld.exe --initialize --console

mysqld.exe install
net start mysql
#使用上面的初始密码登录
mysql -uroot -pN-Vf(/T,o1)*
#登录数据库后需要修改初始化密码，修改为qwe
alter user user() identified by "qwe";

```

