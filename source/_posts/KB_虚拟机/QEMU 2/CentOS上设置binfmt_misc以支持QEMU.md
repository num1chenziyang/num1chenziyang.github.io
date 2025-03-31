##先一起安装上qemu与qemu-user-static  
sudo yum install epel-release  
sudo yum install qemu qemu-user-static
   

##将binfmt_misc内核模块加载  
##加载binfmt_misc内核模块的作用是在执行arm32架构可执行程序时  
sudo modprobe binfmt_misc
    
qemu-arm -L /path/to/arm/rootfs demo