# Mac Configuration Memo

参考资料：
https://sourabhbajaj.com/mac-setup/

## 上网配置

软件：Chrome Apple Chip

Clash Pro
https://install.appcenter.ms/users/clashx/apps/clashx-pro/distribution_groups/public

https://docs.dler.io/black-hole/shi-yong-jiao-cheng/macos/clashx

配置方法：
把 YAML 放入/home/.config/clash中，在软件中选中该文件
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
- ezip
- mathpix snip
- rest (currently unavailable)
- 搜狗输入法
- visual studio code
terminal.integrated.fontFamily: 'Source Code Pro for Powerline'
- Foxit Reader


## 环境配置
- homebrew
terminal 中输入命令（暂时不是 iterm，zsh）
- iterm2

```shell
brew install --cask iterm2
```
修改主题

- oh my zsh

字体：
source code pro for powerline

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

先装 zsh-nvm，再装 node lts版本
npm 设置国内镜像

- yarn

https://yarnpkg.com/getting-started/install

```shell
corepack enable
```
# Chrome 插件

NewsReader