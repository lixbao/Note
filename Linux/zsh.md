# Linux下安装zsh和oh-my-zsh

> 系统：Debian 10.5 （阿里云）
>
>工具：Xshell 6 (Build 0206)

## 1.Zsh的介绍

Zsh（Z-shell）是一款用于交互式使用的shell，也可以作为脚本解释器来使用。其包含了 bash，ksh，tcsh 等其他shell中许多优秀功能，也拥有诸多自身特色。

从 macOS Catalina 版开始，其默认shell从[bash](https://baike.baidu.com/item/bash/6367661)改为zsh。



## 2.安装Zsh

```shell
sudo apt-get update
sudo apt-get install zsh
chsh -s /bin/zsh #将zsh替换为你的默认shell
```

![image-20210727141220871](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210727141220.png)



## 3.安装git（如有请忽略此步）

```shell
sudo apt-get install git
```



## 4.安装oh-my-zsh

默认的 Zsh 配置有点麻烦。因此一个叫 robbyrussel 的用户在 GitHub 上制作了一个配置文件 oh-my-zsh，这是目前为止最流行的 Zsh 配置：https://github.com/ohmyzsh/ohmyzsh

安装教程在上面仓库的readme文件中有描述（这里以使用curl为例）

| 方式      | 命令                                                         |
| --------- | ------------------------------------------------------------ |
| **curl**  | `sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |
| **wget**  | `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |
| **fetch** | `sh -c "$(fetch -o - https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"` |

![image-20210727142721746](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210727142721.png)



## 5.主题配置

下载完oh-my-zsh后的默认主题【robbyrussell】效果如上一步效果图

主题详细预览参考仓库中的wiki

https://github.com/robbyrussell/oh-my-zsh/wiki/themes

打开配置文件【~/.zshrc】，直接更改目标位置(esc后键入冒号输入/ZSH_THEME检索)的字符串即可

![image-20210727142921803](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210727142921.png)



例如配置成【xiong-chiamiov-plus】

```shell
ZSH_THEME="xiong-chiamiov-plus"
```

![image-20210727143312961](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210727143313.png)

## 6.插件配置

同样插件在仓库wiki也有描述

https://github.com/ohmyzsh/ohmyzsh/wiki/plugins

打开配置文件【~/.zshrc】，直接在目标位置(esc后键入冒号输入/plugins检索)的括号中添加即可

![image-20210727143938921](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210727143938.png)



其实需要哪个插件就在~/.oh-my-zsh/custom/plugins/中克隆哪个仓库就行了

https://github.com/zsh-users

例如通过上方仓库添加代码高亮与自动补全

```shell
cd ~/.oh-my-zsh/custom/plugins/
git clone https://github.com/zsh-users/zsh-autosuggestions.git #自动补全
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git #代码高亮
```

![image-20210727144518803](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210727144518.png)



然后将这两个插件添加到【~/.zshrc】中

![image-20210727144839567](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210727144839.png)



补全和高亮成功设置

![image-20210727144915383](https://raw.githubusercontent.com/lixbao/PicGo/main/img/20210727144915.png)

