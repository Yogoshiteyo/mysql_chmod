# 修复ERROR 1045 (28000): Access denied for user 'root'@'%' (using password: YES)
直接修改 mysql.user 表中的权限字段
停用 MySQL 服务并启动 MySQL 安全模式

在终端中执行以下命令停止 MySQL 服务并以安全模式启动：

```bash
sudo service mysql stop
sudo mysqld_safe --skip-grant-tables &
```
以 root 用户连接 MySQL

以 root 用户登录 MySQL：

```bash
mysql -u root
```
修改 mysql.user 表中的权限字段

执行以下 SQL 语句来直接修改 mysql.user 表中的权限字段：

```sql
USE mysql;
UPDATE user SET Grant_priv='Y', Super_priv='Y' WHERE User='root' AND Host='%';
UPDATE user SET Grant_priv='Y', Super_priv='Y' WHERE User='root' AND Host='localhost';
FLUSH PRIVILEGES;
```
重启 MySQL 服务

退出 MySQL 并重启 MySQL 服务：

```sql
exit
sudo service mysql restart
```
重新连接并验证

使用新密码重新连接 MySQL：

```bash
mysql -u root -p
```
输入密码后，执行以下命令确认 root 用户的权限：

```sql
SELECT host, user, Grant_priv, Super_priv FROM mysql.user WHERE user='root';
```
确保 Grant_priv 和 Super_priv 都设置为 'Y'。

再次尝试授予全局权限
授予 root 用户全局权限

使用以下命令授予 root 用户全局权限：

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '88888888' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
授予 game 用户权限

然后授予 game 用户在 localhost 上的权限：

```sql
GRANT ALL PRIVILEGES ON *.* TO 'game'@'localhost' IDENTIFIED BY '88888888';
FLUSH PRIVILEGES;
```
检查和验证权限
重新连接并验证

尝试重新连接并验证权限：

```bash
mysql -u game -p
```
使用密码 88888888 连接，确保能成功连接并有适当权限。

