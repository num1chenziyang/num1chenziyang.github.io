---
title: gdb调试工具
date: 2025-04-14 23:08:53
categories:
  - KB命令合集
  - linux工具
tags: 
password:
---
常用方法

1.x /100c 变量名  

2.

# 一.gdb 调试

gdb oninit  
set args -vy  
b main  
run
    
# 二.打印调试

在 main 中添加 printf，运行 oninit 时会显示打印信息
    
# 三.子进程调试

gdb oninit
 
b fork_processor
 
r -divy
 
p newvp->classid  
##classid=6，这代表
 
set follow-fork-mode child
 
c  
##此时会产生子进程  
##New process 7778
 
handle SIGUSR2 nostop noprint



---

onstat -g glo  
发现加密VP是encrypt，调试其pid:16166  
gdb attach 16166  
b openssl_init_ctx  
c
 
465