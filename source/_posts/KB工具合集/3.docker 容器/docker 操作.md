# 一.docker常用命令

==安装Docker==  
==yum install -y docker====￼====systemctl start docker====￼====systemctl== ==enable== ==docker==
 
==拉取镜像==  
==docker pull [image_name]==
 
==查看本地镜像==  
==docker images==
 
==运行容器==  
==docker run -it [image_name] [====command====]==
 
==列出容器==  
==docker ps====￼====docker ps -a==
 
==停止/启动/重启容器==  
==docker stop [container_id_or_name]====￼====docker start [container_id_or_name]====￼====docker restart [container_id_or_name]==
 
==删除容器==  
==docker rm [container_id_or_name]====￼====docker rm $(docker ps -a -q)==
 
==删除镜像==  
==docker rmi [image_id]====￼====docker rmi $(docker images -f== =="dangling=true"== ==-q)==
 
==构建镜像==  
==docker build -t [image_name] .==
 
==推送镜像到Docker Hub==  
==docker push [username]/[repository]:[tag]==
 
==查看容器日志==  
==docker logs [container_id_or_name]==
    
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