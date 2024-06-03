## 具体报错：
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405271037904.png)
## 问题原因：
最新版anaconda3移除了activate命令激活环境可以用source activate环境名
## 解决方案：
`source /usr/local/anaconda3/etc/profile.d/conda.sh
像是一次性指令。
