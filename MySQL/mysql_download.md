## MySQL Windows Download
1. 下载地址 ：https://dev.mysql.com/downloads/mysql/
   ![](https://github.com/Duk1906/Learning_Records/blob/master/Pictures/mysql.jpg)

2. 解压到你喜欢的文件夹，我选择 D:\，最后结果是D:\mysql-8.0.15-winx64
3. 在 D:\mysql-8.0.15-winx64 新建 my.ini配置文件，编辑 my.ini 配置以下基本信息：

		[mysql]
		# 设置mysql客户端默认字符集
		default-character-set=utf8
		 
		[mysqld]
		# 设置3306端口
		port = 3306
		# 设置mysql的安装目录
		basedir=D:\mysql-8.0.15-winx64
		# 允许最大连接数
		max_connections=50
		# 服务端使用的字符集默认为8比特编码的latin1字符集
		character-set-server=UTF8MB4
		# 创建新表时将使用的默认存储引擎
		default-storage-engine=INNODB

4. 以管理员身份打开 cmd 命令行工具，切换目录：
     1. > D:
     2. > cd D:\mysql-8.0.15-winx64\bin

5. D:\mysql-8.0.15-winx64\bin>mysqld --remove  (跳过5，后面步骤报错再从5开始往下执行)
    > Service successfully removed.

6.  初始化数据库 D:\mysql-8.0.15-winx64\bin>mysqld --initialize-insecure
    > 2019-02-19T13:57:17.092510Z 5 [Note] [MY-010454] [Server] A temporary >password is generated for root@localhost: pfe&&QZar4dE
  
7. 安装 D:\mysql-8.0.15-winx64\bin>mysqld install
    > Service successfully installed.
8. 启动/停止 D:\mysql-8.0.15-winx64\bin>net start/stop mysql
    > MySQL 服务正在启动 ....
    > MySQL 服务已经启动成功。
    
9. 登录 D:\mysql-8.0.15-winx64\bin>mysql -uroot -p
    > Enter password: ************ (上面的pfe&&QZar4dE)

10. 首次登录需修改初始密码 mysql> alter user 'root'@'localhost' identified by '19981016';
11. mysql> show databases;

		+--------------------+
		| Database           |
		+--------------------+
		| information_schema |
		| mysql              |
		| performance_schema |
		| sys                |
		+--------------------+
        4 rows in set (0.03 sec)
12. mysql> use mysql
    > Database changed
13. mysql> show tables;
14. mysql> select user,host, Password_reuse_history  from user;

		+------------------+-----------+------------------------+
		| user             | host      | Password_reuse_history |
		+------------------+-----------+------------------------+
		| mysql.infoschema | localhost |                   NULL |
		| mysql.session    | localhost |                   NULL |
		| mysql.sys        | localhost |                   NULL |
		| root             | localhost |                   NULL |
		+------------------+-----------+------------------------+
        4 rows in set (0.00 sec)
