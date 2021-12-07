# Windows操作系统基础

## 系统常用命令

### 用户组管理

+ 添加一个永不过期的用户，并且设置登录口令

  net user [userName] [password] /add /expires:never

+ 删除用户

  net user [userName] /delete

+ 将用户添加到管理员组

  net localgroup Administrators [userName] /add

+ 将用户从管理员组删除

  net localgroup Administrators [userName] /delete

+ 修改当前用户的密码

  net user [userName] [password]

+ 激活用户

  net user [userName] /active:yes

+ 禁用用户

  net user [userName] /active:no

+ 查看用户

  net user

  账户被隐藏，无法通过net user命令查看。但可在用户列表中进行查看。

+ 新建用户组

  net localgroup [groupName] /add

+  删除用户组

  net localgroup [groupName] /delete

### 文件和目录

列出目录结构：dir [path]:\[folderName]

- 显示当前目录及其子目录下的隐藏文件

  dir /a:h /s

- 显示当前目录及其子目录下的系统文件

  dir /a:s /s 

- 显示当前目录及其子目录下的只读文件

  dir /a:r /s 

- 显示当前目录及其子目录下的存档文件

  dir /a:a /s 

创建文件并输出信息

+ 覆盖源文件

  echo "[file]" > [newfile]

+ 追加更新文件内容

  echo "[file]" >> [newfile]

目录操作

+ 创建目录

  md [folder]

+ 切换目录、盘符

  cd [path]:\[folder]、d:

+ 删除文件、目录

  del [file]、rd [folder]

  rd [folder] /S /Q    //删除目录的同时删除其中的子目录和文件

+ 查看文件内容

  type [file]

  start [file]

+ 设置文件或目录属性

  attrib +S +H [file]

  attrib +S +H [file]  /S /D

+ 重命名

  ren [file1] [file2]

+ 显示目录树

  tree [folder] /F

查找文件内容：

```shell
FIND [/V] [/C] [/N] [/I] [/OFF[LINE]] "string" [[drive:][path]filename[ ...]]
  /V         显示所有未包含指定字符串的行。
  /C         仅显示包含字符串的行数。
  /N         显示行号。
  /I         搜索字符串时忽略大小写。
  /OFF[LINE] 不要跳过具有脱机属性集的文件。
  "string" 指定要搜索的文本字符串。
  [drive:][path]filename
             指定要搜索的文件
find "abc" D:\abc\abc.txt#查找含有abc的行,并输出该行
find /n "abc" D:\abc\abc.txt#查找含有abc的行,并输出该行和行号
```

### 网络/服务

ping

+ 死亡之 ping

  ping -t -l 65500 [IP or domain]

ipconfig

+ 查看当前所有的 ip 地址信息

  ipconfig -all

+ 清除当前获取到的 ip 地址

  ipconfig /release

+ 重新获取 ip 地址

  ipconfig /renew

natstat

+ 列出所有端口的使用情况

  netstat -ano

+ 显示连接进程的情况，通常用于查找是否有木马程序

  netstat -o 

arp

+ 显示 ARP 列表

  arp -a

+ 清除 ARP 列表，需要管理员权限

  arp -d

+ 添加静态项

  arp -s [ip] [macAddress]  // 

tracert

+ 跟踪路由

  tracert [IP or domain]

nslookup

+ 查询指定域名的dns信息

  nslookup www.sangfor.com.cn 

route

+ 打印路由表

  route print 

+ 新增路由

  route add [network] mask [mask] [next-hop]

net

- 查看当前局域网内的其他连接者

  net view

- 查看开启了哪些服务

  net start

- 开启某一个服务

  net start [serviceName]

- 停止某一个服务

  net stop [serviceName]

- 将共享的某一个服务器的C 盘映射成 K 盘，攻击者常用命令

  net use k: \\[ipAddress]\c$

- 查看本地开启的共享

  net share

- 开启 ipc$ 共享

  net share ipc$

- 删除 ipc$ 共享

  net share ipc$ /del 

- 删除 C 盘的共享

  net share c$ /del 

## 操作系统权限

### 类型

用户帐户

-  本地用户
-  域用户

组帐户

-  everyone组
-  network组

### 安全标识符（Security Identifier，SID）

+ 安全主体的代表（标识用户、组和计算机账户的唯一编码）

+ 范例：S-1-5-21-1736401710-1141508419-1540318053-1000

  第一项S表示该字符串是SID

  第二项是SID的版本号，对于2000来说，这个就是1

  然后是标志符的颁发机构（identifier authority），对于2000内的帐户，颁发机构就是NT，值是5。

  然后表示一系列的子颁发机构，前面几项是标志域的，最后一个标志着域内的帐户和组

### 文件权限

> 使用访问控制列表进行文件权限管理

权限：

- 完全控制
- 修改
- 读取和执行
- 读取
- 写入
- 特别的权限

### 常见系统服务

