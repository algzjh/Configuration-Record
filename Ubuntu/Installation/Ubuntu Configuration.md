# Ubuntu Dual Boot Configuration

下载 Ubuntu 镜像：使用华为云的镜像下载，这样速度更快

https://launchpad.net/ubuntu/+mirror/mirrors.huaweicloud.com-release

验证完整性

```shell
echo "c0d025e560d54434a925b3707f8686a7f588c42a5fbc609b8ea2447f88847041 *ubuntu-18.04.4-desktop-amd64.iso" | shasum -a 256 --check

ubuntu-18.04.4-desktop-amd64.iso: OK
```

制作 Ubuntu 镜像时，U盘被分成了多个区，在 Windows 下，只识别第一个区，即只显示 Ubuntu 引导区的大小。解决办法：https://www.zhihu.com/question/27888608

创建 Bootable USB driver，使用 Rufus

具体过程可以参见：https://ubuntu.com/tutorials/tutorial-create-a-usb-stick-on-windows#1-overview

在 windows 下查看 windows 磁盘分区类型：

win + R 打开运行框，输入 cmd 回车，在 cmd 中输入 diskpart，再输入 list disk，如果 GPT 下带星号则是 GPT 分区表的硬盘。

机械硬盘使用的是 GPT 分区，主板使用 UEFI 模式，所以在 Rufus 中选择 GPT 分区类型，目标系统类型选择 UEFI（非 CSM）

GPT 模式需要采用 DO 写入模式，然后启用 CSM 选择 UEFI USB（这个需要根据自身机器情况进行选择，可以多试几次） 即可

installation 需要一定的时间

Install third-party software for graphics and wifi hardware and additional media formats

https://askubuntu.com/questions/1043763/system-can-not-reboot-after-completing-installation



关闭 windows10 快速启动：系统设置-电源和按钮-电源和睡眠

关闭 security boot



在 Ubuntu 上制作系统启动 U 盘

解决无法格式化的问题



- install third-party software for graphics and wifi hardware and additional media formats

这样可以安装显卡驱动和一些多媒体编码解码软件

- 实验室内部需要配置网络
20.04 使用 netplan 作为网络管理工具
https://segmentfault.com/a/1190000023426677
https://linux265.com/news/6376.html


## partition recommendation

Total: 256 GB
如果需要的话，还可以分配更多空间

UEFI System:

EFI System Partition: 250MB primary

/swap: 8 GB=8192MB  primary

/: 100 GB=102400 MB primary

/home: 160 GB = 163840 MB rest space primary

device for boot loader installation: 选择 EFI System Partition



Legacy BIOS:

/boot: 500MB  ext4 /boot

/swap 8GB

/: 100GB

/home: remain space

## Ubuntu Polishing

https://zhuanlan.zhihu.com/p/61228593

- 日期语言改成 English，reboot
- Ubuntu 换成 Other Server 阿里源，Software & Updates
- 设置阿里 DNS

```shell
sudo vim /etc/resolv.conf
# ipv4
nameserver 223.5.5.5
nameserver 223.6.6.6
dig www.taobao.com +short

# ipv6
nameserver 2400:3200::1
nameserver 2400:3200:baba::1
dig alidns.com
```

- 看 home 是不是单独分开挂载的
- 调整休眠时间
- C++ 环境

https://linuxconfig.org/how-to-install-g-the-c-compiler-on-ubuntu-18-04-bionic-beaver-linux

https://linuxize.com/post/how-to-install-gcc-compiler-on-ubuntu-18-04/

```shell
sudo apt install build-essential
g++ --version
gcc --version
```

- 安装 nvidia 驱动 - 440

```shell
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
```

Graphics llvmpipe (LLVM 9.0, 256 bits)

Software & Updates - Additional Drivers - 440 (open source)，使用 nouveau 的话可能会很卡

Using NVIDIA driver metapackage from nvidia-driver-440 (open source)

```shell
reboot
```

- show hidden files

```shell
ls -a
```

- Vim

```shell
sudo apt update
sudo apt install vim
```

- 支持 exFat，方便 U 盘复制文件

```shell
sudo add-apt-repository universe
sudo apt update
sudo apt install exfat-fuse exfat-utils
```

- 安装 zip, unzip

```
sudo apt-get install zip unzip
```

- 安装 unrar

