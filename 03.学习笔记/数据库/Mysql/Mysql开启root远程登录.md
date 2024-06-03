## 1.直接创建一个允许从所有地址连接的用户

```mysql
CREATE USER '用户名'@'%' IDENTIFIED BY '密码';
```

## 2.给该用户赋权

```mysql
GRANT ALL PRIVILEGES ON *.* TO '用户名'@'%';
```

## 3.刷新权限，重启数据库

```mysql
FLUSH PRIVILEGES;
```