> Windows服务是指Windows NT操作系统中的一种运行在后台的计算机程序。它在概念上类似于Unix守护进程。

### 进程

> 进程通常被定义为一个正在运行的程序的实例，当你运行一个程序，你就启动了一个进程。进程又分为系统进程和用户进程。系统进程主要用于完成操作系统的功能，而QQ、Foxmail等应用程序的进程就是用户进程。

常见进程

- System.exe windows：系统进程，一个重要的进程
- System Idle Process.exe ：系统进程，它的作用是显示系统有多少闲置的cpu资源。
- svchost Service Host Process：是一个标准的动态连接库主机处理服务。Svchost用来启动服务。Svchost.只是负责为这些服务提供启动的条件，其自身并不能实现任何服务的功能，也不能为用户提供任何服务。 经常会被病毒利用进行dll注入的进程。
- explorer.exe ：用于控制Windows图形，包括开始菜单、任务栏，桌面和文件管理。
- lsass.exe ：系统进程 这是一个本地安全权限服务管理 进程详解：管理 IP安全策略以及启动 ISAKMP/Oakley (IKE) 和 IP 安全驱动程序。
- services.exe： 系统进程 用与管理启动和停止Windows服务，该进程也管理计算机启动和关机时的运行的服务，所以很重要。
- alg.exe：这是一个应用层网关服务用于网络共享，alg.exe是微软Windows操作系统自带的程序。它用于处理微软Windows网络连接共享和网络连接防火墙。
- csrss.exe： 微软客户端/服务端运行时子系统。该进程管理Windows图形相关任务。注意：有些病毒也会创业该进程。
- mdm.exe ：微软Windows进程除错程序。用于使用可视化脚本工具对Internet Explorer除错。
- taskmgr.exe： 任务管理器进程。
- rundll32.exe：用于在内存中运行DLL文件，它们会在应用程序中被使用，一般有多个。
- smss.exe： 微软Windows操作系统的一部分。
- winlogon ：管理用户登录和退出的。
- Wuauclt.exe：Windows自动升级管理程序。
- spoolsv.exe ：打印的进程 

进程查看辅助工具：

- Pchunter
- ProcessExplorer
- ProcessHacker
- ProcessMonitor
- 火绒剑

### 线程

> 通常在一个进程中可以包含若干个线程，它们可以利用进程所拥有的资源，在引入线程的操作系统中，通常都是把进程作为分配资源的基本单位，而把线程作为独立运行和独立调度的基本单位，由于线程比进程更小，基本上不拥有系统资源，故对它的调度所付出的开销就会小得多，能更高效的提高系统内多个程序间并发执行的程度。

## 日志分析

> 在应急响应过程中，日志是我们首选需要调取的重要材料信息之一，通过日志我们可以快速还原出攻击者的攻击手法，并且通过日志内容找出攻击入口点，从而准确及时堵住攻击入口，并且修复安全缺陷，保障组织的业务及产品安全。

### Windows审计策略

> WINDOWS审计日志通过审计策略进行配置，审计策略默认关闭。双击需要开启的审计项，开启审计策略

![image-20211207143413380](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20211207143413.png)



事件查看器

![image-20211207143521250](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20211207143521.png)

- 应用程序（Application）

  记录的是应用程序在系统中产生的事件信息

- 安全（Security）

  记录系统的安全信息，包括成功的登陆、退出，不成功的登陆，系统文件的创建、删除、更改

- 系统（System）

  记录windows系统组件产生的事件，主要包括驱动程序、系统组件、应用程序错误信息等

### Web审计日志分析

> 辅助工具：Web日志安全分析工具 v2.0、360星图、日志宝

Apache日志：

- Windows

  <Apache安装目录>\logs\access.log 

- Linux

  RHEL：/var/log/httpd/access_log

  Centos：/var/log/httpd/access_log

  Fedora：/var/log/httpd/access_log

  Ubuntu：/var/log/apache2/access.log

  Debian：/var/log/apache2/access.log

Apache日志配置文件：

- /etc/httpd/conf/httpd.conf（Centos/RHEL）
- /etc/apache2/apache2.conf（Ubuntu/Debian）

Tomcat日志：通常位于Tomcat安装目录下的logs文件夹内

Ngnix日志：存储路径在Nginx配置文件中conf\nginx.conf 

### 系统审计日志分析

> 辅助工具Log Parser（WINDOWS）

RDP登陆分析：RDP登录日志位于windows安全日志中，登录类型为10

- 登录失败事件

  比如RDP爆破登录

- 异常账号的登陆成功时间

  如非管理员维护账号的登陆行为

- 异常IP的登录成功事件

  如登陆IP为

- 异常时间的登录成功事件

  如凌晨2点等非常规时间段远程登录主机

WIN文件共享登陆分析：共享目录登录记录位于windows安全日志中，登录类型为3

Windows账户管理日志分析：Windows帐户管理日志记录于Windows安全日志中

+ 创建账户
+ 账户权限分配