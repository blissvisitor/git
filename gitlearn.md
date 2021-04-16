# GIT常用命令

[TOC]



### https://git-scm.com //git官网

http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000  //廖雪峰git教程地址

## 创建版本库

```
1.cd git    //定位到指定路径

2.git init  //新建版本库

//版本分支提交

3.git add .  git add readme.txt //工作区存到暂存区

4.git commit -m "提交备注"      //暂存区所有文件提交到仓库

 5.git status //查看当前仓库状态

6.git diff readme.txt //查看文件修改内容

7.git log //仓库修改日志

 8.git reflog //查看所有操作日志
```



## 版本回退

```
 9.git reset --hard HEAD^ //回退到上一个版本 --  git reset --hard HEAD^^ //上上个版本  -- git reset --hard HEEAD~100  //回退到上100个版本

 10.git reset --hard 版本号 //回退到指定版本

 11.git cat readme.txt //查看文件内容

git clean -df  //清空所有本地工作区的修改、和删除添加的的文件
```



## 撤销修改

```
 12.git checkout --readme.txt  //工作区撤销到上一步
 13.git reset HEAD readme.txt  //暂存区文件回退到工作区  如果提交到版本库了，可以回退到上个版本（如9）
```



## 删除文件

14. ### rm readme.txt  //删除文件或直接找到文件删除  注：commit后版本库中已删除，否则可以恢复删除的文件到最新版本，但最近提交的一次修改会丢失（9）

    ## 远程仓库

    ### 本地注册github账户

    git config --global user.name "yourname"
    git config --global user.email“your@email.com"
15. ### ssh-keygen -t rsa -C "your email address"  //创建SHH并保存，登录github，setting中add shh key拷贝id_rsa.pub中所有内容到key栏，添加完成
16. ### git remote add origin git@github.com:github帐户名/本地仓库名.git  //github上新建版本库，把本地已有版本库与之关联
17. ### git push -u origin master //本地版本库推送到远程库上，初次推送加-u 以后不用
18. ### github ssh第一次推送时，会确认本地key是否添加到github信任列表中了，yes即可
19. ### git clone 远程库地址  //克隆远程库到本地，有多种协议除原生ssh外还有http（速度慢，每次推送输入口令）

    ## //分支管理

20. ### git checkout -b dev //创建并切换到改分支
21. ### git branch dev //创建分支
22. ### git checkout dev //切换分支
23. ### git branch //查看所有分支 *为当前分支
24. ### git merge dev //合并指定分支到当前分支
25. ### git branch -d dev //删除分支  git branch -D feature  //强行删除分支feature

    //解决冲突 多个分支提交同一个文件修改，合并时发生冲突，不能快速合并
    //会提示CONFLICT，修改提示的各个分支中的部分，再提交    当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
    26.$ git log --graph --pretty=oneline --abbrev-commit //查看冲突合并情况 删除一个分支即可
27. ### git merge --no-ff -m "merge with no-ff" dev //合并dev分支到当前分支，禁用fast forwad
27. ### git log --graphy --pretty=oneline --abbrev-commit //查看分支合并日志

    //在实际开发中，我们应该按照几个基本原则进行分支管理：
    //首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
    //那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
    //你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
    //bug解决

### 29 git stash  //储存当前分支工作现场

```
$ git checkout master //切换到修改bug的分支
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
$ git checkout -b issue-101  //创建bug分支，进行修改
Switched to a new branch 'issue-101'
//修改bug，提交 
$ git add readme.txt 
$ git commit -m "fix bug 101"
[issue-101 cc17032] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
//切换回分支mastrer 合并到master，删除bug分支 提交
 $ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 2 commits.
$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt |    2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git branch -d issue-101
Deleted branch issue-101 (was cc17032).
//切换回原来工作的分支
$ git checkout dev
Switched to branch 'dev'
$ git status
```



```
On branch dev
nothing to commit (working directory clean)、
//工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：
$ git stash list
stash@{0}: WIP on dev: 6224937 add merge
//工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：
//一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
//另一种方式是用git stash pop，恢复的同时把stash内容也删了：
$ git stash pop
```



```
# On branch dev
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#       new file:   hello.py
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)

#   (use "git checkout -- <file>..." to discard changes in working directory)

#       modified:   readme.txt
```

```
Dropped refs/stash@{0} (f624f8e5f082f2df2bed8a4e09c12fd2943bdd40)
//再用git stash list查看，就看不到任何stash内容了：
$ git stash list
//你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
$ git stash apply stash@{0}
```




## 多人协作

查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

## 标签

```shell
git tag tagname //新建tag
git tag -a tagname  -m "info" //创建有说明的tag
git tag //查看所有tag
git tag tagname commitid //指定commit id 赋值tag
git show tagname //查看tag说明

git tag -s tagname -m "info"  //私匙签名一个标签
签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错：
gpg: signing failed: secret key not available
error: gpg failed to sign the data
error: unable to sign the tag
--如果报错，请参考GnuPG帮助文档配置Key。
用命令git show <tagname>可以看到PGP签名信息：
用PGP签名的标签是不可伪造的，因为可以验证PGP签名。验证签名的方法比较复杂，这里就不介绍了。
git tag -d tagname //删除tag
git push origin tagname  //推送标签到远程
git push origin --tag  //推送所有tag
//tag已经推送，删除
git tag -d tagname //删除本地 git push origin :refs/tags/tagname  //删除远程
```



## 使用github


    在GitHub上，可以任意Fork开源仓库；
    
    自己拥有Fork后的仓库的读写权限；
    
    可以推送pull request给官方仓库来贡献代码。
## 忽略特殊文件


    忽略某些文件时，需要编写.gitignore；
    
    .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！
## 配置git

给Git配置好别名，就可以输入命令时偷个懒。我们鼓励偷懒。
搭建Git服务器  linux系统搭建，搜索学习


    搭建Git服务器非常简单，通常10分钟即可完成；
    
    要方便管理公钥，用Gitosis；
    
    要像SVN那样变态地控制权限，用Gitolite。
