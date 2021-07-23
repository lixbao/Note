# Linux系统加固思路

![image-20210702162703685](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210712225158.png)

## 1.系统账号安全

### 1.1 禁用或删除无用账号

目的：减少系统无用账号，降低安全风险

检查方法：

+ cat /etc/passwd
+ cat /etc/shadow

> 查看账户、口令文件，与系统管理员确认不必要的账号。对于一些保留的系统伪账户如：bin、sys、adm、uucp、lp、nuucp、hpdb、www、daemon等可根据需要进行锁定登陆

操作步骤：

+ 使用命令 userdel <用户名> 删除不必要的账户
+ 使用命令 passwd -l <用户名> 锁定不必要的账户
+ 使用命令passwd -u <用户名> 解锁必要的用户



### 1.2 检查特殊账号

目的：检查是否存在空口令或和root权限的账号

操作步骤：

（1）查看空口令和root权限账号，确认是否存在异常账号：

+ 使用命令 awk -F: '($2=="")' /etc/shadow 查看空口令账号
+ 使用命令 awk -F: '($3==0)' /etc/passwd 查看UID为0的账号

（2）加固口令账号：

+ 使用命令 passwd <用户名> 为空口令账号设置密码
+ 确认UID为0的账号只有root



### 1.3 添加口令策略

目的：加强口令额复杂度等，降低被猜解的可能性

检测方法：查看密码策略设置

+ ```shell
  cat /etc/login.defs|grep PASS
  ```

操作步骤：

（1）使用命令 vi /etc/login.defs 修改配置文件

+ ``` shell
  PASS_MAX_DAYS 90 #新建用户的密码最长使用天数
  PASS_MIN_DAYS 0 #新建用户的密码最短使用天数
  PASS_WARN_AGE 7 #新建用户的密码到期前提醒天数
  ```

（2）使用chage命令修改用户设置

