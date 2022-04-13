# Linux操作系统基础

## 系统常用命令

### 文件目录管理

#### 命令帮助：

+ --help：如ls --help
+ man：如man ls

#### 目录操作：ls、stat

> stat命令用于显示文件的状态信息。stat命令的输出信息比ls命令的输出信息要更详细。
>
> •参数：
>
> -L：支持符号连接； 
>
> -f：显示文件系统状态而非文件状态； 
>
> -t：以简洁方式输出信息； 
>
> --help：显示指令的帮助信息； 
>
> --version：显示指令的版本信息。

+ la：所有文件包括隐藏文件列表

+ ll：所有文件详细信息

  ![image-20211207114454867](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20211207114455.png)

+ ls -t：按修改时间降序排列

+ ls -al：即ll+la

+ ls -lt：即ll+（ls -t）

+ ls -ltr：按修改时间列出文件详细信息

+ ls -lut：按访问时间顺序查看

#### 文件权限属性设置：chmod、chgrp

> u User，即文件或目录的拥有者； 
>
> g Group，即文件或目录的所属群组； 
>
> o Other，除了文件或目录拥有者或所属群组之外，其他用户皆属于这个范围； 
>
> a All，即全部的用户，包含拥有者，所属群组以及其他用户； 
>
> r=读取属性　　//值＝4 
>
> w=写入属性　　//值＝2 
>
> x=执行属性　　//值＝1
>
> chgrp修改文件的所属组、chown修改文件的所有者

+ 字母法：chmod u+x，g+w file
+ 数字法：chmod 777 file
+ 改变abc所属的组为root：chgrp root abc

#### 文件查找：find

+ 根据文件或者正则表达式匹配：find /home -name "*.txt"
+ 否定参数：find /home ! -name "*.txt"
+ 根据文件时间戳：find . -type f -atime -7
  + 访问时间（-atime/天，-amin/分钟）：用户最近一次访问时间。 
  + 修改时间（-mtime/天，-mmin/分钟）：文件最后一次修改时间。 
  + 变化时间（-ctime/天，-cmin/分钟）：文件数据元（例如权限等）最后一次修改时间。

+ 按文件权限匹配：find . type f -perm 777

#### 文件内容查看：

+ cat
+ more
+ less

#### 文件编辑：

+ vi、vim
+ sed

#### 文件过滤：grep

+ 在文件中搜索一个单词，命令会返回一个包含“match_pattern”的文本行：

  grep match_pattern file_name

  grep "match_pattern" file_name

+ 使用正则表达式 -E 选项：

  grep -E "[1-9]+" 或 egrep "[1-9]+"

+ 统计文件或者文本中包含匹配字符串的行数 -c 选项：

  grep -c "text" file_name

#### 目录操作：

+ mkdir：创建目录
+ rm：删除一个目录中文件或目录

#### 文件解压缩：tar

+ tar -cvf log.tar log2012.log  仅打包，不压缩 

+ tar -zcvf log.tar.gz log2012.log  打包后，以 gzip 压缩 

+ tar -jcvf log.tar.bz2 log2012.log  打包后，以 bzip2 压缩 

+ tar -zxvf /opt/soft/test/log.tar.gz	将tar包解压缩

#### 文件分析：

+ awk：awk [选项参数] 'script' var=value file(s)

  awk '{[pattern] action}' {filenames}  # 行匹配语句 awk '' 只能用单引号

  awk '{print $1,$4}' log.txt  # 对log.txt文件内容每行按空格或TAB分割，输出文本中的1、4项

+ sort：将文本文件内容加以排序

  

### 系统管理

#### 系统安全

+ last：显示用户最近登录信息
  + -a：把从何处登入系统的主机名称或ip地址，显示在最后一行； 
  + -d：将IP地址转换成主机名称； 
  + -f <记录文件>：指定记录文件。 
  + -n <显示列数>或-<显示列数>：设置列出名单的显示列数； 
  + -R：不显示登入系统的主机名称或IP地址； 
  + -x：显示系统关机，重新开机，以及执行等级的改变等信息。
+ lastb：显示用户错误的登录列表，参数同last
+ w：命令用于显示已经登陆系统的用户列表，并显示用户正在执行的指令
  + -h：不打印头信息； 
  + -u：当显示当前进程和cpu时间时忽略用户名； 
  + -s：使用短输出格式； 
  + -f：显示用户从哪登录； 
  + -V：显示版本信息。

#### 进程和作业管理

+ ps：报告当前系统的进程状态
  + ps -aux
  + ps -ef
  + ps -aux|less
+ pidof：查找指定名称的进程的进程号id号
  + -s：仅返回一个进程号； 
  + -c：仅显示具有相同“root”目录的进程； 
  + -x：显示由脚本开启的进程； 
  + -o：指定不显示的进程ID。
+ top：实时动态地查看系统的整体运行情况

#### 系统监测

+ lsof：查看你进程开打的文件，打开文件的进程，进程打开的端口(TCP、UDP)

+ mpstat：显示各个可用CPU的状态信息

+ netstat：打印Linux中网络系统的状态信息

#### 内核查看：uname