```shell
sudo apt-get install unrar
sudo apt-get install rar
unrar x tecmint.rar
rar a tecmint.rar tecmint
```

- Java 环境

https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04

https://linuxize.com/post/install-java-on-ubuntu-18-04/

https://www.linuxuprising.com/2019/06/new-oracle-java-11-installer-for-ubuntu.html

```shell
sudo mkdir -p /var/cache/oracle-jdk11-installer-local/
sudo cp jdk-11.0.6_linux-x64_bin.tar.gz /var/cache/oracle-jdk11-installer-local/

sudo apt update
sudo apt-get upgrade
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-installer-local
java -version
```

java

auto mode

manual mode

`sudo update-alternatives --config java`

`sudo update-alternatives --config javac`

```shell
There are 2 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
  0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      auto mode
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      manual mode
* 2            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode


There are 2 choices for the alternative javac (providing /usr/bin/javac).

  Selection    Path                                          Priority   Status
------------------------------------------------------------
  0            /usr/lib/jvm/java-11-openjdk-amd64/bin/javac   1111      auto mode
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/javac   1111      manual mode
* 2            /usr/lib/jvm/java-8-openjdk-amd64/bin/javac    1081      manual mode
```

配置：

.bash_profile

.bashrc：executed for non-login shells

.profile：executed for login shells, this file is not read by bash, if .bash_profile or .bash_login exists

Pay Attention！

```shell
sudo update-alternatives --config java
sudo vim /etc/environment
JAVA_HOME="/usr/lib/jvm/java-11-oracle"
source /etc/environment
echo $JAVA_HOME
/usr/lib/jvm/java-11-oracle
```

安装 OpenJDK
更加方便

```shell
sudo apt-get install openjdk-8-jdk
java -version

JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64/jre/bin"
```

- anaconda

在 tuna 镜像中下载，避免网络过慢，https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/

按照官网教程 https://docs.anaconda.com/anaconda/install/linux/ 进行安装

```shell
bash ~/Downloads/Anaconda3-2020.02-Linux-x86_64.sh
source ~/.bashrc
conda config --set auto_activate_base false
```

更改 pypi 源和 conda 源，具体参见 tuna 使用帮助

```shell
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U

pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
vim .condarc
conda config --set show_channel_urls yes
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
```

- Conda Python2 Env

```shell
conda clean -i
conda create -n python2 python=2.7 anaconda
```

- 安装 Terminator (提前)

```shell
sudo apt-get update
sudo apt-get install terminator
sudo update-alternatives --config x-terminal-emulator
```

- 安装 deepin-terminal

```shell
sudo apt-get update
sudo apt-get install deepin-terminal
# Register deepin terminal
sudo update-alternatives --install /usr/bin/x-terminal-emulator x-terminal-emulator /usr/bin/deepin-terminal 50

# Select your terminal
sudo update-alternatives --config x-terminal-emulator
```

- git

安装 git

```shell
sudo apt update
sudo apt install git
git --version
git config --global core.quotepath false # 解决 git status 中文乱码 
```

- curl & wget

```
sudo apt install curl
sudo apt install wget
```

- zsh

```shell
sudo apt install zsh
zsh --version
chsh -s $(which zsh)
log out and login back again
echo $SHELL
```

zsh newuser config
`~/.zshrc`

安装 oh my zsh
https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh
sh install.sh


配置：
.zshrc
https://medium.com/@abhinavkorpal/bash-profile-vs-bashrc-c52534a787d3

安装字体
```
# clone
git clone https://github.com/powerline/fonts.git --depth=1
# install
cd fonts
./install.sh
# clean-up a bit
cd ..
rm -rf fonts
```

```
sudo apt-get install fonts-powerline
pip install --user powerline-status
sudo apt install powerline
download from github,https://github.com/powerline/fonts
./install.sh
Powerline fonts installed to /home/zjlab/.local/share/fonts
```

只显示username，在zsh中添加
```shell
prompt_context() {
  if [[ "$USER" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
    prompt_segment black default "%(!.%{%F{yellow}%}.)$USER"
  fi
}
```

zsh-newuser-install

.zshenv

.zprofile

.zshrc

.zlogin

- VS Code
https://code.visualstudio.com/docs/setup/linux

sudo apt install ./<file>.deb

