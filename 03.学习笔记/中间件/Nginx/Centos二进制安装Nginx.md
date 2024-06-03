---
创建时间: 2024-05-27
---
# 1.安装依赖
`yum -y install make zlib zlib-devel gcc-c++ libtool openssl openssl-devel pcre pcre-devel`

# 2.解压编译安装
```shell
[root@localhost nginx]# tar -zxvf nginx-1.20.2.tar.gz 
[root@localhost nginx]# cd nginx-1.20.2/

#--prefix指定安装的路径，组件文件统一存放，方便日后卸载删除
[root@localhost nginx-1.20.2]# ./configure --prefix=/usr/local/nginx
#如果服务需要支持ssl安全，如https，则需要安装ssl模块，该语句也可以与上一条一起执行
[root@localhost nginx-1.20.2]# ./configure --with-http_ssl_module --with-stream_ssl_module --with-stream_ssl_preread_module

#-j 4指定并行编译任务数，适用多核CPU，一般配置为cpu核数的2倍
[root@localhost nginx-1.20.2]# make -j 4 && make install
```
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405271407470.png)
# 3.添加nginx环境变量
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405271407689.png)
## 4.配置nginx.service
`vim /usr/lib/systemd/system/nginx.service`
```shell
[Unit]
Description=nginx service
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405271407792.png)
