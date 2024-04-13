
```bash
apt install mysql-server # 安装mysql
```
```sql
mysqladmin -uroot -p123456 password root       --修改root的密码
CREATE USER 'gpt'@'%' IDENTIFIED BY '***';     --新建用户gpt
CREATE DATABASE gpt_db;                        --新建库
GRANT ALL PRIVILEGES ON gpt_db.* TO 'gpt'@'%'; --给用户授权
```

### /etc/my.cnf
```
[mysqld]
user=root
skip-grant-tables
[mysql]
user=root
socket=/var/lib/mysql/mysql.sock
[mysqld_safe]
user=root
pid-file=/var/lib/mysql/mysqld.pid
socket=/var/lib/mysql/mysql.sock
[mysqladmin]
user=root
socket=/var/lib/mysql/mysql.sock
```



```
SET GLOBAL validate_password.LENGTH = 0;
SET GLOBAL validate_password.policy = 0;
SET GLOBAL validate_password.mixed_case_count = 0;
SET GLOBAL validate_password.number_count = 0;
SET GLOBAL validate_password.special_char_count = 0;
SET GLOBAL validate_password.check_user_name = 0;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root'; 

FLUSH PRIVILEGES;

CREATE USER 'app'@'%' IDENTIFIED BY 'app';     --新建用户gpt
CREATE DATABASE sharingan;                        --新建库
GRANT ALL PRIVILEGES ON sharingan.* TO 'app'@'%'; --给用户授权
```