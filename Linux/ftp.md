# `Linux`下安装FTP服务

> 系统：Debian 10.5 （阿里云）
>
> 工具：Xshell 6 (Build 0206)

## 1.安装FTP

> 在安装任何软件之前，重要的是通过apt在终端中运行以下命令来确保系统是最新的

```shell
sudo apt update
```

安装vsftpd守护程序

```SHELL
sudo apt install vsftpd
sudo systemctl status vsftpd #用于确认FTP版本等信息
```

![image-20210804141515673](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210804141515.png)



## 2.配置FTP服务器

先备份配置文件，然后再对源文件进行编辑

```shell
cp /etc/vsftpd.conf /etc/vsftpd.conf.bak
```

修改源文件如下选项（可以键入esc然后输出:/加想检索的内容快速跳转）

```shell
vim /etc/vsftpd.conf
listen=NO #服务器监听
listen_ipv6=YES
anonymous_enable=NO #禁止匿名访问
local_enable=YES #允许本地主机访问
write_enable=YES #有写权限
local_umask=022 #上传文件的权限
dirmessage_enable=YES
use_localtime=YES
xferlog_enable=YES
connect_from_port_20=YES
chroot_local_user=YES #是否将所有用户限制在主目录）
chroot_list_enable=YES #设置是否将用户锁定在自己的主目录中 
chroot_list_file=/etc/vsftpd.chroot_list #可在文件中设置多个账号
secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd #根据情况可更改为ftp
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=NO
#以下需要手动添加,不添加就会出现问题见后面的内容分析
pasv_enable=Yes #允许使用pasv模式
pasv_min_port=10000
pasv_max_port=10100
allow_writeable_chroot=YES
pasv_address=0.0.0.0 #服务器外网ip
```

> 以上设置完成后需要开放端口20和10000-10100

启动并验证
```shell
sudo service vsftpd start
ps aux | grep vsftp
```

![image-20210804143627558](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210804143627.png)



## 3.配置FTP默认用户

安装好ftp后默认是会自动创建ftp用户的，然后我们设置ftp用户的密码，输入

sudo passwd ftp，然后输入密码，再确认密码。

ftp用户家目录默认如下

```shell
cat /etc/passwd | grep ftp 
ftp:x:108:119:ftp daemon,,,:/srv/ftp:/usr/sbin/nologin
```

## 4.可能出现的问题



![image-20210804153249566](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210804153249.png)

如果登录ftp总是出现密码错误，可以将/etc/vsftpd.conf配置文件的pam_service_name=vsftpd改为pam_service_name=ftp，即可解决



更改后不再出错，但如果出现如下问题

这里是因为服务器不接受被动连接（前面服务器端配置有问题，接下来会进行修改），然后win10默认使用被动连接

![image-20210804153655780](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210804153655.png)

分析之后发现，采用ftp被动模式登录成功，服务端映射给客户端的ip地址是私有的，所以客户端无法和服务器建立连接关系，也就意味着ftp命令无法使用，只能采取主动模式。解决方案如下：

```shell
listen=NO           ===》 修改为listen=YES    
listen_ipv6=YES     ===》 修改为listen_ipv6=NO   
pasv_address=公网IP    #一定要是公网IP，如果不添加，返回被动模式的ip是0.0.0.0
```

开启主动连接解决办法：打开控制面板：选择internet选项—–高级—-使用被动FTP（为防火墙和DSL调制解调器兼容性）”前面的勾去掉

![image-20210804155753099](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210804155753.png)



当然如果在不更改的情况下想要访问，可以通过cmd进行

![image-20210804154959242](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210804154959.png)

 

## 5. FTP挂载

在搭建完FTP服务器后，在Windows系统上，最常见的挂载方式是ftp映射。

右键选择【添加一个网络位置】，根据向导完成即可

![image-20210804163131203](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210804163131.png)

## 6.权限控制

> 只想让指定的账户不限制在其主目录，其它账户都限制在其主目录。

对于chroot_local_user与chroot_list_enable的组合效果，可以参考下表：

|                        | chroot_local_user=YES                                        | chroot_local_user=NO                                         |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| chroot_list_enable=YES | 1.所有用户都被限制在其主目录下 2.使用chroot_list_file指定的用户列表，这些用户作为“例外”，不受限制 | 1.所有用户都不被限制其主目录下 2.使用chroot_list_file指定的用户列表，这些用户作为“例外”，受到限制 |
| chroot_list_enable=NO  | 1.所有用户都被限制在其主目录下 2.不使用chroot_list_file指定的用户列表，没有任何“例外”用户 | 1.所有用户都不被限制其主目录下 2.不使用chroot_list_file指定的用户列表，没有任何“例外”用户 |



因安全问题，vsftpd不允许匿名用户在ftp主目录上传，可以新建一个子目录，然后设置权限为777

设置fpt相关目录及权限

```shell
sudo mkdir -p /srv/ftp/up
sudo mkdir -p /srv/ftp/server
sudo chmod 755 /srv/ftp
#server为只读文件
sudo chmod 777 /srv/ftp/up
sudo chmod 755 /srv/ftp/server
```

在文件/etc/ftpusers中列出了不可登陆ftp的用户，可按需自行注释他们



