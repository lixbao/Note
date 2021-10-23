# 如何在Debian 10上安装Python 3和设置编程环境

## 第1步-设置Python 3 

Debian Linux附带了预安装的Python 3和Python 2。 为了确保我们的版本是最新的更新

```shell
sudo apt update
sudo apt -y upgrade
#`-y`标志将确认我们同意安装所有项目。
```

该过程完成后，我们可以通过输入以下命令检查系统中安装的Python 3版本：

```shell
python3 -V
```



要管理Python软件包，让我们安装**pip** ，该工具将安装和管理我们可能要在开发项目中使用的编程软件包。 

```shell
sudo apt install -y python3-pip
```



可以通过输入以下命令安装Python软件包：

```shell
pip3 install package_name
pip3 install -r requirement.txt
```

在这里， `package_name`可以引用任何Python包或库，例如用于Web开发的Django或用于科学计算的NumPy。 因此，如果您想安装NumPy，则可以使用命令`pip3 install numpy` 。



设置Python，安装pip和其他工具后，我们可以为开发项目设置虚拟环境。

```shell
sudo apt install build-essential libssl-dev libffi-dev python3-dev
```

## 步骤2 —设置虚拟环境 **(**Step 2 — Setting Up a Virtual Environment**)**

虚拟环境使您可以在服务器上为Python项目提供隔离的空间，从而确保每个项目都可以拥有自己的一组依赖关系，这些依赖关系不会破坏其他任何项目。

设置编程环境使我们可以更好地控制Python项目以及如何处理不同版本的软件包。 在使用第三方软件包时，这一点尤其重要。

您可以根据需要设置任意数量的Python编程环境。 每个环境基本上都是服务器上的目录或文件夹，其中包含一些脚本以使其充当环境。

尽管有几种方法可以在Python中实现编程环境，但我们将在这里使用**venv**模块，该模块是标准Python 3库的一部分。 让我们通过输入以下内容来安装venv：

```shell
sudo apt install -y python3-venv
```

安装此程序后，我们准备创建环境。 让我们选择我们想要放置Python编程环境的目录，或者使用`mkdir`创建一个新目录，如下所示：

```shell
mkdir  /srv/py_env
cd /srv/py_env
python3.7 -m venv my_env #创建环境,本质上，my_venv是一个新目录
```

这些文件一起工作，以确保您的项目与本地计算机的更广泛的上下文隔离开来，从而避免系统文件和项目文件混在一起。 这是进行版本控制并确保您的每个项目都可以访问所需的特定程序包的良好做法。 Python Wheels是Python的一种内置打包格式，可以通过减少所需的编译次数来加快软件生产，它位于Ubuntu 18.04 `share`目录中。



要使用此环境，您需要激活它，可以通过键入以下调用**激活**脚本的命令来实现：

```shell
source my_env/bin/activate
```



现在，您的命令提示符将以您的环境名称为前缀，在本例中，该名称为my_env 。 根据您所运行的Debian Linux版本，您的前缀可能会有所不同，但是在括号中的环境名称应该是您在该行中首先看到的内容：

该前缀使我们知道环境my_env当前处于活动状态，这意味着在此处创建程序时，它们将仅使用此特定环境的设置和程序包。

**注意：**在虚拟环境中，可以根据需要使用命令`python`代替`python3` ，并使用`pip`代替`pip3` 。 如果在环境之外的计算机上使用Python 3，则将需要专门使用`python3`和`pip3`命令。

完成这些步骤后，即可使用虚拟环境。

## 第3步-创建一个“ Hello，World”程序 

现在我们已经建立了虚拟环境，让我们创建一个传统的“ Hello，World！”。

为此，我们将打开一个命令行文本编辑器(例如nano)并创建一个新文件：

```python
print("Hello, World!")
```

```shell
python hello.py
```

要离开环境，只需键入命令`deactivate` ，您将返回到原始目录。



安装jdk

 sudo apt install default-jdk