- VS Code terminal powerline
terminal.integrated.fontFamily: 'DejaVu Sans Mono for Powerline'

- 安装 Krusader

```shell
apt-get install krusader
```

- 安装 deb

```shell
sudo apt install ./name.deb # 推荐
or
sudo dpkg -i /path/to/deb/file
```

- 删除 deb

```shell
sudo apt purge "software name"
```

- 安装 Chrome，同步 Google 账号信息

make google chrome the default browser

```shell
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install ./google-chrome-stable_current_amd64.deb
```

- Qv2ray

下载Qray
https://github.com/Qv2ray/Qv2ray/releases
Qv2ray_2.5.0-1_amd64.deb
Qv2ray.v.2.5.0.linux-x64.AppImage

sudo apt install ./qv2ray_2.5.0-1_amd64.deb
/home/username/.config/qv2ray/vcore

下载 v2ray-core

https://github.com/v2ray/v2ray-core/releases

https://qv2ray.net/getting-started/step2.html#download-v2ray-core-files

chmod +x v2ray

chmod +x v2ctl

V2ray path configuration check passed.
Current version of V2ray is: 
V2Ray 4.25.1 (V2Fly, a community-driven edition of V2Ray.) Custom (go1.14.4 linux/amd64)

Tag: outBound_PROXY
Host: 地址
Port: 端口
Type: VMess
UUID:
AlterId: 64
Security加密方式: auto
Protocol Settings-Transport Protocol: ws
Protocol Settings-Path: /api/v3/download.getFile
Protocol Settings-Headers: Host|ws-host

Qv2ray Debian Repository

http://qv2ray.net/debian/

FastGit Mirror

```shell
# Install some prerequisites needed by adding GPG public keys
$ sudo apt-get install gnupg ca-certificates

# Import our GPG key:
$ curl -sS https://qv2ray.github.io/debian/pubkey.gpg | sudo apt-key add -

# Add the our official APT repository:
$ echo "deb https://qv2ray.github.io/debian/ stable main" | sudo tee /etc/apt/sources.list.d/qv2ray.list

# echo "deb https://qv2ray.github.io/debian/ unstable main" | sudo tee /etc/apt/sources.list.d/qv2ray.list (for debian testing/unstable and Ubuntu 20.10+)

# To update the APT index:
$ sudo apt-get update

# You can install Qv2ray from APT now:
$ sudo apt-get install qv2ray
```

Qv2ray Groups 填入订阅地址

SwitchyOmega 配置


- terminal proxy

zshrc 中写入

```shell
alias setproxy="export http_proxy=http://127.0.0.1:8889 https_proxy="http://127.0.0.1:8889"
alias unsetproxy="unset http_proxy https_proxy"
```

socks5好像有点问题，要 http
每次要用的时候输入setproxy，不用了就unsetproxy。

测试`curl cip.cc`

- nvm

oh my zsh 安装插件：
https://github.com/lukechilds/zsh-nvm

```
git clone https://github.com/lukechilds/zsh-nvm ~/.oh-my-zsh/custom/plugins/zsh-nvm
# .zshrc 中 plugins 添加 zsh-nvm
source ~/.zshrc
```

https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04

nvm 国内镜像

```
# 添加入 zshrc
export NVM_NODEJS_ORG_MIRROR=http://npm.taobao.org/mirrors/node
```

nvm、node、npm 区别：
https://www.jianshu.com/p/db8af76ff9ae

设置nodejs镜像和npm镜像

```
npm config set registry https://registry.npm.taobao.org
npm config get registry
```

https://github.com/nvm-sh/nvm

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash
# or
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash

chmod +x install.sh
sudo ./install.sh

# add to ~/.zshrc
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm

export NVM_NODEJS_ORG_MIRROR=http://npm.taobao.org/mirrors/node

# Calling nvm use automatically in a directory with a .nvmrc file

