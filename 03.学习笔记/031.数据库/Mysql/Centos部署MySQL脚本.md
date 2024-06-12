---
创建时间: 2024-05-28
---
```shell
#!/bin/bash

#解决软件的依赖关系
yum install libaio

#解压mysql二进制安装包
tar xf {file_name}

#移动mysql解压后的文件到/usr/local下改名叫mysql
mv {file_name} /usr/local/mysql

#新建组mysql
groupadd mysql

#新建用户mysql,mysql这个用户的shell是/bin/false,属于mysql组
useradd -g mysql -s /sbin/nologin mysql

#关闭防火墙firewalld这个服务，并且设置开机不要启动
service firewalld stop
systemctl disable firewalld

#mysql的初始化操作
mkdir /data/mysql -p
chown -R mysql:mysql /data/mysql/
chomd 750 /data/mysql/

#进入/usr/local/mysql/bin目录
cd /usr/local/mysql/

#初始化mysql
./bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql/ --datadir=/data/mysql &>passwd.txt

#获得临时密码
tem_passwd=$(cat passwd.txt | grep "temporary" | awk '{print $NF}')
#$NF 表示最后一个字段

#修改PATH变量，加入mysql下的bin目录的路径
#临时修改PATH变量的值
export PATH=/usr/local/mysql/bin/:$PATH
#重新启动linux系统后也生效，永久修改
echo 'PATH=/usr/local/mysql/bin:$PATH' >>/root/.bashrc

#复制support-files里的mysql.server文件到/etc/init.d/目录下叫mysqld
cp ../support-files/mysql.server /etc/init.d/mysqld
#修改/etc/init.d/mysqld脚本文件里的datadir目录的值
sed -i '70c datadir=/data/mysql/' /etc/init.d/mysqld

#生成/etc/my.cnf配置文件=>传递参数
cat >/etc/my.cnf <<EOF
[mysqld_safe]

[client]
socket=/data/mysql/mysql.sock

[mysqld]
socket=/data/mysql/mysql.sock
port = 3306
open_files_limit = 8192
innodb_buffer_pool_size = 512M
character-set-server=utf8

[mysql]
auto-rehash
prompt=\\u@\\d \\R:\\m mysql>
EOF

#启动mysqld进程服务
service mysqld start

#将mysqld添加到linux系统里的服务管理名单
/sbin/chkconfig --add mysqld
#设置mysqld服务开机自启动
/sbin/chkconfig mysqld on

#登录重新设置初始密码为Sanchuang123#
#初次修改密码需要使用--connect-expired-password 选项
#-e 后面接的表示在mysql里需要执行的命令
mysql -uroot -p$tem_passwd --connect-expired-password -e "set password='（此处输入设置密码）';"
#检验上一步修改密码是否成功，验证密码是否设置成功，如果有输出就能看到mysql里的数据库，说明成功
mysql -uroot -p（接你设置的密码） -e 'show databases;' && echo "database is installed success"

```