+ ``` shell
  chage -m 0 -M 30 -E 2022-01-01 -W 7 <user> #将user的密码最长使用天数设为30，最短使用天数设为0，密码于2022-01-01过期，过期前7天警告用户

（3）设置密码复杂度

+ ``` shell
  vim /etc/pam.d/system-auth #做如下修改
  ```

+ 在password requisite pam_cracklib.so后添加try_fist_pass retry=3 dcredit=-1 lcredit=-1 ucredit=-1 ocredit=-1 minlen=8

+ 上述修改意为：至少包含一个数字、一个小写字母、一个大写字母、一个特殊字符且密码长度大于等于8

（4）设置连续输错3次密码改账号锁定5分钟

+ ``` shell
  vi /etc/pam.d/common-auth #添加如下内容
  ```

+ ``` shell
  auth required pam_tally.so onerr=fail deny=3 unlock_time=300
  ```



### 1.4 限制用户su

目的：限制能su到root的用户

操作步骤：

+ ```shell
  vi /etc/pam.d/su #添加如下行
  ```

+ ```shell
  auth required pam_wheel.so group=test
  #仅允许test组用户su到root
  ```



### 1.5 禁止root用户直接登陆

目的：限制root用户直接登录

检查方法：

+ telnet：输入命令“cat /etc/securetty”查看根用户登陆的设备名
+ 默认是vc/1、vc/2、……、tty1、tty2、……、pts/0、pts/1
+ ssh：输入命令“cat /etc/ssh/sshd_config”查看PermitRootLogin后是yes还是no

操作步骤：

+ 创建普通权限账号并配置密码，防止无法远程登陆
+ telnet：删除pts/0、pts/1
+ 修改/etc/ssh/sshd_config中的PermitRootLogin为no，使用service sshd restart重启服务后生效



### 1.6 多次登陆失败锁定

目的：当用户试图登陆多次失败后，锁定此用户

操作步骤：

+ ```shell
  vi /etc/pam.d/system-auth
  ```

+ ```shell
  auth requisite pam_tally.so per_user onerr=fail deny=4 unlock_time=3600
  ```

+ 注：超过4次错误就会lock user，为了防止拒绝服务攻击，加入per_user参数



## 2.文件系统安全

### 2.1 重要目录和文件的权限设置

目的：合理配置重要目录和文件权限，增强安全性

检查方法：

+ ```shell
  #执行以下命令检查目录和文件的权限设置情况：
  chmod 700 /bin/rpm  
  chmod 600 /etc/exports  #NFS共享目录配置文件
  chmod 600 /etc/hosts.*  #主机访问控制文件
  chmod 644 /var/log/messages
  ```

+ ```shell
  #系统日志配置文件
  chmod 640 /etc/syslog.conf
  chmod 660 /var/log/wtmp
  chmod 640 /var/log/lastlog
  chmod 600 /etc/ftpusers
  ```

+ ```shell
  #用户口令文件
  chmod 644 /etc/passwd
  chmod 600 /etc/shadow
  ```

+ ```shell
  #对于重要目录，建议执行如下类似操作：
  #这样只有root可以读、写和执行这个目录下的脚本
  chmod -R 750 /etc/rc.d/init.d/*
  ```



### 2.2 设置umask值

目的：设置默认的umask值，增强安全性

操作步骤：

+ 使用命令 vi /etc/profile 修改配置文件，添加行 umask 027， 即新创建的文件属主拥有读写执行权限，同组用户拥有读和执行权限，其他用户无权限。
+ 使用命令 vi /etc/profile 修改配置文件，将以 TMOUT= 开头的行注释，设置为TMOUT=180，即超时时间为三分钟



## 3.服务协议安全

### 3.1 关闭不必要的服务

目的：检测是否有与业务无关的服务自启，关闭不必要的服务（如普通服务和xinetd服务），降低风险。

操作步骤：

+ 使用命令systemctl disable <服务名>设置服务在开机时不自动启动。
+ 说明： 对于部分老版本的Linux操作系统（如CentOS 6），可以使用命令chkconfig --level 
  <init级别> <服务名> off设置服务在指定init级别下开机时不自动启动。

> 常见的无用服务有：
>
> lpd服务：打印机服务
> kshell服务：544/tcp kshell Kerberos 版本5（v5）远程 shell 
> time服务：37时间协议
> ntalk服务：518/udp网络交谈（ntalk），远程对话服务和客户 
> klogin服务：543/tcp klogin Kerberos 版本5（v5）远程登录 



### 3.2 SSH服务安全

目的：对SSH服务进行安全加固，防止暴力破解成功

操作步骤：

+ ```shell
  vim /etc/ssh/sshd_config
  
  #设置如下参数的值
   PermitRootLogin no #不允许root登陆
   Protocol 2 #设置ssh使用额版本协议
   MaxAuthTries 3 #设置允许密码错误次数（默认为6）
  ```

  

## 3.3 禁止匿名FTP

目的：禁止匿名用户登陆FTP

操作步骤：

+ ```shell
  vim /etc/vsftpd/vsftpd.conf
  anonymous_enable=NO    #如果存在anonymous_enable则修改,如果不存在则手动增加
  ```

  

## 4.日志安全

### 4.1 rsyslogd日志

目的：启动日志功能，并配置日志记录，查看所有日志记录

操作步骤：

+ ```shell
  #Linux系统默认启用如下类型日志
  /var/log/messages #系统日志（默认）
  /var/log/cron #cron日志（默认）
  /var/log/secure #安全日志（默认）
  #注意：部分系统可能使用syslog-ng日志，配置文件为：/etc/syslog-ng/syslog-ng.conf。
  ```

+ ```shell
  #查看是否有authpriv.*
  cat /etc/rsylogriz.conf|grep authpritv
  authpriv.#/var/log/socure
  #将authpirv设备的融合级别的学习记录记录到/var/log/secure文件中
  vim /etc/rsyslog.conf添加authpriv行
  ```



### 4.2 记录所有用户的登陆和操作日志

目的：通过脚本代码实现记录所有用户的登陆操作日志，防止出现安全事件后无据可查。

操作步骤：

+ 打开配置文件/etc/profile，输入以下内容

  ![image-20210723142213144](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210723142213.png)

> 通过上述步骤，可以在 /var/log/history 目录下以每个用户为名新建一个文件夹，每次用户退出后都会产生以用户名、登录IP、时间的日志文件，包含此用户本次的所有操作（root
> 用户除外）。同时，建议您使用OSS服务收集存储日志
>
> 注意： /var/log/history 是记
> 录日志的存放位置，可以自定义



## 5.其他安全设置

### 5.1 限制用户访问管理的主机

目的：设置访问控制策略限制能够管理本机的IP地址

检查方法：

```shell
#查看有无AllowUsers的语句
cat /etc/ssh/sshd_config
```

操作步骤：

+ ```shell
  vi /etc/ssh/sshd_config #添加如下语句
  ```

+ ```shell
  AllowUsers *@10.138.*.* 
  #此句意为：仅允许10.138.0.0/16网段所有用户通过ssh访问
  ```

+ ```shell
  #保存后重启ssh服务
  service sshd restart
  ```



### 5.2 屏蔽登陆banner信息

目的：避免泄露敏感信息

检查方法：

+ ```shell
  #查看文件中是否存在Banner或banner字段为NONE
  cat /etc/ssh/sshd_config
  ```

+ ```shell
  #该处内容将作为banner信息显示给登录用户
  cat /etc/motd

操作步骤：

+ ```shell
  vi /etc/ssh/sshd_config
  ```

+ ```shell
  banner NONE
  ```

+ ```shell
  vi /etc/motd #自定义修改
  ```



### 5.3 CTRL+ALT+DEL重启系统

目的：防止误用快捷键重启系统

检查方法：

+ ``` shell
  #查看输入行是否被注释
  cat /etc/inittab|grep ctrlaltdel
  ```

操作步骤：

+ ```shell
  vi /etc/inittab
  #在行开头添加注释井号
  ```

+ ```shell
  #ca::ctrlaltdel:/sbin/shutdown -t3 -r now
  ```

  

### 5.4 bash历史命令

目的：设置bash保留历史命令的条数

操作步骤：

+ ```shell
  #查看保留历史命令的条数
  cat /etc/profile|grep HISTSIZE=
  cat /etc/profile|grep HISTFILESIZE=
  ```

+ ```shell
  #直接修改HISTSIZE和HISTFILESIZE即可
  vi /etc/profile
  ```

  

### 5.5 检查Grub/Lilo密码

目的：查看系统引导管理器是否设置密码

检查方法：

+ ```shell
  #查看grub是否设置密码
  cat /etc/grub.conf|grep password
  ```

+ ```shell
  #查看lilo是否设置密码
  cat /etc/lilo.conf|grep password

操作步骤：

+ ```shell
  vi /etc/grub.conf
  #splashimage这个参数下一行添加: 
  password ***
  #如果需要md5加密，可以添加一行：
  password --md5 ***
  ```

+ ```shell
  vi /etc/lilo.conf
  p