# place this after nvm initialization!
autoload -U add-zsh-hook
load-nvmrc() {
  local node_version="$(nvm version)"
  local nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")

    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$node_version" ]; then
      nvm use
    fi
  elif [ "$node_version" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc

# verify installation
command -v nvm
```

- node and npm

```shell
sudo apt-get update
sudo apt-get install nodejs
sudo apt-get install npm
node -v
npm -v
npm config set registry https://registry.npm.taobao.org
npm config get registry
sudo npm install -g npm@latest
```


https://github.com/nodesource/distributions/blob/master/README.md#debinstall
```
# Using Ubuntu
curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs

## Run `sudo apt-get install -y nodejs` to install Node.js 12.x and npm
## You may also need development tools to build native addons:
     sudo apt-get install gcc g++ make
## To install the Yarn package manager, run:
     curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
     echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
     sudo apt-get update && sudo apt-get install yarn

```

- yarnpkg

https://stackoverflow.com/questions/52174381/yarn-not-installing-in-nvm-version-node-version

npm install -g yarn

yarn --version

yarn config set registry https://registry.npm.taobao.org

yarn config get registry

配置 yarn global bin 路径到环境变量

```shell
# in zshrc
export PATH="$PATH:$(yarn global bin)"
```

- TypeScript

```shell
npm install -g typescript
tsc -v
```

- golang

```shell
sudo tar -C /usr/local -xzf go$VERSION.$OS-$ARCH.tar.gz
sudo vim /etc/profile
Add /usr/local/go/bin to the PATH environment variable
export PATH=$PATH:/usr/local/go/bin
source $HOME/.profile.
```

- ruby

```shell
sudo apt-get install ruby-full
ruby -v
sudo apt install ruby-railties
rails --version
```

- typescript

```shell
npm install -g typescript
```

- 珂雪尚往，参见 monocloud 的指南，Network Proxy Settings - Manual

http://shadowsocks.org/en/download/servers.html

https://github.com/shadowsocks/shadowsocks-qt5

（CSDN：Install Chrome Extensions）

```
apt-install git
mkdir ~/SSR
cd ~/SRR
git clone https://github.com/shadowsocksrr/shadowsocksr.git
conda activate python2
sudo apt install python-openssl
sudo apt install libcanberra-gtk-module libcanberra-gtk3-module gconf2 gconf-service libappindicator1
sudo apt-get install libssl-dev
sudo apt-get install libsodium-dev
sudo dpkg -i electron-ssr-0.2.7.deb
```

Setting Network Proxy Manual

127.0.0.1   12333

127.0.0.1   12333

127.0.0.1    1080

ignore hosts: localhost, 127.0.0.0/8, ::1

SwitchyOmega Option 中只需要设置 socks5

https://github.com/qingshuisiyuan/electron-ssr-backup/blob/master/Ubuntu.md

出错看 log，记得重启

```shell
sudo apt install python-openssl
```

https://github.com/shadowsocksrr/electron-ssr/issues/25

```shell
export http_proxy="socks5://127.0.0.1:1080"
```

- 搜狗输入法

https://pinyin.sogou.com/linux/help.php

sogou linux 官网下载 deb

安装

```shell
sudo apt-get install fcitx
sudo apt install ./sogoupinyin_2.3.1.0112_amd64.deb
# sudo dpkg -i sogoupinyin_版本号_amd64.deb
# sudo apt -f install
```

input sources 只选 English(US)

Manage Installed Languages中的 language 只选 English (United States) 和 English，不用选汉语，Keyboard input method system 选择 fcitx 

Settings-Region & Language - Manage Installed Languages - Keyboard input method system - fcitx 

Apply System Wide

reboot

右上角键盘选择 configure，增加 sogou 输入法，bingo

- ibus 中文输入法

https://blog.csdn.net/wu10188/article/details/86540464

- Visual Studio Code
- 解压

```shell
tar -xzf archive.tar.gz
tar -xjf archive.tar.bz2
```

- CLion

```shell
tar -xzf archive.tar.gz
```

https://askubuntu.com/questions/25961/how-do-i-install-a-tar-gz-or-tar-bz2-file

- PyCharm

记住别把含有.sh 的文件夹删了，最好单独装到 home 下的一个文件夹

先安装好 JDK 以及 G++ 等环境

create shortcut

https://askubuntu.com/questions/391439/how-can-i-set-up-pycharm-to-launch-from-the-launcher

Settings 配置：

Theme：Darcula

Font：DejaVu Sans Mono，monospace

Size：17

Python Interpreter

```shell
import sys
print(sys.executable)
/home/zjvis/anaconda3/bin/python
```

available for all projects

输入中文：https://zhuanlan.zhihu.com/p/81182213

- IntelliJ

- Ubuntu 设置终端走代理

https://www.windsings.com/posts/3c42339d/

```
export ALL_PROXY=socks5://127.0.0.1:1080
source ~/.zshrc

alias setproxy="export ALL_PROXY=socks5://127.0.0.1:1080" 
alias unsetproxy="unset ALL_PROXY"
```

- Sublime

https://www.sublimetext.com/docs/3/linux_repositories.html

- Typora：比较慢

https://typora.io/#linux

- 删除软件

```shell
dpkg --list
sudo apt-get remove <qpplication_name>
sudo apt-get purge <package-name>
```

- 通过 ssh 访问局域网中的 Ubuntu

1. 在 Ubuntu 中安装

   `sudo apt-get install openssh-server`

   `sudo apt-get install openssh-client`

2. 配置 sshd_config 文件

   `sudo vim /etc/ssh/sshd_config`

```shell
Port 22
PermitRootLogin yes
PasswordAuthentication yes
```

3. 启动 ssh 服务
   `sudo service ssh start`
   可以通过以下命令查看 ssh 服务是否在运行
   `ps -aux | grep ssh`
   `sudo apt install net-tools`
   `netstat -a | grep ssh`

4. 远程连接
   先利用`ifconfig`查看 Ubuntu 在局域网中的 IP 地址，然后在 Mac 中打开 Terminal，输入
   `ssh rootname@ip_address`
   例如，`ssh root@192.168.2.3`
   输入密码，即可进入

- tmux

```shell
sudo apt-get install tmux
tmux 
exit
tmux ls
```

- sshfs

挂载服务器上的目录到本地

```shell
sudo apt-get install sshfs
```

- cuda 安装

http://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html

https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=deblocal

~/.profile
```shell
export PATH="$PATH:/usr/local/cuda-10.2/bin"
```

- Home-Templates 添加

常用 CSVDocument.csv, HTMLPage.html, Markdown.md, NewTextFile.txt, PythonScript.py, ShellScript.sh

- 安装 filezilla

```shell
sudo apt update
sudo apt install filezilla
```

- 安装 Postman

```shell
snap install postman
# sudo snap install postman
```
too slow

- Latex

```shell
sudo apt-get install texlive-full
```

- texstudio

```shell
sudo add-apt-repository ppa:sunderme/texstudio
sudo apt-get update

# ustc镜像有ppa的反向代理
/etc/apt/sources.list.d/sunderme-ubuntu-texstudio-focal.list
# 只需要将其中的http://ppa.launchpad.net替换为http://launchpad.proxy.ustclug.org就行。
sudo apt-get update
sudo apt-get install texstudio
```


- GNOME Dictionary

```shell
sudo snap install gnome-dictionary
```

- GeoGebra Classic 6

Linux (deb) 64 bit installer for .deb based systems

https://wiki.geogebra.org/en/Reference:GeoGebra_Installation

```shell
sudo apt-add-repository -u 'deb http://www.geogebra.net/linux/ stable main' 
sudo apt install geogebra-classic 
```

- 卸载 LibreOffice，安装 WPS

```shell
sudo apt-get remove --purge 'libreoffice*'
sudo apt-get clean
sudo apt-get autoremove
```

WPS 缺失字体：https://blog.desdelinux.net/en/error-wps-office-missing-font-wingdings-wingdings-2-wingdin-solucionado-en-linux-mint/

```shell
sudo fc-cache -vfs
```

- 安装 MySQL、MongoDB
- Docker 安装
- HDFS 安装

```shell
sudo apt update
sudo apt install mysql-server
sudo mysql_secure_installation
```

Adjusting User Authentication and Privileges.
```shell
sudo mysql
SELECT user,authentication_string,plugin,host FROM mysql.user;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
FLUSH PRIVILEGES;
SELECT user,authentication_string,plugin,host FROM mysql.user;
exit
```

login 

```sql
mysql -u root -p
select current_user();
```

密码政策：
```sql
SHOW VARIABLES LIKE 'validate_password%';
SET GLOBAL validate_password.policy=LOW;
SELECT user FROM mysql.user;
GRANT ALL PRIVILEGES ON *.* TO 'zjh_dev'@'localhost' WITH GRANT OPTION;
exit
```

改密码：

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassword';
```

Testing MySQL

```shell
systemctl status mysql.service
```

If MySQL isn’t running, you can start it with `sudo systemctl start mysql`.

using the mysqladmin tool

```shell
sudo mysqladmin -p -u root version
```

- 安装 mysql workbench
https://dev.mysql.com/doc/workbench/en/wb-installing-linux.html

https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/

sudo apt-get install mysql-workbench-community

- 安装 navicat 15
https://buxuhunao.com/article/how-to-use-navicat15-in-ubuntu.html


- 安装 Hadoop、Yarn、Spark

- 安装 gnome-sushi 预览

- 安装 RescureTime

Ubuntu Usage:
After installing the package, open terminal and run the command `rescuetime`, and enter your email

- Environment Variable

Session-wide:

`~/.pam_environment`

`~/.profile`: 优先使用

```shell
export FOO=bar
export PATH="$PATH:$HOME/MyPrograms"
```

Other files
`~/.bashrc`： non-login shells
`~/.bash_profile`
`~/.bash_login`

System-wide environment variables

`/etc/environment`

```shell
FOO=bar
```

**Note:** Variable expansion does not work in `/etc/environment`

`/etc/profile.d/*.sh`

Files with the .sh extension in the `/etc/profile.d` directory get executed whenever a bash login shell is entered (e.g. when logging in from the console or over ssh), as well as by the DisplayManager when the desktop session loads.

You can for instance create the file `/etc/profile.d/myenvvars.sh` and set variables like this:

```shell
export JAVA_HOME=/usr/lib/jvm/jdk1.7.0
export PATH=$PATH:$JAVA_HOME/bin
```

- 安装 mathpix

```shell
sudo snap install mathpix-snipping-tool
```

- 中文字体显示问题

https://xlee00.github.io/2019/03/26/Ubuntu%E4%B8%8A-%E9%97%A8-%E5%A4%8D-%E5%AD%97%E6%98%BE%E7%A4%BA%E4%B8%8D%E6%AD%A3%E7%A1%AE/

```shell
sudo vim 64-language-selector-prefer.conf
```

- tree
sudo apt-get install tree

- flameshot

sudo apt install flameshot

- 安装录屏软件

sudo apt install kazam

sudo apt install simplescreenrecorder

- github CLI

https://github.com/cli/cli/blob/trunk/docs/install_linux.md

```shell
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key C99B11DEB97541F0
sudo apt-add-repository https://cli.github.com/packages
sudo apt update
sudo apt install gh
```

- mendeley

https://www.mendeley.com/download-desktop-new/#download

- nutstore
install from source code
https://www.jianguoyun.com/s/downloads/linux

```shell
sudo dpkg -i nautilus_nutstore_amd64.deb
sudo apt-get install -f
```

- Zotero

https://github.com/retorquere/zotero-deb

preferences 中的Sync 登录 Zotero 账号，取消 File Syncing 中的选中状态，Export 中选择 IEEE，Advanced 中的 Files and Folders 中选择 Linked Attachment Base Directory 中的 Base directory 设置为Nutstore Files/zotero，Data Directory Location 选择 Default。

安装 zotfile 插件
打开 Zotfile Preferences，Source Folder for Attaching New Files 中设置为 Downloads，Location of Files 设置为 Custom Location：Nutstore Files-zotero，

```shell
wget -qO- https://github.com/retorquere/zotero-deb/releases/download/apt-get/install.sh | sudo bash
sudo apt update
sudo apt install zotero
```

- JASP
https://jasp-stats.org/download/

- Jamovi
https://www.jamovi.org/download.html

- Xmind
https://www.xmind.net/

`sudo snap install xmind`


- 百度网盘

https://pan.baidu.com/download#pan

- safe eyes

https://slgobinath.github.io/SafeEyes/

手动添加 ppa （上面成功了就不用）

https://blog.zackad.dev/en/2017/08/17/add-ppa-simple-way.html


- Spotify

https://www.spotify.com/us/download/linux/
需要设置 proxy

- netease music

https://music.163.com/#/download

- qq music

https://y.qq.com/download/download.html

- snap & snap-store

```shell
sudo apt update
sudo apt install snapd
sudo snap install snap-store
```

- vlc

```shell
sudo snap install vlc
```

## Other Problems

输入密码后不能进入桌面

https://askubuntu.com/questions/1086367/ubuntu-18-04-login-window-loop
