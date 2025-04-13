---
title: docker容器
date: 2025-04-13 22:25:53
categories:
  - KB命令合集
  - 通用工具
tags: 
password:
---
1.（实战）浪潮-欧拉-高可用通讯加密验证

docker常用操作

# 二.docker实战操作

## 1.172.16.3.168（arm64 平台）

**docker container ls -a**  
##用于列出所有容器，包括正在运行的和已停止的容器
 
**docker** **start na7sd8quhd8**  
##启动docker
 
**docker exec -it centos7_arm64 /bin/bash**  
##进入一个已经存在的容器内部，并通过终端与之进行交互。  
##==这条命令各部分的意义如下：==

- ==docker exec====: Docker的exec子命令用来在现有容器内开始一个新进程。==
- ==-it====: 这两个选项一起使用表示分配一个伪输入终端（-i），并且让容器的标准输入保持打开（-t）。这使得你可以与容器内的进程进行交互，就像你在直接控制一台服务器一样。==
- ==centos7_arm64====: 这是指定要进入哪个容器。这里是一个名为“centos7_arm64”的容器。==
- ==/bin/bash====: 这是在容器内要启动的新进程。在这里，它是bash shell，这意味着你会得到一个交互式的bash shell环境。==
