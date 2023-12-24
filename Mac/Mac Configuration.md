# Mac Configuration Memo

参考资料：
https://sourabhbajaj.com/mac-setup/

## 上网配置

软件：Chrome Apple Chip

Clash Pro
https://install.appcenter.ms/users/clashx/apps/clashx-pro/distribution_groups/public

https://docs.dler.io/black-hole/shi-yong-jiao-cheng/macos/clashx

配置方法：
把 YAML 放入/home/.config/clash 中，在软件中选中该文件
设置为系统代理

## 常用软件

- sublime
- magnet
- Snipaste
- 小历
- QR Capture
- Manico
- IINA
- GIPHY Capture
- maczip
- mathpix snip
- rest (currently unavailable)
- time out
- 搜狗输入法
- visual studio code
  terminal.integrated.fontFamily: 'Source Code Pro for Powerline'
- Foxit Reader
- Sketch (麦氪搜)

## 环境配置

- homebrew

terminal 中输入命令（暂时不是 iterm，zsh）

将 homebrew 加入 path

```
- Run these two commands in your terminal to add Homebrew to your PATH:
    (echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/jeffrey/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
- Run brew help to get started
- Further documentation:
    https://docs.brew.sh
```

- iterm2

```shell
brew install --cask iterm2
```

iterm2 好像默认就是 zsh

修改主题

- oh my zsh

也可以直接下载 install.sh

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
sh install.sh
```

字体：
source code pro for powerline
安装：https://github.com/powerline/fonts

iterm2 preferences profiles text font 修改

homebrew 在 .zshenv 中配置清华镜像

- vim

```shell
brew install vim
```

- git

```shell
brew install git
```

- Java

```shell
brew install openjdk@8

sudo ln -sfn /usr/local/opt/openjdk@8/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk-8.jdk

echo 'export PATH="/usr/local/opt/openjdk@8/bin:$PATH"' >> ~/.zshrc
```

- Node

先装 zsh-nvm，再装 node lts 版本
npm 设置国内镜像

- yarn

https://yarnpkg.com/getting-started/install

```shell
corepack enable
```

- curl & wget

```shell
brew install wget
brew install curl

curl is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

If you need to have curl first in your PATH, run:
  echo 'export PATH="/usr/local/opt/curl/bin:$PATH"' >> ~/.zshrc

For compilers to find curl you may need to set:
  export LDFLAGS="-L/usr/local/opt/curl/lib"
  export CPPFLAGS="-I/usr/local/opt/curl/include"


zsh completions have been installed to:
  /usr/local/opt/curl/share/zsh/site-functions
```

# Chrome 插件

NewsReader

# VS Code

python 语言支持

新建 Conda 环境
`conda create -n python3.9 python=3.9`
