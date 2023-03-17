# **git使用总结**

2023.3.17

## **1.初始化一个仓库**：

```sh
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```

然后提交文件

```sh
$ git add .#提交所有文件
$ git commit -m "wrote a readme file"
```

创建和切换分支，详情见[**分支管理**](https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424) 

```sh
$ git checkout -b dev
$ git checkout master
```

## **2.提交到网络仓库**：

```sh
cd existing_repo#自己的目录
git remote add origin http://git.hhqj.com/zjy/vim3fairmot.git#在网络仓库界面复制
git branch -M main#更改分支名字，可不改
git push -uf origin main
#对于git.hhqj.com 用户名为zjy 
$ git push origin master#第一次提交之后直接提交就好
$ git remote -v#查看现有远程库
$ git remote rm XXX#deleter
```

对于ssh的添加详情请见[远程库添加](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416) 如果发生提交失败的情况显示权限问题，在网页关闭分支保护即可
