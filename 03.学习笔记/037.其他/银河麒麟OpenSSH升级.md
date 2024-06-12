---
创建时间: 2024-05-28
---
==服务器架构不影响源码安装部署==
## 当前服务器架构
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1711678071856-dc4a9c8a-a3f0-411b-a850-4cf84ff1718f.png#averageHue=%23151729&clientId=u8a95981d-3799-4&from=paste&height=75&id=ue1eb47b1&originHeight=150&originWidth=2522&originalType=binary&ratio=2&rotation=0&showTitle=false&size=92428&status=done&style=none&taskId=u72adb26d-0ab6-4c56-979b-5e5f9d31864&title=&width=1261)
## 部署telnet服务
### 安装服务
```shell
# 安装
yum -y install telnet-server xinetd

# 启用root登陆
# 这一步，本服务器默认可使用root登陆，可以启动telnet后，先尝试用root登陆
sed -i 's/^#\s*auth\s*required\s*pam_securetty.so/auth required pam_securetty.so/' /etc/pam.d/remote

sed -i 's/^#\s*auth.*pam_securetty.so/auth [user_unknown=ignore success=ok ignore=ignore default=bad] pam_securetty.so/' /etc/pam.d/login

egrep 'auth\s*required\s*pam_securetty.so' /etc/pam.d/remote || sed -i '1aauth required pam_securetty.so' /etc/pam.d/remote
egrep 'auth.*pam_securetty.so' /etc/pam.d/login || sed -i '1aauth [user_unknown=ignore success=ok ignore=ignore default=bad] pam_securetty.so' /etc/pam.d/login
```
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405281105646.png)![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405281105170.png)
### 验证服务
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405281105910.png)
## 更新操作
### 安装依赖
`yum -y install gcc zlib-devel pam-devel perl `
### openssl编译
```shell
# 下载源码
wget https://www.openssl.org/source/openssl-1.1.1q.tar.gz
# 解压编译
tar -xzf openssl-1.1.1q.tar.gz
cd openssl-1.1.1q
./config shared --prefix=/usr/local/openssl

make && make install

# 备份旧版 openssl，后续如有问题，直接还原回来即可，其中 $(date +%F) 会自动解析为执行命令时的日期
mv /usr/bin/openssl /usr/bin/openssl_$(date +%F)

# 生成快捷方式并更新链接库
ln -s /usr/local/openssl/bin/openssl /usr/bin/openssl
ln -s /usr/local/openssl/include/openssl /usr/include/openssl
sed -i "1i/usr/local/openssl/lib" /etc/ld.so.conf
ldconfig

# 验证
openssl version
openssl prime -generate -bits 128
```
### openssh编译
```shell
# 下载源码
wget https://mirrors.aliyun.com/pub/OpenBSD/OpenSSH/portable/openssh-9.6p1.tar.gz

# 解压编译
tar -xzf openssh-9.6p1.tar.gz
cd openssh-8.9p1
./configure --prefix=/usr/local --sysconfdir=/etc/ssh --with-ssl-dir=/usr/local/openssl --with-pam --with-md5-passwords
make
# 赋权
chmod 600 /etc/ssh/ssh_host_rsa_key
chmod 600 /etc/ssh/ssh_host_ecdsa_key
chmod 600 /etc/ssh/ssh_host_ed25519_key
# 安装
make install
# 拷贝 ssh-copy-id
cp contrib/ssh-copy-id /usr/local/bin
chmod 755 /usr/local/bin/ssh-copy-id
# 验证
/usr/local/bin/ssh -V
# 卸载旧版 openssh，防止冲突，注意 ${installed_packages} 替换为 rpm -qa | grep openssh 查到的具体包
rpm -qa | grep openssh # 获取到安装的相关包 ${installed_packages}
rpm -e --nodeps ${installed_packages}
# 验证
ssh -V
```
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405281105104.png)
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405281105368.png)
### sshd_config配置
```shell
# 禁止 root 密码登陆
PermitRootLogin without-password
# 允许秘钥登陆
PubkeyAuthentication yes
# 关闭 PAM，未知原因开启该选项之后免密登陆会失效
UsePAM no
# 注释掉以下不支持的配置项
# GSSAPIAuthentication
# GSSAPICleanupCredentials
# Deprecated
# RhostsRSAAuthentication
```
**注意：如果配置中，UsePAM置为yes，会开启PAM认证，导致无法登陆，原因暂未知**
### systemd 配置
```shell
# 生成 sshd-keygen.service 文件并赋权
cat > /usr/lib/systemd/system/sshd-keygen.service << 'EOF'
[Unit]
Description=SSH Key Generation
ConditionPathExists=|!/etc/ssh/ssh_host_dsa_key
ConditionPathExists=|!/etc/ssh/ssh_host_dsa_key.pub
ConditionPathExists=|!/etc/ssh/ssh_host_ecdsa_key
ConditionPathExists=|!/etc/ssh/ssh_host_ecdsa_key.pub
ConditionPathExists=|!/etc/ssh/ssh_host_ed25519_key
ConditionPathExists=|!/etc/ssh/ssh_host_ed25519_key.pub
ConditionPathExists=|!/etc/ssh/ssh_host_rsa_key
ConditionPathExists=|!/etc/ssh/ssh_host_rsa_key.pub
[Service]
ExecStart=/usr/local/bin/ssh-keygen -A
Type=oneshot
RemainAfterExit=yes
EOF

chmod 644 /usr/lib/systemd/system/sshd-keygen.service

# 生成 sshd.service 文件并赋权
cat > /usr/lib/systemd/system/sshd.service << 'EOF'
[Unit]
Description=OpenSSH Daemon
Wants=sshd-keygen.service
After=sshd-keygen.service
After=network.target
[Service]
ExecStart=/usr/local/sbin/sshd -D
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=always
[Install]
WantedBy=multi-user.target
EOF

chmod 644 /usr/lib/systemd/system/sshd.service

# 重载 systemd
# Ps：Ubuntu 最小化安装时可能没有 systemctl 程序，可通过 apt install systemctl 进行安装
systemctl daemon-reload

# 启动 ssh
systemctl start sshd

# 查看 ssh 状态
systemctl status sshd

# 开机启动
systemctl enable sshd
```
### 验证
```shell
# 重启 sshd 服务
systemctl restart sshd

# 远程登陆，看是否可以正常登陆，注意替换 ${hostname} 为升级 OpenSSH 的服务器的 IP 地址
ssh root@${hostname}

# 验证无误后，关闭 telnet 服务即可
systemctl stop telnet.socket
systemctl stop xinetd
```
