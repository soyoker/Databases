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
