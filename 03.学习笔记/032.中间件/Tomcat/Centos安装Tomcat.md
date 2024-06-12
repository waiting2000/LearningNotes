# 1.部署java环境
## 1.1.安装java（二进制安装）
`ln -s jdk1.8.0_391 jdk`
![[Pasted image 20240519095926.png]]

## 1.2.添加环境变量
`vim /etc/profile.d/jdk.sh`
```shell
export JAVA_HOME=/usr/local/jdk
export PATH=$PATH:$JAVA_HOME/bin
```
![[Pasted image 20240519095952.png]]
## 2.安装Tomcat
下载地址：[Index of /apache/tomcat/tomcat-8/v8.5.98/bin](https://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.98/bin/)
## 2.1.建立软链接（便于更换版本）
`[root@taozhichen local]# ln -s apache-tomcat-8.5.98 tomcat`
## 2.2.设置环境变量
```shell
[root@rocky8 local]#echo 'PATH=/usr/local/tomcat/bin:$PATH' > /etc/profile.d/tomcat.sh
[root@rocky8 local]#cat /etc/profile.d/tomcat.sh
#通过该文件，可以设置 Tomcat 的路径、Java 环境变量、系统属性等
[root@rocky8 local]#. /etc/profile.d/tomcat.sh 
[root@rocky8 local]# echo $PATH
```
![[Pasted image 20240519100041.png]]
## 2.3.查看java环境变量和版本信息
`catalina.sh version`
![[Pasted image 20240519100021.png]]
## 2.4.启动tomcat
![[Pasted image 20240519100056.png]]
**云服务器需要开启8080访问端口安全组**
![[Pasted image 20240519100119.png]]
2.5.配置tomcat启动
```shell
[root@taozhichen conf]# useradd -s /sbin/nologin tomcat
[root@taozhichen conf]# vim tomcat.conf
[root@taozhichen conf]# chown -R /usr/local/tomcat
[root@taozhichen conf]# chown -R tomcat.tomcat /usr/local/tomcat/
[root@taozhichen conf]# vim /lib/systemd/system/tomcat.service

[Unit]
Description=Tomcat
#After=syslog.target network.target remote-fs.target nss-lookup.target
After=syslog.target network.target
 
[Service]
Type=forking
EnvironmentFile=/usr/local/tomcat/conf/tomcat.conf
ExecStart=/usr/local/tomcat/bin/startup.sh
ExecStop=/usr/local/tomcat/bin/shutdown.sh
RestartSec=3
PrivateTmp=true
User=tomcat
Group=tomcat
 
[Install]
WantedBy=multi-user.target
```
![[Pasted image 20240519100141.png]]

