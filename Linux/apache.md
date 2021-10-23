# Linux下安装Apache及证书服务

> 系统：Debian 10.5 （阿里云）
>
> 工具：Xshell 6 (Build 0206)

## 1.安装Apache2

``` shell
sudo apt-get update
sudo apt-get install apache2
```

![image-20210802180116222](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210802180116.png)



## 2.开放端口

为了后续验证需前往云服务器控制台开放80和443端口



## 3.进程管理

```shell
systemctl start apache2 	#启动进程
systemctl stop apache2		#关闭进程
systemctl status apache2	#查看状态
systemctl reload apache2	#重新加载
systemctl disable apache2	#禁用自启动
systemctl enable apache2	#自启动
```

![image-20210803135319337](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210803135319.png)



## 4.设置虚拟主机

```shell
sudo mkdir -p /var/www/your_domain	#使用-p标志创建所有必需的父目录
sudo chown -R $USER:$USER /var/www/your_domain	#使用$USER环境变量分配目录的所有权
sudo chmod -R 755 /var/www/your_domain	#确保根目录权限
```

![image-20210803142746864](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210803142746.png)

然后就可以添加一个示例主页了

```shell
vim /var/www/your_domain/index.html
<html>
    <head>
        <title>Welcome to your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain virtual host is working!</h1>
    </body>
</html>
```

为了使Apache可以提供此内容，必须使用正确的指令创建虚拟主机文件

```shell
vim /etc/apache2/sites-available/your_domain.conf
<VirtualHost *:80>
    ServerAdmin admin@your_email_domain
    ServerName your_domain
    ServerAlias www.your_domain
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    
    
</VirtualHost>
```

使用`a2ensite`工具启用文件：

```shell
sudo a2ensite your_domain.conf
sudo apache2ctl configtest #该命令可以测试是否有配置错误
sudo systemctl restart apache2	#重启使配置生效
```

然后就可以通过域名（需自行申请备案一个域名）访问你的站点了

此时输入IP访问的是html目录下的默认站点

## 5.申请SSL并下载

> 原DIgicert 免费单域名证书，org、jp等特殊域名存在无法申请的情况，正式环境建议使用付费证书。
>
> 每个实名主体个人/企业，一个自然年内可以领取一次数量为20的云盾单域名试用证书，如需更多云盾单域名试用证书需要额外付费购买。
>
> 证书个数是指可以签发证书的数量
>
> 例如，证书个数为1，将签发一张有效期为1年的证书
>
> **证书个数支持同时签发多张证书或者1张证书托管多年的服务**

![image-20210803140401335](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210803140401.png)

## 6.安装SSL

```SHELL
sudo apt-get install openssl    #安装SSL
sudo a2enmod ssl    #开启SSL
```

![image-20210803140650540](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210803140650.png)



此时会在/etc/apache2/sites-available中生成default-ssl.conf

![image-20210803141053615](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210803141053.png)



在sites-enabled目录下可以发现，里面有一个文件000-default.conf，实质上这个文件是/etc/apache2/sites-available/000-default.conf这个文件的软链接，它保证了我们可以通过http方式访问

同理设置一个default-ssl.conf的软链接，把这个文件链接到sites-enabled这个文件夹中:

```shell
ln -s /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-enabled/000-default-ssl.conf
```

![image-20210803141417731](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210803141417.png)

现在对证书文件进行配置，因为已经做了软链接，这时候修改000-default-ssl.conf或default-ssl.conf都一样

然后把从阿里云上面下载好的证书(3个文件)传到你自定义的目录中，利用rz命令上传

```shell
apt-get install lrzsz # 安装完输入rz命令即可上传命令了，-f 可以覆盖上传，默认上传至云服务器上输入命令rz时所在的目录；从云服务器上下载文件的命令式sz
```

![image-20210803145205530](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210803145205.png)

然后我们需要修改一下，修改成这样:``

```
<IfModule mod_ssl.c>
    <VirtualHost _default_:443>
        ServerAdmin 你的邮箱
        
        DocumentRoot /var/www/你的目录
        ServerName 你的域名
 
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
 
        SSLEngine on
        # 注意，需要添加这三行
        SSLCertificateFile 你自定义的路径/2_xxx.xxx.xxx.crt
        SSLCertificateKeyFile 你自定义的路径/3_xxx.xxx.xxx.key
        SSLCertificateChainFile 你自定义的路径/1_root_bundle.crt
    
        <FilesMatch "\.(cgi|shtml|phtml|php)$">
                SSLOptions +StdEnvVars
        </FilesMatch>
        <Directory /usr/lib/cgi-bin>
                SSLOptions +StdEnvVars
        </Directory>
    </VirtualHost>
</IfModule>
```

```shell
sudo a2enmod ssl   #加载模块
sudo service apache2 restart # 重启服务
```

![image-20210803150542778](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210803150542.png)

## 7.强制使用https

```shell
#将以下内容加入到/etc/apache2/sites-available/中对应的配置文件中
RewriteEngine on
RewriteCond   %{HTTPS} !=on
RewriteRule   ^(.*)  https://%{SERVER_NAME}$1 [L,R]
```

然后启动 Apache2 的重定向：

```
#sudo a2ensite your_domain.conf
sudo a2enmod rewrite
sudo service apache2 restart
```

## 8.安装PHP并集成Apache

```shell
sudo apt update
sudo apt install php libapache2-mod-php
sudo systemctl restart apache2
```

### 安装PHP扩展模块

您可以通过安装其他扩展来扩展PHP核心功能。 PHP扩展可以作为软件包提供，并且可以通过键入以下命令轻松安装：

```shell
sudo apt install php-[extname]
```

例如，要安装MySQL和GD PHP扩展，您将运行以下命令：

```shell
    sudo apt install php-mysql php-gd
```

安装PHP扩展时，请不要忘记重新启动Apache或PHP FPM服务，具体取决于您的设置。

