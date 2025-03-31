[使用](https://blog.csdn.net/zhvngchvng/article/details/107856401)qemu搭建arm嵌入式linux开发环境_qemu-system-arm-CSDN博客
   

qemu-system-arm \  
-machine raspi3 \  
-cpu cortex-a7 \  
-m 1024M \  
-nographic \  
-kernel path/to/vmlinuz \  
-initrd path/to/initrd.img \  
-dtb path/to/your-dtb-file.dtbo \  
-drive if=sd,format=raw,file=~/Downloads/ubuntu-20.04-preinstalled-server-armhf+raspi.img \  
-netdev user,id=net0,hostfwd=tcp::2222-:22 \  
-device virtio-net-pci,netdev=net0