+ -a或--all：显示全部的信息； 
+ -m或--machine：显示电脑类型； 
+ -n或-nodename：显示在网络上的主机名称；
+  -r或--release：显示操作系统的发行编号； 
+ -s或--sysname：显示操作系统名称；
+ -v：显示操作系统的版本； 
+ -p或--processor：输出处理器类型或"unknown"； 
+ -i或--hardware-platform：输出硬件平台或"unknown"；
+ -o或--operating-system：输出操作系统名称； 
+ --help：显示帮助； 
+ --version：显示版本信息。

### 用户组

> 超级用户： root 具有操作系统的一切权限 ，UID 值为0
>
> 普通用户：普通用户具有操作系统有限的权限，UID值 500 -- 6000
>
> 伪用户：是为了方便系统管理，满足相应的系统进程文件属主的要求，伪用户不能登录系统，UID值 1 -- 499

#### 账号管理

+ useradd [user]：创建用户
+ passwd [user]：设置用户密码
+ userdel [user]：删除用户
+ usermod -L [user]：锁定用户
+ usermod -U [user]：解锁用户

#### 用户组管理

+ groupadd [group]：添加用户组
+ groupmod -n [new] [group]：修改组名
+ groupdel [group]：删除组账号
+ gpasswd -a [user] [group]：添加用户到组
+ gpasswd -d [user] [group]：从组中删除用户
+ groups [user]：查看用户属组

## 服务管理

> 有些程序在启动之后持续在后台运行，等待用户或其他应用程序调用，此类程序就是服务（service）
>
> 大多数服务都是通过守护进程（daemon）实现的。守护进程是服务的具体实现。

#### 服务的类型：

+ 系统服务（System Service）

  系统服务是指那些为系统本身或者系统用户提供的一类服务，如提供作业调度服务的Cron服务。

+ 网络服务（Networking Service）

  网络服务是指供客户端调用的一类服务，如Web服务、文件服务等。

+ 独立服务（Standalone Service）

  独立服务一经启动，将始终在后台执行，除非关闭系统或强制中止。多数服务属于此种类型。

+ 临时服务（Transient Service）

  临时服务只有当客户端需要时才会被启动，使用完毕就会结束。

+ 超级服务xinetd

  用于管理其他服务

#### 常见守护进程：

+ atd：at和batch命令守护进程，用户用at命令调度的任务。batch用于在系统负荷比较低时运行批处理任务
+ crond：linux下的计划任务
+ httpd：WEB服务器
+ Lpd：打印服务器
+ named：DNS服务器
+ mysqld：mysql数据库引擎守护进程
+ postgresql:一种SQL数据库服务器
+ sendmail:邮件服务器sendmail 
+ smb:Samba文件共享/打印服务 
+ snmpd:本地简单网络管理候进程 
+ squid:激活代理服务器squid 
+ sshd：OpenSSH服务器守护进程。
+ syslog：一个让系统引导时启动syslog和klogd系统日志守候进程的脚本。
+ vsftpd：vsftpd服务器的守护进程。
+ vncserver：VNC守护进程
+ zebra：路由治理守护进程。
+ apache2：apache守护进程

## 文件系统

>Linux常见文件系统均为无结构的字符流形式，文件名是文件的标识，由字母、数字、下划线和圆点组成的字符串构成。

#### 常见目录：

- / 根目录
- /bin 存放必要的命令
- /boot 存放内核以及启动所需的文件
- /dev 存放设备文件
- /etc 存放系统的配置文件
- /home 用户文件的主目录,用户数据存放在其主目录中
- /lib 存放必要的运行库
- /mnt 存放临时的映射文件系统
- /proc 存放存储进程和系统信息
- /root 超级用户的主目录 
- /sbin 存放系统管理程序
- /tmp 存放临时文件的目录
- /usr 包含一个不要修改的应用程序,命令程序文件,程序库,手册
- /var 包含系统产生的经常变化的文件

#### 关键文件：

- /etc/passwd ：用户配置文件
- /etc/shadow：用户的密码信息
- /var/log/messages(Solaris的为/var/adm/messages)：核心系统日志文件
- /var/log/cron：crontab守护进程crond所派生的子进程的动作
- /var/log/dmesg ：内核缓冲信息（kernel ring buffer）
- /var/log/secure：ssh登录日志
- /var/log/lastlog：最近成功登录的事件和最后一次不成功的登录事件
- /var/log/wtmp：永久记录每个用户登录、注销及系统的启动、停机的事件

## 日志分析

### Linux审计策略

+ syslog/rsyslog

  系统缺省已经开启syslog/rsyslog服务，禁止关闭。系统syslog/rsyslog服务会将所有系统日志自动记录到/var/log/messages文件中。

### 系统审计日志分析

SSH：（账户管理日志：/var/log/auth（/var/log/secure））

+ RHEL/Centos：/var/log/secure
+ Ubuntu/Debian：/var/log/auth.log

SSH登陆分析：

+ 登录时间异常的IP

  grep "Accepted" /var/log/auth.log | awk '($3<="06:00"){print$11}' | sort | uniq -c | sort -rn

+ 登录频繁的IP

  grep "Accepted" /var/log/auth.log | awk '($3<="06:00"){print$11}' | sort | uniq -c | sort -rn*

+ 登录失败的IP

  grep "Failed password for root" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -rn

FTP：

+ /var/log/xferlog/ 主要记录FTP用户进行的一些上传下载行为

+ /var/log/vsftpd.log 主要记录用户的登录日志