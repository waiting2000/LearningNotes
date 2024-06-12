![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405271035939.png)
## 1.创建虚拟环境
conda create --name {env_name} python={version}
## 2.删除虚拟环境
conda remove --name {env_name} --all
## 3.复制虚拟环境
conda create --name {env_name_new} --clone {env_name_old}
## 4.激活虚拟环境
旧版：conda activate {env_name} 
[[conda activate激活环境失败]]
新版：source activate {env_name}
## 5.查看虚拟环境列表
conda env list 或 conda info -e
## 6.虚拟环境安包
可先激活虚拟环境后
conda install {package_name}
## 7.配置Anaconda镜像网址
conda config
conda config --set show_channel_urls yes
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
确保配置文件的格式如下：
```yaml
ssl_verify: true
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
show_channel_urls: true
```
查看当前镜像：
conda config --show channels
查看配置文件路径及镜像：
conda config --show-sources
