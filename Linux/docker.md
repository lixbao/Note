# Linux下安装docker

> 系统：Debian 10.5 （阿里云）
>
> 工具：Xshell 6 (Build 0206)

## 先决条件

### 操作系统要求

要安装 Docker Engine，您需要以下 Debian 或 Raspbian 版本之一的 64 位版本：

- Debian Bullseye 11（测试）
- Debian Buster 10（稳定版）
- Raspbian Bullseye 11（测试）
- Raspbian Buster 10（稳定版）

Docker Engine 在`x86_64`（或`amd64`）`armhf`、 和`arm64`架构上受支持。

### 卸载旧版本

```
$ sudo apt-get remove docker docker-engine docker.io containerd runc
```

## 安装方法

您可以根据需要以不同方式安装 Docker Engine：

- 大多数用户 [设置 Docker 的存储库](https://docs.docker.com/engine/install/debian/#install-using-the-repository)并从中安装，以便于安装和升级任务。这是推荐的方法，除了 Raspbian。
- 一些用户下载 DEB 包并 [手动安装](https://docs.docker.com/engine/install/debian/#install-from-a-package)并完全手动管理升级。这在诸如在无法访问互联网的气隙系统上安装 Docker 等情况下非常有用。
- 在测试和开发环境中，一些用户选择使用自动化的 [便捷脚本](https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script)来安装 Docker。这是目前 Raspbian 的唯一方法。

### 使用存储库安装

在新主机上首次安装 Docker Engine 之前，您需要设置 Docker 存储库。之后，您可以从存储库安装和更新 Docker。

> **Raspbian 用户不能使用这种方法！**
>
> 对于 Raspbian，尚不支持使用存储库进行安装。您必须改为使用[便利脚本](https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script)。

#### 设置存储库

1. 更新`apt`包索引并安装包以允许`apt`通过 HTTPS 使用存储库：

   ```
   $ sudo apt-get update
   
   $ sudo apt-get install \
       apt-transport-https \
       ca-certificates \
       curl \
       gnupg \
       lsb-release
   ```

2. 添加Docker官方的GPG密钥：

   ```
   $ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

3. 使用以下命令设置**稳定**存储库。要添加 **每晚**或**测试**存储库，请在以下命令中的单词后添加单词`nightly`或`test`（或两者）`stable`。[了解**nightly**和**test**频道](https://docs.docker.com/engine/install/)。

   > **注意**：下面的`lsb_release -cs`子命令会返回您的 Debian 发行版的名称，例如`helium`. 有时，在像 BunsenLabs Linux 这样的发行版中，您可能需要更改`$(lsb_release -cs)` 为您的父 Debian 发行版。例如，如果您使用的是 `BunsenLabs Linux Helium`，则可以使用`stretch`. Docker 不对未经测试和不受支持的 Debian 发行版提供任何保证。

   - x86_64 / amd64
   - 阿姆哈夫
   - arm64

   ```
   $ echo \
     "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

   

#### 安装 Docker 引擎

此过程适用于Debian的上`x86_64`/ `amd64`，`armhf`，`arm64`，和Raspbian。

1. 更新`apt`包索引，安装*最新版本*的Docker Engine和containerd，或者到下一步安装特定版本：

   ```
    $ sudo apt-get update
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io
   ```

   > 有多个 Docker 存储库？
   >
   > 如果您启用了多个 Docker 存储库，则在`apt-get install`或 `apt-get update`命令中未指定版本的情况下安装或更新始终会安装可能的最高版本，这可能不适合您的稳定性需求。

2. 要安装*特定版本*的 Docker Engine，请在 repo 中列出可用版本，然后选择并安装：

   一种。列出您的存储库中可用的版本：

   ```
   $ apt-cache madison docker-ce
   
     docker-ce | 5:18.09.1~3-0~debian-stretch | https://download.docker.com/linux/debian stretch/stable amd64 Packages
     docker-ce | 5:18.09.0~3-0~debian-stretch | https://download.docker.com/linux/debian stretch/stable amd64 Packages
     docker-ce | 18.06.1~ce~3-0~debian        | https://download.docker.com/linux/debian stretch/stable amd64 Packages
     docker-ce | 18.06.0~ce~3-0~debian        | https://download.docker.com/linux/debian stretch/stable amd64 Packages
   ```

   湾 使用第二列中的版本字符串安装特定版本，例如`5:18.09.1~3-0~debian-stretch `.

   ```
   $ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
   ```

3. 通过运行`hello-world` 映像验证 Docker Engine 是否已正确安装。

   ```
   $ sudo docker run hello-world
   ```

   此命令下载测试映像并在容器中运行它。当容器运行时，它会打印一条消息并退出。

Docker 引擎已安装并正在运行。该`docker`组已创建，但未向其中添加任何用户。您需要使用`sudo`来运行 Docker 命令。继续[Linux postinstall](https://docs.docker.com/engine/install/linux-postinstall/)以允许非特权用户运行 Docker 命令和其他可选配置步骤。

#### 升级 Docker 引擎

要升级 Docker Engine，首先运行`sudo apt-get update`，然后按照 [安装说明](https://docs.docker.com/engine/install/debian/#install-using-the-repository)，选择要安装的新版本。

### 从包安装

如果您无法使用 Docker 的存储库来安装 Docker 引擎，您可以下载该`.deb`版本的 文件并手动安装。每次要升级 Docker 时都需要下载一个新文件。

1. 转到[`https://download.docker.com/linux/debian/dists/`](https://download.docker.com/linux/debian/dists/)，选择你的Debian版本，然后浏览`pool/stable/`，选择`amd64`， `armhf`或`arm64`，并下载`.deb`文件要安装多克尔引擎版本。

   > **笔记**
   >
   > 要安装**每晚**或**测试**（预发布）包，`stable`请将上述 URL 中的单词更改为`nightly`或`test`。 [了解**nightly**和**test**频道](https://docs.docker.com/engine/install/)。

2. 安装 Docker Engine，将下面的路径更改为您下载 Docker 包的路径。

   ```
   $ sudo dpkg -i /path/to/package.deb
   ```

   Docker 守护进程会自动启动。

3. 通过运行`hello-world` 映像验证 Docker Engine 是否已正确安装。

   ```
   $ sudo docker run hello-world
   ```

   此命令下载测试映像并在容器中运行它。当容器运行时，它会打印一条消息并退出。

Docker 引擎已安装并正在运行。该`docker`组已创建，但未向其中添加任何用户。您需要使用`sudo`来运行 Docker 命令。继续[Linux 的安装后步骤](https://docs.docker.com/engine/install/linux-postinstall/)以允许非特权用户运行 Docker 命令和其他可选配置步骤。

#### 升级 Docker 引擎

要升级 Docker Engine，请下载更新的包文件并重复 [安装过程](https://docs.docker.com/engine/install/debian/#install-from-a-package)，指向新文件。

### 使用便利脚本安装

Docker 在[get.docker.com](https://get.docker.com/) 上提供了一个方便的脚本，可以快速且非交互地将 Docker 安装到开发环境中。不建议将便捷脚本用于生产环境，但可以用作示例来创建适合您需求的配置脚本。另请参阅[使用存储库安装](https://docs.docker.com/engine/install/debian/#install-using-the-repository) 步骤以了解使用软件包存储库进行安装的安装步骤。该脚本的源代码是开源的，可以[`docker-install`在 GitHub 上](https://github.com/docker/docker-install)的 [存储库中找到](https://github.com/docker/docker-install)。

在本地运行之前，请务必检查从 Internet 下载的脚本。在安装之前，让自己熟悉便利脚本的潜在风险和限制：

- 脚本需要`root`或`sudo`特权才能运行。
- 该脚本尝试检测您的 Linux 发行版和版本并为您配置包管理系统，并且不允许您自定义大多数安装参数。
- 该脚本无需确认即可安装依赖项和建议。这可能会安装大量软件包，具体取决于主机的当前配置。
- 默认情况下，该脚本会安装 Docker、containerd 和 runc 的最新稳定版本。使用此脚本配置机器时，可能会导致 Docker 的主要版本意外升级。在部署到生产系统之前，始终在测试环境中测试（主要）升级。
- 该脚本并非旨在升级现有的 Docker 安装。使用脚本更新现有安装时，依赖项可能不会更新到预期版本，从而导致使用过时的版本。

> 提示：运行前预览脚本步骤
>
> 您可以运行带有`DRY_RUN=1`选项的脚本以了解脚本在安装过程中将执行的步骤：
>
> ```
> $ curl -fsSL https://get.docker.com -o get-docker.sh
> $ DRY_RUN=1 sh ./get-docker.sh
> ```

此示例从[get.docker.com](https://get.docker.com/)下载脚本 并运行它以在 Linux 上安装 Docker 的最新稳定版本：

```
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
Executing docker install script, commit: 7cae5f8b0decc17d6571f9f52eb840fbc13b2737
<...>
```

安装了 Docker。该`docker`服务在基于 Debian 的发行版上自动启动。在`RPM`基于发行版的发行版上，例如 CentOS、Fedora、RHEL 或 SLES，您需要使用适当的`systemctl`or`service`命令手动启动它。如消息所示，默认情况下，非 root 用户无法运行 Docker 命令。

> **以非特权用户身份使用 Docker，还是以无根模式安装？**
>
> 安装脚本需要`root`或`sudo`具有安装和使用 Docker 的权限。如果要授予非 root 用户访问 Docker 的权限，请参阅 [Linux 的安装后步骤](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)。Docker 也可以在没有`root`特权的情况下安装，或配置为在无根模式下运行。有关在无根模式下运行 Docker 的说明，请参阅以 [非 root 用户身份运行 Docker 守护进程（无根模式）](https://docs.docker.com/engine/security/rootless/)。

#### 安装预发行版

Docker 还在[test.docker.com](https://test.docker.com/) 上提供了一个方便的脚本，用于在 Linux 上安装 Docker 的预发布版本。此脚本等效于 中的脚本`get.docker.com`，但会配置您的包管理器以启用我们包存储库中的“测试”通道，其中包括 Docker 的稳定版和预发布版（测试版、候选发布版）。使用此脚本可以提前访问新版本，并在它们稳定发布之前在测试环境中对其进行评估。

要从“测试”频道在 Linux 上安装最新版本的 Docker，请运行：

```
$ curl -fsSL https://test.docker.com -o test-docker.sh
$ sudo sh test-docker.sh
<...>
```

#### 使用便利脚本后升级 Docker

如果您使用便利脚本安装 Docker，则应直接使用您的包管理器升级 Docker。重新运行便利脚本没有任何好处，如果它尝试重新添加已经添加到主机的存储库，它可能会导致问题。

## 卸载 Docker 引擎

1. 卸载 Docker Engine、CLI 和 Containerd 包：

   ```
   $ sudo apt-get purge docker-ce docker-ce-cli containerd.io
   ```

2. 主机上的映像、容器、卷或自定义配置文件不会自动删除。删除所有镜像、容器和卷：

   ```
   $ sudo rm -rf /var/lib/docker
   $ sudo rm -rf /var/lib/containerd
   ```

您必须手动删除任何已编辑的配置文件。

## 下一步

- 继续[Linux 的安装后步骤](https://docs.docker.com/engine/install/linux-postinstall/)。
- 查看[使用 Docker 开发中](https://docs.docker.com/develop/)的主题，了解如何使用 Docker 构建新应用程序。

[要求](https://docs.docker.com/search/?q=requirements)，[apt](https://docs.docker.com/search/?q=apt)，[安装](https://docs.docker.com/search/?q=installation)，[debian](https://docs.docker.com/search/?q=debian)，[安装](https://docs.docker.com/search/?q=install)，[卸载](https://docs.docker.com/search/?q=uninstall)，[升级](https://docs.docker.com/search/?q=upgrade)，[更新](https://docs.docker.com/search/?q=update)https://docs.docker.com/search/?q=update)