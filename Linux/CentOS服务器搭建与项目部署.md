# CentOS服务器搭建与项目部署

## 一、连接服务器

### 1、服务器相关软件

#### ①XShell

通过XShell连接远程Linux服务器：

[XShell]: https://www.netsarang.com/zh/xshell/	"下载"

![image-20210304094157246](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210304094157246.png)

#### ②WinSCP

WinSCP是远程连接centos的工具，作用实现文件传输作用：

[WinSCP]: https://winscp.net/eng/download.php	"下载"

![image-20210304094559206](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210304094559206.png)

## 二、CentOS服务器搭建

### 1、环境配置

首先检查CentOS的版本

```
cat /etc/issue 
```

#### ①环境检查

如果服务器不是空服务器，需要先进行环境检查，确认是否有安装相关的环境，否则可直接转向②。

检查JAVA

```
java -version
```

检查MySQL

```
mysql -V
```

检查TomCat

```
rpm -qa|grep tomcat
ps -ef|grep tomcat
```

#### ②环境安装

##### 宝塔7.5

Centos安装命令：

```
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
```

安装后：启动命令：/etc/init.d/bt start 

然后控制台会打印面板地址和用户名和密码

##### JDK1.8

[安装教程]: https://www.cnblogs.com/wangzhichao/p/12692179.html

##### Mysql5.7、PHP7.2、nginx1.18

以上环境可在宝塔内安装：

![image-20210308165556413](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308165556413.png)

打开宝塔界面，打开软件商店，在搜索栏上搜索安装即可（注意选择编译安装）

##### Redis

[安装教程]: https://www.cnblogs.com/hunanzp/p/12304622.html

![image-20210308182115135](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308182115135.png)

[密码的设置与查看参照]: https://blog.csdn.net/qq_42815754/article/details/83827375

##### GitLab

[安装教程]: https://blog.csdn.net/duyusean/article/details/80011540

![image-20210308170340213](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308170340213.png)

![image-20210308170518587](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308170518587.png)

若出现policycoreutils-python is needed by缺少依赖错误

[错误参照]: https://blog.csdn.net/fu18838928050/article/details/107901895

![image-20210308170643418](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308170643418.png)

##### Tomcat9

[安装教程]: https://www.cnblogs.com/boguse/p/8394976.html

![image-20210308171537730](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308171537730.png)

然后由于手动安装Tomcat我们还需要对tomcat进行jdk环境配置：

[配置参照]: https://www.cnblogs.com/kikis/p/10755698.html

![image-20210308171830345](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308171830345.png)

注意JAVA_HOME要配置java的安装路径

然后输入 cd bin 进入 bin 目录，再输入 ./startup.sh 启动

如果出现 Cannot find /usr/local/tomcat/bin/setclasspath.sh错误

请在命令行输入unset CATALINA_HOME 即可

##### 禅道

[安装教程]: https://www.zentao.net/book/zentaopmshelp/101.html
[禅道源码安装下载]: https://www.zentao.net/download/zentaopms12.5.3-80319.html

![image-20210308172415409](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308172415409.png)

选择官方下载源

--------------------

![image-20210308172508955](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308172508955.png)

解压的文件夹目录可以自行选择，但需与nginx里面的root指定路径一致。下面进行nginx部署配置：

[nginx部署禅道参照]: http://www.voidcn.com/article/p-ahxxkdwr-bxg.html

![image-20210308172823162](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308172823162.png)

注意在此配置中的fastcgi_pass需与php-fpm文件的listen 配置一致否则会报404错误

[错误参照]: https://www.cnblogs.com/deverz/p/8875406.html

![image-20210308173015595](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308173015595.png)

然后进行安装禅道继续参照安装步骤即可。

注意在配置数据库步骤

![image-20210308173556542](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308173556542.png)

这里数据库要先建立好，在填入相关数据。

## 三、项目部署

### 1、前端部署

在项目打包之前配置静态资源路径和访问资源路径

#### ①静态资源路径

在vue.config.js文件中修改

![image-20210308174259147](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308174259147.png)

红线处修改为打包后的文件夹名

#### ②访问资源路径

在config文件夹的index.js文件中进行修改

![image-20210308174449993](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308174449993.png)

dev中的路径需要修改为请求路径

----

将vue项目通过npm run build命令对vue项目进行打包

打包后生成dist文件，将dist文件放到/usr/local文件夹下

然后进行nginx配置，进入nginx.conf添加如下配置

```
server {
  listen 83;
  server_name localhost;

  # 注意设定 root路径是有dist的
  location / {
    root /usr/local;
    index /index.html;
  }

  #跨域 ip和port自行替换
  location /adminApi {
    proxy_pass http://117.78.8.178:8080;
  }

}
```

然后访问即可，注意需要开放listen对应的端口。

### 2、后端部署

通过idea打开项目文件，打开application.yml文件

配置redis、mysql信息

![image-20210308175321022](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308175321022.png)

host填写服务器地址，密码即redis配置的密码

![image-20210308175426847](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308175426847.png)

数据库配置对应的库名、用户名和密码

注意你的项目路径不能存在中文，否则会报空指针异常

![image-20210308175550344](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308175550344.png)

本示例使用war包模式打包，然后由于要部署到服务器上，注意对启动文件进行更改。

[部署参考]: https://blog.csdn.net/qq_22638399/article/details/81506448	"jar包也可以参考此配置"

![image-20210308175859617](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308175859617.png)

![image-20210308175921677](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308175921677.png)

![image-20210308180148269](https://gitee.com/PrayFancyXl/typora-image/raw/master/img/image-20210308180148269.png)

然后打包成war包直接放到tomcat的webapp目录下即可，它会自动解压。

