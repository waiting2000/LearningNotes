# 检查是否安装audit_log插件

`SELECT * FROM information_schema.PLUGINS WHERE PLUGIN_NAME LIKE '%audit%';`
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405201934466.png)
# 下载插件库
## 确认C库版本

`ldd --version`
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405201935864.png)
## 下载插件库

下载地址：
https://docs.percona.com/percona-server/8.0/binary-tarball-install.html
`wget https://downloads.percona.com/downloads/Percona-Server-8.0/Percona-Server-8.0.36-28/binary/tarball/Percona-Server-8.0.36-28-Linux.x86_64.glibc2.17-minimal.tar.gz?_gl=1*2cagac*_gcl_au*MTEwMzEyNDg4MC4xNzE2MTk2NTg0`

查看Mysql插件目录
`mysql> show variables like '%plugin_dir%';`
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405202210230.png)

解压、复制进Mysql插件目录
```shell
cd /root/Percona-Server-8.0.36-28-Linux.x86_64.glibc2.17-minimal
cp audit_log.so /usr/lib64/mysql/plugin/
```
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405202210332.png)

## 安装插件（在Mysql命令行进行）

```mysql
install plugin audit_log soname 'audit_log.so';
```
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405202210441.png)
## 验证是否成功

```mysql
SELECT * FROM information_schema.PLUGINS WHERE PLUGIN_NAME LIKE '%audit%'\G

SHOW variables LIKE 'audit%';
```
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405202211709.png)

![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405202211436.png)

## 添加配置后，重启服务

```shell
# mysql_audit
plugin-load = audit_log.so
# 该文件必须提前创建，并确保Mysql用户能够写入
audit_log_file = /var/log/mysqld_audit.log
audit_log_format = JSON
audit_log_policy = ALL
audit_log_handler = FILE
audit_log_rotate_on_size = 1048576
```
**修改用户组（这一步很重要，未修改会导致配置无法生效）**
`chown mysql:mysql /var/log/mysqld_audit.log
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405202212822.png)
## 完成
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405202213250.png)