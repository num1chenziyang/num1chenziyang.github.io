service 和 systemctl 命令都是用来管理和控制系统服务的工具，但它们分别适用于不同的 Linux 发行版和服务管理机制。
 
# 一.service 命令

==service== ==命令主要用于== ==SysVinit== ==系统的服务管理。====SysVinit== ==是传统的== ==Linux== ==初始化系统，许多早期的== ==Linux== ==发行版（如== ==Debian== ==和== ==Red Hat====）使用这种方法来启动服务。==

## 1.基本语法

## service SERVICE_NAME COMMAND

==常见选项==

- ==start====：启动服务。==
- ==stop====：停止服务。==
- ==restart====：重启服务（先停止再启动）。==
- ==status====：检查服务状态。==
- ==reload====：重新加载服务配置（如果支持）。==
- ==force-reload====：强制重新加载服务配置。==
 
## 2.示例

==假设你想要管理== ==Apache Web== ==服务器（通常是== ==httpd== ==或== ==apache2====）：==  
==service httpd start====￼====service httpd stop====￼====service httpd restart====￼====service httpd status====￼====service httpd reload====￼====service httpd force-reload==
    
# 二.systemctl 命令

==systemctl== ==命令是== ==Systemd== ==系统的服务管理工具。====Systemd== ==是一个更现代的初始化系统，已经被许多现代== ==Linux== ==发行版采用，如== ==Fedora====、====CentOS 7+====、====RHEL 7+== ==和== ==Debian 9+====。==

## 1.基本语法

## systemctl COMMAND UNIT_NAME

==常见选项==

- ==start====：启动服务。==
- ==stop====：停止服务。==
- ==restart====：重启服务（先停止再启动）。==
- ==status====：检查服务状态。==
- ==enable/disable====：使能或禁用服务，使其在系统启动时自动启动或不自动启动。==
- ==reload-or-restart====：重新加载配置文件（如果支持）或重启服务。==
- ==is-enabled/is-active/is-failed====：检查服务是否已使能、是否处于活动状态或是否失败。==
 
## 2.示例

==假设你想要管理== ==MySQL== ==数据库服务（通常是== ==mysqld====）：==  
==systemctl start mysqld====￼====systemctl stop mysqld====￼====systemctl restart mysqld====￼====systemctl status mysqld====￼====systemctl enable mysqld====￼====systemctl disable mysqld====￼====systemctl reload-or-restart mysqld====￼====systemctl is-enabled mysqld====￼====systemctl is-active mysqld====￼====systemctl is-failed mysqld==
    
# 三.对比

- ==SysVinit====：==
    
    - ==较为传统，支持较早的== ==Linux== ==发行版。==
    - ==依赖于== ==/etc/init.d== ==脚本。==
    - ==通常使用== ==service== ==命令来管理服务。==
- ==Systemd====：==
    
    - ==更加现代化，支持并发启动和服务依赖关系。==
    - ==使用== ==.service== ==文件来定义服务。==
    - ==通常使用== ==systemctl== ==命令来管理服务。==

## 1.示例

**SysVinit** **示例**  
**service ssh start****￼****service ssh stop****￼****service ssh restart****￼****service ssh status**
 
**Systemd** **示例**  
**systemctl start sshd****￼****systemctl stop sshd****￼****systemctl restart sshd****￼****systemctl status sshd**
 
## 2.选择

- ==如果你的系统使用== ==SysVinit== ==初始化系统（如一些较旧的== ==Debian== ==或== ==Red Hat== ==版本），你应该使用== ==service== ==命令。==
- ==如果你的系统使用== ==Systemd== ==初始化系统（如大多数现代== ==Linux== ==发行版），你应该使用== ==systemctl== ==命令。==