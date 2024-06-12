 安装该版本时下载的是glibc为2.28版本，且操作系统实际为2.17安装过程也出了各种问题。故后续尝试安装另一版本
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705327570902-9ffe3bdb-0513-4f06-b2a9-903a0c891789.png#averageHue=%23151829&clientId=u341c9582-d35f-4&from=paste&height=109&id=ua203c892&originHeight=109&originWidth=455&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16203&status=done&style=none&taskId=u797cac0b-e620-47e4-8283-cc96556db21&title=&width=455) 

---

## 1.下载安装包（[https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)）
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705140881242-45cf2348-b244-4c68-9609-1de7b2ce9f20.png#averageHue=%23ebebd1&clientId=u6d020b59-3e2d-4&from=paste&height=767&id=u30b958d6&originHeight=767&originWidth=1023&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83824&status=done&style=none&taskId=u02c848ae-dbc5-4c77-b514-dfd748e4faa&title=&width=1023)
## 2.解压文件到指定目录
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705141104181-2b8a194d-f114-46ff-a0c5-138c08289599.png#averageHue=%23262424&clientId=u6d020b59-3e2d-4&from=paste&height=126&id=u56dd1b5f&originHeight=126&originWidth=1361&originalType=binary&ratio=1&rotation=0&showTitle=false&size=19196&status=done&style=none&taskId=u70db5b43-ba23-4206-bbc6-be1db131587&title=&width=1361)
## 3.创建mysql用户并授权
添加mysql用户组：`groupadd mysql`
添加mysql用户，同时指定mysql用户的初始组是mysql组：`useradd -g mysql mysql`
修改文件夹权限：`chown -R mysql:mysql mysql-8.0.35`
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705142234614-44c0e64d-23ba-4185-9ed2-de563374eb2c.png#averageHue=%232b2927&clientId=u6d020b59-3e2d-4&from=paste&height=462&id=ue13c94f9&originHeight=462&originWidth=961&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59706&status=done&style=none&taskId=uba0ffb90-4aff-447e-82db-9bde8c5c813&title=&width=961)
## 4.创建一个mysql数据存储目录
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705145216124-22ccc4e4-8d58-451a-ad4a-322c5d7f1f70.png#averageHue=%23282625&clientId=u6d020b59-3e2d-4&from=paste&height=212&id=u08f149c8&originHeight=212&originWidth=820&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18867&status=done&style=none&taskId=uaf496738-f9e4-4158-ad31-328d9d9d9dc&title=&width=820)
## 5.初始化数据库
`./bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql/ --datadir=/data/mysql`
### 该步骤出现的问题

1. 出现缺少libaio依赖

