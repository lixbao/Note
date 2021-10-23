# 一键SSH连接

> SSH密钥登陆 + WindowsTerminal

## 一、在本地生成SSH密钥对

1.打开终端，输入ssh-keygen -t rsa执行，根据提示设置密钥保存路径、密钥密码（默认为空），建议按默认设置，一直按回车成功生成密钥文件
![image-20210805173326308](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210805173326.png)

2.密钥文件有两个，ssh_key对应私钥可存储在本地，ssh_key.pub对应公钥需要放在到目标机器



## 二、在远程主机安装公钥

1.在本地上传公钥文件

```
sftp username@ip # 回车输入密码
sftp> put 本地公钥文件 远程路径(默认为用户家目录)
```

![image-20210805173639636](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210805173639.png)



2.连接到远程主机，修改密钥及所在文件夹权限

```
mkdir -m 700 ~/.ssh #如此文件夹已存在，也要确保权限为700

#cd 密钥.pub所在目录，然后设置其权限
chmod 600 ssh_key.pub

mv ./ssh_key.pub ~/.ssh/authorized_keys
```



## 三、在远程主机打开密钥登陆功能

1.编辑sshd配置文件
`vi /etc/ssh/sshd_config`
2.编辑以下内容

```
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PermitRootLogin yes
PasswordAuthentication no #此行会关闭密码登录功能，请确认密钥登陆功能设置好后再添加
```

以上内容在配置文件里都有对应行，但被注释了起来，可通过删除注释符号设置，也可直接追加到文件末尾
3.重启sshd
`systemctl restart sshd`



## 四、设置WindowsTerminal SSH快捷键

在WindowsTerminal配置文件里增加内容，可以直接复制powershell使用

![image-20210805174319788](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210805174319.png)



复制完之后更改配置如下，主要必须更改如下参数，其余按需修改

```shell
ssh.exe -i D:\WorkPlace\IOS\kali-linux-2021.1-vmware-amd64.vmwarevm\ssh_key root@192.168.110.120
```

![image-20210805174812600](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210805174812.png)



重启窗口打开即可使用（无需输入密码）

![image-20210805174900024](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210805174900.png)