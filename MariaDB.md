### 数据库连接操作
---
设置 MySQL 密码
```bash
root@host:~# mysqladmin -u ${User} password ${Password}
```
连接到 MySQL 数据库服务器
```bash
root@host:~# mysql -u ${User} -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3509
Server version: 10.1.40-MariaDB-0ubuntu0.18.04.1 Ubuntu 18.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```
登录到指定的数据库
```bash
root@host:~# mysql -u ${User}  -D ${DBName} -p${Password}
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3510
Server version: 10.1.40-MariaDB-0ubuntu0.18.04.1 Ubuntu 18.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [${DBName}]>

```
登录到远程 MySQL 服务器；默认为 3306 端口
```bash
root@host:~# mysql -u ${User} -p -h ${DBHost} -P ${Port}
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 3517
Server version: 10.1.40-MariaDB-0ubuntu0.18.04.1 Ubuntu 18.04

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```
执行指定 SQL 指令返回结果
```bash
#不登陆数据库，只返回结果
root@host:~# mysql -u ${User} -p${Password} -e '${SQL}'
#登陆数据库，返回结果
root@host:~# mysql -u${User} -p${Password} -e '${SQL}'
```
参数含义：
```bash
root@host:~# mysql -u ${User} -p{Password} -h ${DBHost} -P ${Port} -e '${SQL}' -S ${MySQLSockPath} -C -E
#-h ：指定主机 IP 地址
#-P ：指定端口
#-e ：execute 执行模式
#-V ：纵向显示
#-C ：压缩数据传输
```
### 用户管理
---
数据库的用户信息存储在默认的 mysql 库

查看所有的数据库用户：
```bash
#切换到 mysql 库
MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names

Database changed
#查找 mysql 存储的所有用户名称
MariaDB [mysql]> select user from user;
+--------+
| user   |
+--------+
| zabbix |
| root   |
+--------+
2 rows in set (0.01 sec)
```
创建用户：
```bash
#1.创建一个没有密码的用户 ${Uer}
MariaDB [none]> create user '${User}'@'{Host}';
Query OK, 0 rows affected (0.00 sec)
#为用户 ${Uer} 设置密码
MariaDB [none]> set password for ${User}@${Host} = password('${Password}');
Query OK, 0 rows affected (0.00 sec)

#2.创建一个用户 ${User} 并设置它的密码为 ${Password}
MariaDB [none]> create user '${User}'@'{Host}' identified by '${Password}';
Query OK, 0 rows affected (0.00 sec)

#3.直接授权用户时（如果没有指定用户）自动创建该用户
MariaDB [none]> grant all on ${DBName}* to ${User}@${Host} identified by '${Password}';
Query OK, 0 rows affected (0.00 sec)
```
删除用户：
```bash
#1.使用 delete 条件语句删除用户，只删除用户字段
MariaDB [none]> delete from mysql.user where user='${User}' and host='${Host}';
Query OK, 1 row affected (0.00 sec)

#2.使用 drop 语句删除用户 删除关于 ${User} 的所有字段，包含权限
MariaDB [none]> drop user ${User}@${Host};
Query OK, 0 rows affected (0.00 sec)
```
重命名用户：
```bash
#rename user 只能改变 ${User} 字段
MariaDB [none]> rename user ${User}@${Host} to ${NewUser}@${Host};
Query OK, 0 rows affected (0.00 sec)
```
忘记 root 密码的处理方法：

  - 停止当前 SQL 进程

  - 略过 SQL 的认证开启 SQL 进程
```bash
root@host:~# mysqld_safe --skip-grant-tables &
```
  - 登录数据库(此时不需要密码)
```bash
root@host:~# mysql -uroot
```
  - 修改数据库中 root 的密码字段
```bash
MariaDB [(none)]> UPDATE mysql.user SET password=PASSWORD("${NewPassword}") WHERE user='root';
MariaDB [(none)]> FLUSH PRIVILEGES;
```
