**问题****:****老是关闭防火墙太麻烦，所以选择彻底关闭防火墙，发现每次都记不住命令****!**  
**下面是****redhat/CentOs7****关闭防火墙的命令****!**
   

1:查看防火状态
 
systemctl status firewalld
 
service iptables status
 
2:暂时关闭防火墙
 
systemctl stop firewalld
 
service iptables stop
 
3:永久关闭防火墙
 
systemctl disable firewalld
 
chkconfig iptables off
 
4:重启防火墙
 
systemctl enable firewalld
 
service iptables restart
 
5:永久关闭后重启
 
//暂时还没有试过
 
chkconfig iptables on　  
————————————————
 
版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。   原文链接：https://blog.csdn.net/qq_35971258/article/details/79318842