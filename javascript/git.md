# 将本地仓库中的文件上传到对应的远程仓库 #

## 提交代码需要经历的三个区 ##

1. 工作区 本地文件夹
2. 暂存区 缓存区域
3. 历史区 针对本次提交，生成一个版本记录

## 提交一个版本 ##

- git status 查看当前仓库状态
- git add . 将当前工作区文件添加到暂存区
- git status 
	- 红色 说明当前文件在工作区
	- 绿色 说明当前文件在暂存区
- git commit -m '描述' 将暂存区文件 全部添加到历史区
- git push origin master 将历史区文件 上传到远程仓库（master是主干的意思）origin 对应的远程仓库的名称
- 输入github用户名 密码

## 将本地修改上传到远程仓库 ##

- 修改文件
- git add .
- git commit -m '备注'
- git push origin master
- push后有的需要输入用户名和密码

## 使用git仓库（clone） ##

- git clone 远程仓库地址
- git pull origin master 拉取别人的代码


## 本地已有文件，变成git仓库，并且和远程指定仓库建立关联 ##

- git init 把本地已有文件夹初始化成git仓库
- git remote add origin 远程仓库地址
- git remote -v 查看关联地址

## 多人协作
- git remote add origin xxx.git 给本地关联一个远程仓库
- git push -u origin master 将本地master推送到远程库，并制定push命令的默认远程库
### 在本地创建开发分支dev,并推送到远程仓库中，所有人都在dev分支上提交代码
   - git branch dev
   - git push origin dev
   开发人员在本地创建一个远程库dev分支的副本dary
   - git pull 更新
   - git checkout -b dary origin/dev 在远程origin/dev分支的基础上创建本地dev分支
   - git branch --set-upstream-to dary origin/dev 将本地dary分支与远程分支关联起来
   

# 常用linux命令 #

1. mkdir 文件命令  创建文件夹
2. touch 文件名.扩展名 创建文件
3. cat 文件名 查看文件内容
4. vim 文件名 编辑文件   i 修改； esc 退出修改状态；:w 保存； ：q 退出； ：wq 保存并退出
5. cd 目录名 切换目录
6. pwd 查看当前所在路径
7. ls -al 查看所有文件包含的隐藏文件
8. rm -rf 文件或文件名 强制删除文件
9. find -name 文件名 根据文件名查找文件
10. clear 清屏
