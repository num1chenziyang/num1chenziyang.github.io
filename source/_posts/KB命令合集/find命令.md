---
title: find命令
date: 2025-04-23 15:40:25 
categories: 
- KB命令合集
- 
tags: 
password:
---
# 1. 基础文件搜索操作
## 1.1 按文件类型查找
### <1> 查找文本文件  
```bash
find . -name "*.txt"
```
• `find`：文件搜索命令

• `.`：起始搜索路径（当前目录）

• `-name`：按文件名匹配，区分大小写

• `"*.txt"`：通配符模式，匹配任意前缀的`.txt`文件

• 扩展：使用`-iname`进行不区分大小写搜索


### <2> 精确名称匹配  
```bash
find /path/to/search -name "example"
```
• `/path/to/search`：需替换为实际路径（如`/home/user/docs`）

• 支持多路径搜索：`find /path1 /path2 -name ...`

• 匹配模式：完全匹配"example"文件名


## 1.2 按时间属性查找
### <1> 历史文件检索  
```bash
find . -mtime +7
```
• `-mtime`：修改时间过滤（modification time）

• `+7`：7天前（不含第7天）

• 时间单位：

  • `+n`：超过n天

  • `n`：正好n天

  • `-n`：n天以内


### <2> 按访问时间检索  
```bash
find . -atime +30
```
• `-atime`：访问时间（access time）

• 适用场景：清理长期未使用的缓存文件


## 1.3 文件类型筛选
### <1> 目录检索  
```bash
find . -type d
```
• `-type`：文件类型筛选

• 类型参数：

  • `d`：目录（directory）

  • `f`：普通文件（file）

  • `l`：符号链接（symbolic link）


### <2> 二进制文件过滤  
```bash
find /usr/bin -type f -perm /111
```
• `-perm /111`：任意用户有执行权限

• 组合应用：`-type f -executable`


# 2. 高级搜索与批量操作
## 2.1 文件属性过滤
### <1> 容量限制搜索  
```bash
find . -size +1M
```
• 容量单位：

  • `c`：字节

  • `k`：KB（1024字节）

  • `M`：MB

  • `G`：GB

• 范围限定：

  • `+1M`：大于1MB

  • `-1M`：小于1MB

  • `1M`：等于1MB


### <2> 权限特征搜索  
```bash
find . \( -perm -u+x -o -perm -g+x \)
```
• `-perm`：精确权限匹配

• 权限模式：

  • `u+x`：用户具备执行权限

  • `g+x`：组具备执行权限

• `-o`：逻辑或操作符


## 2.2 用户/组权限管理
### <1> 所有权检索  
```bash
find . -user root -group www-data
```
• `-user`：文件所有者

• `-group`：所属用户组

• 组合应用：`-uid 1000 -gid 1000`


### <2> 权限回收操作  
```bash
find /var/www -type f -exec chmod 644 {} \;
```
• 典型应用：Web服务器文件权限标准化


# 3.自动化批量处理
## 3.1 文件内容操作
### <1> 全局字符串替换  
```bash
find directory -type f -exec sed -i 's/old/new/g' {} +
```
• 执行流程：

  1. 遍历指定目录所有文件
  2. 对每个文件执行sed替换命令
  3. `{}`：当前文件路径占位符
  4. `+`：批量处理优化参数

<2> 安全替换方案  
```bash
find . -name "*.conf" -exec sed -i.bak 's/old/new/g' {} \;
```
• `-i.bak`：创建备份文件（原文件加.bak后缀）

• 建议：重要操作前创建备份


3.2 统计与清理
<1> 代码行数统计  
```bash
find src/ -type f -exec cat {} + | wc -l
```
• 过滤空行增强版：

  ```bash
  find src/ -type f -exec grep -v '^$' {} + | wc -l
  ```

<2> 自动化清理系统  
```bash
find /var/log -name "*.log" -mtime +30 -exec rm -v {} \;
```
• `-v`：显示删除详情（verbose）

• 安全建议：先执行`-print`确认文件列表


2. 正则表达式深度应用
4.1 复杂模式匹配
```bash
find . -regextype posix-extended -regex ".*/[0-9]{4}-[0-9]{2}.log"
```
• `-regex`：正则表达式匹配

• 正则类型：

  • `posix-extended`：扩展正则

  • `emacs`：默认类型


4.2 排除式搜索
```bash
find . -type f ! -name "*.tmp" ! -size +5M
```
• `!`：逻辑非操作符

• 组合排除多种文件类型


二、文件格式规整技术手册
3. 行尾符处理
<1> Windows换行符清除  
```vim
:%s/\r//g
```
• 原理：删除CR（Carriage Return）字符

• 安全操作：`:%s/\r//gc`（逐项确认）


<2> 文件格式转换  
```vim
:set ff=unix
:w
```
• 格式类型：

  • `unix`：LF换行

  • `dos`：CRLF换行

• 查看当前格式：`:set ff?`


4. 空白行处理
<1> 纯空格行清理  
```vim
:%s/^\s*$//g
```
• 正则解析：

  • `^`：行首锚定

  • `\s`：空白字符（含空格/tab）

  • `*`：0个或多个匹配


<2> 保留空行策略  
```bash
find . -type f -exec sed -i '/^\s*$/d' {} +
```
• `/d`：删除匹配行

• 例外处理：保留含空白字符但非空行


5. 批量格式处理
<1> 目录级CRLF清除  
```bash
find /project/src -type f -exec sed -i 's/\r//g' {} +
```
• 文件类型限制：`-name "*.cpp"`


<2> 混合空白处理  
```bash
find . -name "*.md" -exec sed -i -e 's/\t/    /g' -e 's/\s\+$//g' {} +
```
• 功能组合：

  • 替换tab为4空格

  • 删除行尾空白


附：命令参数速查表

| 参数        | 功能描述                          | 示例                      |
|-------------|-----------------------------------|---------------------------|
| `-maxdepth` | 搜索深度限制                      | `-maxdepth 3`             |
| `-printf`   | 自定义输出格式                    | `-printf "%p\n"`          |
| `-execdir`  | 在文件所在目录执行命令            | `-execdir pwd \;`         |
| `-ok`       | 交互式确认执行                    | `-ok rm {} \;`            |
| `-newer`    | 比参照文件更新的文件              | `-newer reference.file`  |

> 操作安全建议：  
> 1. 危险操作前先执行`-print`确认文件列表  
> 2. 使用`-ok`替代`-exec`进行交互确认  
> 3. 重要数据操作前做好备份