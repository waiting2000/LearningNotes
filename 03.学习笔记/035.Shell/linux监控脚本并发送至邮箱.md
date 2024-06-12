```shell
#!/bin/bash
# 获取的磁盘数据
# 磁盘总量
total_d=`df -h|grep "/$"|awk -F " " '{print $2}'`
# 磁盘已使用
use_d=`df -h|grep "/$"|awk -F " " '{print $3}'`
# 磁盘剩余
left_d=`df -h|grep "/$"|awk -F " " '{print $4}'`
# 磁盘使用率
pre_d=`df -h|grep "/$"|awk -F " " '{print $5}'`
# 磁盘的警告
mess=`echo "磁盘使用率超过40%，磁盘总大小为:${total_d},已用：${use_d},剩余${left_d},使用率为${pre_d}"`
# 内存信息
total_m=`free -m|grep Mem|awk -F " " '{print $2}'`
avail_m=`free -m|grep Mem|awk -F " " '{print $7}'`
pre=` echo "($total_m-$avail_m)*100/$total_m"|bc `
pre_m=` echo ${pre%.*} `
# 内存的警告
mess2=` echo "内存的使用率超过20%" `
# cpu
cpu_c=` sar 1 1|tail -1|awk '{print$NF}'|cut -d '.' -f1 `
mess3=` echo "cpu使用率超过30%" `

num=""
if [[ $a -gt 40 ]];then
    num=$mess
else
    echo "磁盘使用正常"
fi
if [[ $pre_m -gt 20 ]];then
    num="$mess2,$num"
else
    echo "内存使用正常"
fi
if [[ $cpu_c -lt 30 ]];then
    num="$mess3,$num"
else
    echo "cpu使用正常"
fi

echo $num |mail -s "warning" 1195306400@qq.com

```