![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705143294942-227e8a38-087d-435a-8613-33e89cbc20ed.png#averageHue=%23292726&clientId=u6d020b59-3e2d-4&from=paste&height=101&id=u053c3de9&originHeight=101&originWidth=1851&originalType=binary&ratio=1&rotation=0&showTitle=false&size=22625&status=done&style=none&taskId=u77e8afd4-2dd9-4e05-a103-4b4149d0fc5&title=&width=1851)
安装libaio依赖
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705143472615-cc23c37a-1bf4-4afc-abab-52fe361a0e4f.png#averageHue=%23232222&clientId=u6d020b59-3e2d-4&from=paste&height=793&id=u285c4562&originHeight=793&originWidth=1403&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71870&status=done&style=none&taskId=ucd59a9ae-2e1a-46ff-9353-2170e62e4c8&title=&width=1403)

---

2. 缺少xxx文件

![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705143659428-cc9e64e1-1292-4e14-aba9-3c141dfc96d3.png#averageHue=%232b2927&clientId=u6d020b59-3e2d-4&from=paste&height=551&id=u4dc94671&originHeight=551&originWidth=1873&originalType=binary&ratio=1&rotation=0&showTitle=false&size=125769&status=done&style=none&taskId=ue90f62da-8548-4ca9-9b29-6fddfce3de9&title=&width=1873)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705146253271-36ac133b-4a00-4c45-88a4-da3b8ed55052.png#averageHue=%232a2726&clientId=ua49649c0-6d4c-4&from=paste&height=119&id=ua7b0bb8c&originHeight=119&originWidth=1195&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17953&status=done&style=none&taskId=ud4e24cf8-7396-4311-90eb-b1bbabcfb8a&title=&width=1195)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705157446843-e2ad118c-b7b4-4f4c-9eed-619b9e33970f.png#averageHue=%23232121&clientId=ua49649c0-6d4c-4&from=paste&height=371&id=u17898e57&originHeight=371&originWidth=1005&originalType=binary&ratio=1&rotation=0&showTitle=false&size=31582&status=done&style=none&taskId=u96f5b0d1-f67a-4cb7-8f39-b1690a2a59d&title=&width=1005)
下载新版libstdc++.so.6.0.26
```shell
unzip libstdc.so_.6.0.26.zip
# 将下载的最新版本拷贝到 /usr/lib64
cp libstdc++.so.6.0.26 /usr/lib64
cd  /usr/lib64
# 查看 /usr/lib64下libstdc++.so.6链接的版本
ls -l | grep libstdc++
# 删除原先的软连接(不放心可以备份)
rm libstdc++.so.6
# 使用最新的库建立软连接
ln -s libstdc++.so.6.0.26 libstdc++.so.6
# 查看新版本，成功
strings /usr/lib64/libstdc++.so.6 | grep GLIBCXX
```
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705223079740-c66d3982-45d0-4014-b9b5-1a9d5fda8d2e.png#averageHue=%232a2827&clientId=ua4132ded-0e83-4&from=paste&height=648&id=ube2b2e7d&originHeight=648&originWidth=1054&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57177&status=done&style=none&taskId=u7ac0a573-d266-46ba-be66-a750ef2f602&title=&width=1054)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705223447618-c718ed41-0bd3-42a1-8d03-b674c4ac6b0a.png#averageHue=%23383532&clientId=ua4132ded-0e83-4&from=paste&height=151&id=u3c512952&originHeight=151&originWidth=1309&originalType=binary&ratio=1&rotation=0&showTitle=false&size=23608&status=done&style=none&taskId=u44f12718-ca7b-4c20-aacb-394f441b675&title=&width=1309)
[OSError: /lib64/libm.so.6: version `GLIBC_2.27′ not found (required by xxx.so) ——升级GLIBC并解决系统错误](https://zhuanlan.zhihu.com/p/559791450)
安装好后，可正常启动。
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705225116576-e0ad211b-cedd-4f28-942e-2f33f4370fd9.png#averageHue=%2332302e&clientId=ua4132ded-0e83-4&from=paste&height=218&id=ufbf430c9&originHeight=218&originWidth=1722&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38692&status=done&style=none&taskId=u26bf6427-6c7d-4b97-8d01-dca7b34ad5d&title=&width=1722)
## 6.环境变量配置
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705230595615-921fe3fc-70cd-4e95-bc36-d4ea1e58003d.png#averageHue=%23252524&clientId=ua4132ded-0e83-4&from=paste&height=148&id=uc555b492&originHeight=148&originWidth=524&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8348&status=done&style=none&taskId=u8fb22dba-5f06-45cb-b5c8-6fc34d09097&title=&width=524)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/33516003/1705230527965-017dc4ec-0110-44e7-8d16-3a10f47999e2.png#averageHue=%23272626&clientId=ua4132ded-0e83-4&from=paste&height=477&id=u05ab31ac&originHeight=477&originWidth=861&originalType=binary&ratio=1&rotation=0&showTitle=false&size=33227&status=done&style=none&taskId=ud3d942c5-1989-40fe-a327-3137ea54e27&title=&width=861)
![image.png](https://save-826tt.obs.cn-east-3.myhuaweicloud.com/note_imges/202405171422659.png)

==最后需要在本地访问数据库，需要进行远程访问配置==
[[Mysql开启root远程登录]]
