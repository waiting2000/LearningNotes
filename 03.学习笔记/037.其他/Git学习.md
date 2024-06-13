---
创建时间: 2024-06-03
tags:
  - 个人
---
# Git配置
# Git基本操作
# Git工作原理
>https://www.runoob.com/git/git-workspace-index-repo.html
![image.png](https://saveattachment.oss-cn-shanghai.aliyuncs.com/saveImages/202406071055327.png)
# Git操作
## git clone
>从git服务器拉取代码

```shell
git clone https://github.com/waiting2000/LearningNotes.git
```

## git config
>查看git配置

```shell
git config --global user.name 
git config --global user.email
```
## git branch
>创建、重命名、查看、删除项目分支，通过 `Git` 做项目开发时，一般都是在开发分支中进行，开发完成后合并分支到主干。

```shell
git branch main
```
## git checkout
>切换分支

```shell
git checkout master
```
## git status
>查看文件变动状态

```shell

```
## git add
>添加文件变动到暂存区
## git commit
>提交文件变动到版本库

```shell
git commit -m '提交备注'
```
## git push
>将本地的代码改动推送到服务器

```shell
git push <远程主机名> <本地分支名>:<远程分支名>
```
## git pull
>将服务器上的最新代码拉取到本地

```shell
git pull origin main
```
## git log
>查看版本提交记录
## git tag
>为项目标记里程碑



