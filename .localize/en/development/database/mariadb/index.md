---
title: MariaDB
tags:
  - MariaDB
  - SQL
description:
---

## Reference(Japanese)

### For Mac

- HomebrewでMacにMariaDBをインストールする手順と設定

[https://oopsoop.com/steps-and-settings-to-install-mariadb-on-mac/](https://oopsoop.com/steps-and-settings-to-install-mariadb-on-mac/)

- mysql関連インストール済みであれば下記記事を参考に削除する（動作確認済み）
  - どうしよう！困った時のMac上のMySQLのアンインストール＆再インストール、動作確認手順

[https://qiita.com/akiko-pusu/items/aef52b723da2cb5dc596](https://qiita.com/akiko-pusu/items/aef52b723da2cb5dc596)

mariadbをアンインストールする場合は下記記事参考

- homebrewでインストールしたmariadbをアンインストールする

[https://qiita.com/B73W56H84/items/61af451f71ce9263c68e](https://qiita.com/B73W56H84/items/61af451f71ce9263c68e)

### For RaspberryPi(Rasbian/Ubuntu) / WSL2(Ubuntu)

- 【Ubuntu】MariaDBをインストールする：初期設定（セキュリティ、データベース）

[https://office54.net/iot/linux/ubuntu-mariadb-install](https://office54.net/iot/linux/ubuntu-mariadb-install)

- Raspberry Pi にデータベースを構築する【MySQL，MariaDB】

[https://nort-wmli.blogspot.com/2019/06/raspberry-pi-mysqlmariadb.html?m=1](https://nort-wmli.blogspot.com/2019/06/raspberry-pi-mysqlmariadb.html?m=1)

## Install MariaDB

### For Mac

```zsh
brew install mariadb  
==> Downloading https://ghcr.io/v2/homebrew/core/mariadb/manifests/10.8.3_1-1
Already downloaded: $HOME/Library/Caches/Homebrew/downloads/1eb85a48dd5839716d551949cc6b6bb380e780c931597278b6622407afdbbd55--mariadb-10.8.3_1-1.bottle_manifest.json
==> Downloading https://ghcr.io/v2/homebrew/core/mariadb/blobs/sha256:8df94b6571
Already downloaded: $HOME/Library/Caches/Homebrew/downloads/d213cff4c288f685100a941f78704b384832a0dbd8575e63a1b773527bd0b304--mariadb--10.8.3_1.monterey.bottle.1.tar.gz
==> Pouring mariadb--10.8.3_1.monterey.bottle.1.tar.gz
==> /usr/local/Cellar/mariadb/10.8.3_1/bin/mysql_install_db --verbose --user=k.y
==> Caveats
A "/etc/my.cnf" from another install may interfere with a Homebrew-built
server starting up correctly.

MySQL is configured to only allow connections from localhost by default

To restart mariadb after an upgrade:
  brew services restart mariadb
Or, if you don't want/need a background service you can just run:
  /usr/local/opt/mariadb/bin/mysqld_safe --datadir=/usr/local/var/mysql
==> Summary
🍺  /usr/local/Cellar/mariadb/10.8.3_1: 922 files, 187.6MB
==> Running `brew cleanup mariadb`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
```

### For RaspberryPi(Rasbian/Ubuntu) / WSL2(Ubuntu)

```bash
sudo apt install mariadb-server
```

## Startup confirmation and initial setup (no difference between Mac/WSL2/Ubuntu)

```zsh
% mysql --version
mysql  Ver 15.1 Distrib 10.8.3-MariaDB, for osx10.17 (x86_64) using  EditLine wrapper

% sudo mysql_secure_installation
Password:

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): [そのままenter]
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] n
 ... skipping.

You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] Y
New password: [Input Passwrd]
Re-enter new password: [Input Passwrd]
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] Y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] Y
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] Y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] Y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

## Login

### For Mac

```zsh
% mysql -u root -p
Enter password: [Input New Password]
Welcome to the MariaDB monitor.  Commands end with ; or g.
Your MariaDB connection id is 31
Server version: 10.8.3-MariaDB Homebrew

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or 'h' for help. Type 'c' to clear the current input statement.
```

### For WSL2/Ubuntu

Also available on Mac

```zsh
sudo mysql -u root -p
```

```zsh
Enter password: [Input Password]
Welcome to the MariaDB monitor.  Commands end with ; or g.
Your MariaDB connection id is 31
Server version: 10.8.3-MariaDB Homebrew

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or 'h' for help. Type 'c' to clear the current input statement.
```

## User List Confirmation

```bash
MariaDB [(none)]> select host,user,password from mysql.user;
```

## Check the database

```bash
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| pets               |
| sys                |
+--------------------+
8 rows in set (0.018 sec)
```

## Create the Database

### CREATE DATABASE "DB Name";

```bash
MariaDB [(none)]> CREATE DATABASE myTestDB1;
Query OK, 1 row affected (0.001 sec)

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| myTestDB1          |　# Added Database
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.001 sec)

MariaDB [(none)]> use myTestDB1
Database changed
```

## Adding tables (across multiple rows)

### CREATE TABLE `` `[TABLE Name]` ``;

```bash
CREATE TABLE `users2`
(
    `id` int(11) NOT NULL AUTO_INCREMENT,
    `email` varchar(255) COLLATE utf8_bin NOT NULL,
    `password` varchar(255) COLLATE utf8_bin NOT NULL,
    PRIMARY KEY (`id`)
)
ENGINE=InnoDB DEFAULT
CHARSET=utf8mb4
COLLATE=utf8mb4_bin
AUTO_INCREMENT=1;
```

#### <Supplement> "->" is inserted when pasting in the terminal.

```bash
MariaDB [myTestDB2]> CREATE TABLE `users2`
-> (
    ->   `id` int(11) NOT NULL AUTO_INCREMENT,
    ->   `email` varchar(255) COLLATE utf8_bin NOT NULL,
    ->   `password` varchar(255) COLLATE utf8_bin NOT NULL,
    ->   PRIMARY KEY (`id`)
-> )
-> ENGINE=InnoDB DEFAULT
-> CHARSET=utf8mb4
-> COLLATE=utf8mb4_bin
-> AUTO_INCREMENT=1;
Query OK, 0 rows affected (0.021 sec)
```

## Delete database

### DROP DATABASE "DB Name";

```bash
MariaDB [myTestDB1]> drop database myTestDB2;
Query OK, 3 rows affected (0.286 sec)

MariaDB [myTestDB1]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| myTestDB1          |
| mysql              |
| performance_schema |
| pets               |
| sys                |
+--------------------+
8 rows in set (0.000 sec)
```

## Select Database

```bash
MariaDB [(none)]> use myTestDB1;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

## Show Table List

```bash
MariaDB [myTestDB1]> show tables;
+---------------------+
| Tables_in_myTestDB1 |
+---------------------+
| users               |
+---------------------+
1 row in set (0.000 sec)
```

## Create the table

```bash
MariaDB [(none)]> CREATE DATABASE ec_sol_db1;
```

## Add table (all in one line)

```bash
MariaDB [(none)]> CREATE TABLE ec_sol_tbl1(id INT AUTO_INCREMENT, cameratype char(9), date DATETIME, num INT,  foo1 INT, foo2 INT, foo3 INT, foo4 INT, foo5 INT,PRIMARY KEY (id));
```

## Check data type

```bash
MariaDB [ec_sol_db1]> describe ec_sol_tbl1;
+------------+----------+------+-----+---------+----------------+
| Field      | Type     | Null | Key | Default | Extra          |
+------------+----------+------+-----+---------+----------------+
| id         | int(11)  | NO   | PRI | NULL    | auto_increment |
| cameratype | char(9)  | YES  |     | NULL    |                |
| date       | datetime | YES  |     | NULL    |                |
| num        | int(11)  | YES  |     | NULL    |                |
| foo1       | int(11)  | YES  |     | NULL    |                |
| foo2       | int(11)  | YES  |     | NULL    |                |
| foo3       | int(11)  | YES  |     | NULL    |                |
| foo4       | int(11)  | YES  |     | NULL    |                |
| foo5       | int(11)  | YES  |     | NULL    |                |
+------------+----------+------+-----+---------+----------------+
9 rows in set (0.005 sec)
```

## Checking the table contents

```bash
MariaDB [ec_sol_db1]> select * from ec_sol_tbl1;
+----+------------+---------------------+------+------+------+------+------+------+
| id | cameratype | date                | num  | foo1 | foo2 | foo3 | foo4 | foo5 |
+----+------------+---------------------+------+------+------+------+------+------+
|  1 | serial111  | NULL                | NULL | NULL | NULL | NULL | NULL | NULL |
|  2 | serial111  | 2022-10-02 23:04:33 | NULL | NULL | NULL | NULL | NULL | NULL |
|  3 | serial111  | 2022-10-02 23:05:20 |    1 | NULL | NULL | NULL | NULL | NULL |
|  4 | serial111  | 2022-10-02 23:06:39 |    1 |    4 | NULL | NULL | NULL | NULL |
|  5 | serial111  | 2022-10-02 23:07:55 |    1 |    3 |    0 |    1 |    1 |    4 |
|  6 | serial111  | 2022-10-02 23:08:11 |    1 |    4 |    2 |    0 |    5 |    4 |
|  7 | serial111  | 2022-10-02 23:08:13 |    1 |    5 |    2 |    3 |    4 |    1 |
|  8 | serial111  | 2022-10-02 23:08:15 |    1 |    0 |    4 |    1 |    0 |    2 |
|  9 | serial111  | 2022-10-02 23:32:16 |    1 |    3 |    1 |    3 |    0 |    0 |
| 10 | serial111  | 2022-10-03 22:45:54 |    1 |    1 |    1 |    5 |    0 |    2 |
| 11 | serial111  | 2022-10-03 22:46:05 |    1 |    2 |    2 |    0 |    1 |    3 |
| 12 | serial111  | 2022-10-03 22:46:07 |    1 |    0 |    2 |    5 |    5 |    0 |
| 13 | serial111  | 2022-10-03 22:46:10 |    1 |    1 |    3 |    1 |    2 |    5 |
+----+------------+---------------------+------+------+------+------+------+------+
13 rows in set (0.000 sec)
```

## Empty the table

```bash
MariaDB [ec_sol_db1]> truncate table ec_sol_tbl1;
Query OK, 0 rows affected (0.033 sec)

MariaDB [ec_sol_db1]> select * from ec_sol_tbl1;
Empty set (0.001 sec)
```

## Confirmation of data addition by pymysql

参考：PythonでMySQLを操作する（PyMySQL）<br />
[https://python-work.com/pymysql/](https://python-work.com/pymysql/)

## Check access from other PCs

```bash
mysql -h [IP Address or Hostname] -u [Registered User Name] -p
```

## Status check

### For RaspberryPi / WSL2(Ubuntu)

```bash
sudo service mysql status
```

### For Mac

```bash
ps ax | grep mysql
```

## Service startup commands(start,stop,restart)

### For RaspberryPi / WSL2(Ubuntu)

- sudo systemctl start mariadb
- sudo systemctl stop mariadb
- sudo systemctl restart mariadb

### For Mac

- brew services start mariadb
- brew services stop mariadb
- brew services restart mariadb
