---
创建时间: 2024-06-07
tags:
  - 个人
---
# Docker架构
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202406071659022.png)
容器的特点：
- 类似轻量级的VM
- 共享操作系统内核
- 拥有自己的文件系统、CPU、内存、进程空间等
- 互相隔离
==轻量 快速 隔离 跨平台 高密度==
# 安装Docker
==阿里云镜像地址：http://mirrors.aliyun.com/==
```shell
# 移除旧版本docker
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

# 配置docker yum源。
sudo yum install -y yum-utils
sudo yum-config-manager \
--add-repo \
http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo


# 安装 最新 docker
sudo yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 启动& 开机启动docker； enable + start 二合一
systemctl enable docker --now

# 配置加速
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://mirror.ccs.tencentyun.com","https://82m9ar63.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
# Docker相关操作
## 下载镜像
```shell
#查看运行中的容器
docker ps
#查看所有容器
docker ps -a
#搜索镜像
docker search nginx
#下载镜像
docker pull nginx
#下载指定版本镜像
docker pull nginx:[tag]
#查看所有镜像
docker images
#删除指定id的镜像
docker rmi [image_id/image_name]
```
## 运行容器
```shell
#运行一个新容器
docker run nginx
#停止容器
docker stop [NAMES]
#启动容器
docker start [CONTAINER ID / NAMES]
#重启容器
docker restart [CONTAINER ID / NAMES]
#查看容器资源占用情况
docker stats [CONTAINER ID / NAMES]
#查看容器日志
docker logs [CONTAINER ID / NAMES]
#删除指定容器
docker rm [CONTAINER ID / NAMES]
#强制删除指定容器
docker rm -f [CONTAINER ID / NAMES]
# 后台启动容器
docker run -d --name mynginx nginx
# 后台启动并暴露端口
docker run -d --name mynginx -p 80:80 nginx
# 进入容器内部
docker exec -it mynginx /bin/bash
```
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202406111448778.png)
