
```bash
apt install mysql-server # 安装mysql
```
```sql
mysqladmin -uroot -p123456 password root       --修改root的密码
CREATE USER 'gpt'@'%' IDENTIFIED BY '***';     --新建用户gpt
CREATE DATABASE gpt_db;                        --新建库
GRANT ALL PRIVILEGES ON gpt_db.* TO 'gpt'@'%'; --给用户授权
```
