[安装](https://blog.csdn.net/bobo553443/article/details/81383194)

[删除](https://jingyan.baidu.com/article/e9fb46e150735a7521f76681.html)


0. 管理员身份 C:\Program Files\MySQL\MySQL Server 8.0\bin>
1. 首次登录需修改初始密码 mysql> alter user 'root'@'localhost' identified by '19981016';
2. mysql> show databases;

		+--------------------+
		| Database           |
		+--------------------+
		| information_schema |
		| mysql              |
		| performance_schema |
		| sys                |
		+--------------------+
        4 rows in set (0.03 sec)
3. mysql> use mysql
    > Database changed
4. mysql> show tables;
5. mysql> select user,host, Password_reuse_history  from user;

		+------------------+-----------+------------------------+
		| user             | host      | Password_reuse_history |
		+------------------+-----------+------------------------+
		| mysql.infoschema | localhost |                   NULL |
		| mysql.session    | localhost |                   NULL |
		| mysql.sys        | localhost |                   NULL |
		| root             | localhost |                   NULL |
		+------------------+-----------+------------------------+
        4 rows in set (0.00 sec)
6. select * from userx;
6. describe likex;
6. quit;
