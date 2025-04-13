---
title: git项目管理工具
date: 2025-04-13 22:25:24
categories:
  - KB命令合集
  - 通用工具
tags: 
password:
---
# （一）git实战命令合集
## 1.clone代码
```shell
git clone 172.16.2.108/root/code.git
 
**git checkout develop**
 
git reset --hard oa18qw1f8qw1f59q1f98q
 
git clone https://username:password@github.com/org/project.git
```
当clone报错，使用带用户名密码的格式能解决


## 2.push改动
```shell
##修改user.name与user.email  
vim .git/config

或者命令配置如下

git config user.name "chenziyang"
git config user.email "chenziyang@gbase.cn"
git config user.name
git config user.email

git stn（git status -uno的缩写，自定义的）

git add gtest/src/CMakeLists.txt

git commit -m "add xxx function and gtest xxx,xxx,xxx"

git push origin code_351
```


## 3.切换到其他仓库的分支
```shell
##1.修改配置文件
cd .git
vim config
-------------------------
[remote "czy"]

url = [http://172.16.2.196/chenziyang/czy.git](http://172.16.2.196/chenziyang/czy.git)

fetch = +refs/head/*:refs/remotes/czy/*
-------------------------
git fetch czy
git br -a | grep v9.7.6**

##假设想要找到的分支为remotes/czy/release/v9.7.6，会搜到
git checkout remotes/czy/release/v9.7.6

##2.git remote命令
git remote add
```


## 4.stash贮藏
```shell
##当自己代码已经改动，但自己本地的分支不是最新的，需要将最新的分支信息拉下来，处理完冲突后，进行git push或Merge Request分支合并请求等操作.
git fetch

##将自己的改动保存下来，且起一个名字，仅起说明作用
git stash save chenziyang
git pull

##查看保存下来的改动的堆栈
git stash list

##取出想要的改动
git stash pop stash@{0}
git stash push -p file
git stash drop 堆栈
```


## 5.Merge Request 分支合并请求
```shell
##场景：我在ora_ver_merge_3.5.2分支开发，要将改动merge到ora_ver_merge_3.5.2分支上，提交Merge Request 分支合并请求，在主管审批完后将我的改动merge到上述分支。
git add xxx
git commit -m xxx

##因我在ora_ver_merge_3.5.2分支上开发，要向ora_ver_merge_3.5.2分支merge。
##故要创建新分支，以使用merge操作。
##如果本身就在如:'ora_ver_merge_3.5.2_czy'即另一个分支，则无需创建新分支。
git co -b ora_ver_merge_3.5.2_czy
git push origin 分支名

##若自己的远程分支不要了记得删除
git push origin --delete <branch>

##若想覆盖掉同名的远程分支
git push -f origin <branch>

##Merge Requests
##New merge request
##Compare branches and continue
Source branch选自己有修改的新分支，Target branch选自己想合并的新分支。
##Submit merge request
Assignee选审阅人
```


## 6.git rebase -i
```shell
##合并自己本地的一些提交，然后推到远端自己的分支
git stn
git add xxx
git commit -s

##将要合并的提交的pick改为s，提交说明注掉  
git rebase -i HEAD~2

##注意，push之前看一下自己的信息是否正确  
git push origin ora_ver_merge_3.5.0_syn_czy
```


## 7.git rebase --onto
```shell
##合并次要分支上的某些提交到主要分支上
git rebase --onto 主要分支 次要分支的目标提交的上一个提交 次要分支的目标提交
（如：git rebase --onto release/xianggang rfw35v456wv sz86d4frw48）

##此时位于断连分支，需要删除本地的主要分支，创建同名分支，则此断连分支变成主要分支  
##删除本地分支
git branch -d <branch_name>
（git branch -D <branch_name> ##强制删除本地支)

##创建新分支
git checkout -b new_branch

##检查无误后将本分支推至远端
git push origin new_branch
```


## 8.创建与删除分支
```shell
##创建本地分支
git co -b new_branch
 
##创建远程分支
git push origin new_branch
 
##删除本地分支
git branch -d <branch_name>
 
##强制删除本地分支
git branch -D <branch_name>
 
##删除远程分支
git push origin --delete <branch_name>
```


## 9.commit_hash
```shell
##git中查看特定文件与某次提交的差异
git diff commit_hash <file_path>
 
##git中让特定文件恢复到某一次提交的命令
git checkout commit_hash -- <file_path>
 
##比较两个指定提交之间特定文件的差异
git diff <commit_hash1> <commit_hash2> -- <filename>
```


## 10.reset与revert
```shell
##清除所有未提交的更改，无论是暂存还是未暂存的
git reset --hard
 
##把此次提交的更改保留在工作目录和暂存区，可以继续编辑
git reset --soft
 
##取消add
git reset <commit_hash> <file_path>
 
##取消commit
git revert <commit_hash>
```


