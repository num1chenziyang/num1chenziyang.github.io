---
title: samba文件共享工具
date: 2025-04-20 15:56:02 
categories: 
- KB命令合集
- 
tags: 
password:
---
以下是使用 Samba 将 Linux 目录共享到 Windows 的详细步骤，包含关键参数说明和注意事项：

---

**一、环境准备**
1. 系统要求  
   • Linux 系统（以 CentOS 7/8 或 Ubuntu 20.04+ 为例）  

   • Windows 10/11（需与 Linux 处于同一局域网）


2. 安装 Samba  
   ```bash
   # CentOS/RHEL
   sudo yum install samba samba-client samba-common -y

   # Ubuntu/Debian
   sudo apt update
   sudo apt install samba samba-common-bin smbclient -y
   ```
   > 安装后会生成默认配置文件 `/etc/samba/smb.conf`

---

**二、创建共享目录**
1. 新建目录并设置权限  
   ```bash
   sudo mkdir -p /srv/samba_share  # 创建共享根目录
   sudo chmod -R 2775 /srv/samba_share  # 设置 SGID 权限继承
   sudo chown -R nobody:nogroup /srv/samba_share  # 分配匿名用户权限（可选）
   ```
   > 若需用户级权限控制，建议用 `chown username:groupname` 替代

2. SELinux 配置（仅限 CentOS）  
   ```bash
   sudo setsebool -P samba_export_all_rw=1  # 允许 Samba 写入共享目录
   sudo semanage fcontext -a -t samba_share_t "/srv/samba_share(/.*)?"  # 设置安全上下文
   sudo restorecon -Rv /srv/samba_share  # 刷新安全策略
   ```

---

**三、配置 Samba**
1. 备份原始配置文件  
   ```bash
   sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
   ```

2. 编辑主配置文件  
   ```bash
   sudo vim /etc/samba/smb.conf
   ```
   添加以下内容到文件末尾：
   ```ini
   [global]
   workgroup = WORKGROUP  # 必须与 Windows 工作组同名
   security = user        # 启用用户认证模式
   map to guest = Bad User
   log file = /var/log/samba/log.%m  # 日志文件路径
   max log size = 1000    # 日志文件大小限制（MB）

   [Linux_Share]          # 共享名称（Windows 端显示的名称）
   comment = Samba Shared Folder  # 共享描述（可选）
   path = /srv/samba_share  # 实际共享目录路径
   browseable = yes       # 允许浏览共享列表
   writable = yes         # 允许写入
   valid users = @smbusers  # 允许访问的用户组（需提前创建）
   create mask = 0664     # 新建文件权限
   directory mask = 0775  # 新建目录权限
```

关键参数说明：  
- `valid users`：可指定用户列表（如 `user1,user2`）或用户组（`@groupname`）  
- `force user`/`force group`：强制文件归属（适用于匿名共享场景）

3. 检查配置语法  
   ```bash
   testparm -s  # 验证配置文件语法
   ```

---

**四、用户管理**
4. 创建系统用户  
   ```bash
   sudo useradd sambauser -s /sbin/nologin  # 创建无 shell 用户
   sudo passwd sambauser  # 设置系统密码（非 Samba 密码）
   ```

5. 添加 Samba 用户  
   ```bash
   sudo smbpasswd -a sambauser  # 将系统用户添加到 Samba
   ```
   > 输入两次密码（需与系统密码不同）

6. 管理用户命令  
   ```bash
   sudo pdbedit -L  # 查看所有 Samba 用户
   sudo smbpasswd -x sambauser  # 删除用户
   ```

---

**五、防火墙与权限**
7. 防火墙配置  
   ```bash
   # CentOS
   sudo firewall-cmd --permanent --add-service=samba
   sudo firewall-cmd --reload

   # Ubuntu
   sudo ufw allow samba
   ```

8. 重启服务  
   ```bash
   # CentOS
   sudo systemctl enable --now smb nmb
   sudo systemctl restart smb nmb

   # Ubuntu
   sudo systemctl restart smbd nmbd
   ```

---

**六、Windows 端连接**
9. 通过资源管理器访问  
   • 打开 `此电脑` → 地址栏输入 `\\Linux_IP`（如 `\\192.168.1.100`）  

   • 输入 Samba 用户名和密码（如 `sambauser` 及其密码）


10. 映射网络驱动器（永久挂载）  
   • 右键 `此电脑` → `映射网络驱动器`  

   • 选择驱动器号，输入路径 `\\Linux_IP\Linux_Share`  

   • 勾选 `重新连接时重新连接` → 输入凭证完成映射


11. CMD 命令行测试  
   ```cmd
   net view \\Linux_IP  # 查看共享列表
   net use Z: \\Linux_IP\Linux_Share /user:sambauser  # 映射到 Z 盘
   ```

---

**七、故障排查**
12. 常见问题解决  
   • 连接超时：检查防火墙、Samba 服务状态 `systemctl status smb`  

   • 权限拒绝：确认共享目录本地权限 `ls -ld /srv/samba_share`  

   • 密码错误：重置 Samba 密码 `smbpasswd sambauser`


13. 查看日志  
   ```bash
   tail -f /var/log/samba/log.192.168.1.100  # 实时监控访问日志
   ```

---

通过以上步骤可实现 Linux 与 Windows 的稳定文件共享。如需更复杂的权限控制（如不同用户组访问不同目录），可参考 `/etc/samba/smb.conf` 中多共享段的配置方式。