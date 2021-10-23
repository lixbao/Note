# Linux下安装MySql

> 系统：Debian 10.5 （阿里云）
>
> 工具：Xshell 6 (Build 0206)



1、官网下载mysql 官网地址：[https://dev.mysql.com/downloads/repo/apt/](https://links.jianshu.com/go?to=https%3A%2F%2Fdev.mysql.com%2Fdownloads%2Frepo%2Fapt%2F)

![image-20210816140917562](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210816140917.png)





2、下载下来之后cd mysql*.deb文件夹中打开终端 sudo dpkg -i *.deb 弹出图形界面选择ok

![image-20210816141142829](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210816141142.png)



![image-20210816141104659](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210816141104.png)



3、在终端中 sudo apt-get update

4、sudo apt-get install mysql-server

![image-20210816141242364](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210816141242.png)



**5、授权mysql远程连接**

两种方法

1 改表法。可能是你的帐号不允许从远程登陆，只能在localhost。这个时候只要在localhost的那台电脑，登入mysql后，更改 "mysql" 数据库里的 "user" 表里的 "host" 项，从"localhost"改称"%"

mysql -u root -p vmware

mysql>use mysql;

mysql>update user set host = '%' where user = 'root';

mysql>select host, user from user;



2 授权法。例如，你想myuser使用mypassword从任何主机连接到mysql服务器的话。

GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;

如果你想允许用户myuser从ip为192.168.1.3的主机连接到mysql服务器，并使用mypassword作为密码

GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.1.3' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;

flush privileges;  刷新权限使之生效



以mysql_native_password加密的方式设置MySQL密码

select user,plugin from user;

 ALTER USER 'dvwa'@'%' IDENTIFIED WITH mysql_native_password BY 'Sakura_123';

![image-20211001111418003](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20211001111418.png)