## 11.cherry-pick
### 一.基础用法
#### 1.合并单个提交
```shell
git cherry-pick <commit-hash1>
```

#### 2.合并多个提交
```shell
git cherry-pick <commit-hash1> <commit-hash2> <commit-hash3>
```
note：虽然有合并多个提交的其他方法，但是为了安全，还是逐个提交比较好。

### 二.冲突处理
```shell
##如果应用的提交与目标分支上的现有文件有冲突，cherry-pick 会暂停并需要手动解决冲突。
##解决冲突后，继续过程。
git cherry-pick --continue

##取消cherry-pick
git cherry-pick --abort
```

### 三.跨库cherry-pick
```shell
##本地添加另一个仓库A
git remote add gerrit http://git.xxxx.com/A.git

##通过git remote -v可以查看是否添加成功
origin http://git.xxxx.com/B.git (fetch)
origin http://git.xxxx.com/B.git (push)
gerrit http://git.xxxx.com/A.git (fetch)
gerrit http://git.xxxx.com/A.git (push)

##把远程A库的分支信息同步到本地
git fetch gerrit
##这时候就可以cherry-pick了
```


## 12.git show
```shell
##本提交与目标提交的差异
git show 目标提交号
 
##本提交与目标提交的差异，仅显示文件名
git show --name-only 目标提交号
```


## 13.查看远端分支的提交
```shell
##查看本地与远端所有分支，可以用grep筛选自己想看的远端分支全名
git branch -a
git branch --all
git branch -a | grep release/v3.6.1

##查看远端提交
git log remotes/origin/release/v3.6.1
```


## 14.修改已 commit 的信息
### 1.修改最近一次的提交
```shell
git commit --amend
```

### 2.修改某次提交或者多个提交
```shell
git rebase -i HEAD~8
##要修改的提交，pick 改为 edit 或 e

##修改详细信息
git commit --amend

##继续rebase
git rebase --continue
```

### 3.修正最近一次提交的作者信息
```shell
##请将 "Author Name" 和 <email@example.com> 替换成你希望显示的名字和邮箱地址
git commit --amend --author="Author Name <email@example.com>"
```
note：  
和17.增加 Change_Id相似，一块看以明晰原理。


## 15.远程仓库
```shell
##列出所有配置的远程仓库及其URL。
##-v 标志表示"verbose"，意味着输出包含推送（push）和拉取（fetch）的URL。
git remote -v

##这个命令提供了更多关于名为 origin 的远程仓库的详细信息。
##它会显示远程仓库的URL、引用（ref）的列表、上次更新的时间、以及你与远程仓库同步的本地分支。
git remote show origin
```


## 16.创建tag
```shell
##git tag -a {标签名} -m "{标签信息}"  
##例如，创建一个指向最新提交的附注标签：
git tag -a ver/3.3.3_1SANSEC_2/vbr1e4_X86_64 -m "Release version 1.0.0"

git push origin ver/3.3.3_1SANSEC_2/vbr1e4_X86_64
```


## 17.增加 Change_Id
```shell
##在提交到gerrit上的时候，每个提交都需要有change_id，否则提交失败。
##在code目录安装commit-msg hook工具，每次提交都给予change_id。
git rebase -i HEAD~2

edit abcdef1 First commit message
edit 1234567 Second commit message

git commit --amend
git rebase --continue
```

------------------------------------------------------------------

# 其他问题
## 1.特殊命令记录
```shell
##列出当前仓库中的所有本地分支。  
##对于每个分支，显示其最后一次提交的哈希值和提交信息。  
##显示每个本地分支与其对应的远程跟踪分支的关联关系，以及它们之间的同步状态（如有）。
git branch -vv

##修改最近一次提交（未push）  
##Git会打开默认的文本编辑器，显示最近一次提交的提交信息。在编辑器中修改提交信息为你想要的新信息，保存并关闭编辑器。
git commit --amend
```


## 2.git reset --hard commitid后想要撤销：
```text
（前提条件：丢失的分支或commit信息还没有被git gc清除）  
1.执行git log -g 或者 git reflog show。  
2.找到执行reset --hard之前的commit对应的commitid（可以通过日期和时间来辨别）。  
3.通过git branch recover_branch commitid来建立新分支并撤销了回滚（可不操作）。  
4.使用命令git reset --hard commitid可强制回滚到之前的版本。
```


## 3.git没有颜色的问题
```shell
##以全局启用Git命令行输出的颜色。
git config --global color.ui true

##某个特定的Git仓库中启用颜色，在该仓库目录下运行此命令。
git config color.ui true
```


---
