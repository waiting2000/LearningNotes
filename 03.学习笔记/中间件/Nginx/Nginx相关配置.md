---
创建时间: 2024-05-27
---
# 1.Nginx基本命令
```shell
#检查配置文件
nginx -t
#热加载，重新加载配置文件
nginx -s reload
#直接关闭
nginx -s stop
#等待工作进程处理完成后关闭
nginx -s quit
```
# 2.Nginx访问日志配置
日志配置注释了并不代表关闭日志输出，而是输出至默认位置<br />![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405271409365.png)
### 2.1.日志输出格式
`log_format [格式名] '格式内容'`

| 参数 | 说明 |
| --- | --- |
| $remote_addr | 客户端地址 |
| $remote_user | 客户端用户名称 |
| $time_local | 访问时间和时区 |
| $request | 请求的URI和HTTP协议 |
| $http_host | 请求地址，即浏览器中你输入的地址（IP或域名） |
| $status | HTTP请求状态 |
| $upstream_status | upstream状态 |
| $body_bytes_sent | 发送给客户端文件内容大小 |
| $http_referer | url跳转来源 |
| $http_user_agent | 用户终端浏览器等信息 |
| $ssl_protocol | SSL协议版本 |
| $ssl_cipher | 交换数据中的算法 |
| $upstream_addr | 后台upstream的地址，即真正提供服务的主机地址 |
| $request_time | 整个请求的总时间 |
| $upstream_response_time | 请求过程中，upstream响应时间 |

**在全局的http块中配置了日志路径，会对全局生效；但是若在server块里也配置了路径，则会针对具体情况进行拆分，独立记录日志情况。**<br />![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405271409205.png)<br />![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202405271409863.png)
