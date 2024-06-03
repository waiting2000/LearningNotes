# Docker基础指令
## 查找镜像：docker search

- NAME：镜像仓库源的名称
- DESCRIPTION：镜像描述
- OFFICiAL：是否docker官方发布
- stars：评分
- AUTOMATED：自动构建

![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270943876.png)
## 删除镜像：docker rmi
## 创建镜像：
# 在CentOs环境下安装docker

## 使用官方脚本安装

```shell
# 直接官方脚本安装
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270943979.png)

![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270944194.png)
## 设置开机自启动
### 1.查看是否设置开机启动
```shell
systemctl list-unit-files | grep docker
```
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270944142.png)
### 2.开启开机自启动
```shell
# docker自启动
systemctl enable docker.service
```
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270944827.png)
```shell
# docker里容器自启动
docker update --restart=always 容器名
```
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270944700.png)
# 在docker中部署mysql
## 1.拉取Mysql镜像
docker镜像仓库：[https://hub-stage.docker.com/_/mysql/tags](https://hub-stage.docker.com/_/mysql/tags)
```shell
docker pull mysql:latest
```
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270945587.png)
## 2.查看本地镜像
```shell
# 查看容器镜像
docker images
```
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270945501.png)
## 3.运行容器
```shell
docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql
```
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270945061.png)

- -p 3306:3306 : 映射容器服务的 3306 端口到宿主机的 3306 端口，外部主机可以直接通过 **宿主机ip:3306** 访问到 MySQL 的服务。
## 4.配置网络访问权限
### 4.1.root外网连接权限配置
```shell
# 外网范围授权（mysql 8.0以上不适用）
grant all privileges on *.* to root@'%' identified by 'password';
# 一定要更新密码，否则Navicat连接报1045错误
alter user 'root'@'%' identified with mysql_native_password by '123456';
# 刷新权限
flush privileges;
```
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270945917.png)
### 4.2.开放3306端口
```shell
# 1.开放3306端口正常法
systemctl start firewalld
firewall-cmd  --zone=public --add-port=3306/tcp --permanent
firewall-cmd  --reload
# 2.直接关防火墙法(1或2选一种就行)
systemctl stop firewalld
```
## 5.配置安全组
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270945417.png)<br />![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270945367.png)
# 在docker中部署nginx
## 可以使用 docker search nginx 来查找可用版本
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270946060.png)
## 1.拉取nginx镜像
```shell
$ docker pull nginx:latest
```
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270946473.png)
## 2.运行容器
```shell
$ docker run --name nginx-test -p 8080:80 -d nginx
```
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270946693.png)
## 3.要在华为云上配置安全组
![](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405270946848.png)
