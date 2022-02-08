# Server配置

Ubuntu Server Guide: https://ubuntu.com/server/docs?_ga=2.107390594.1020565921.1642407740-1967348523.1620352327

## 镜像
http://mirrors.aliyun.com/ubuntu

## nvidia 驱动
https://phoenixnap.com/kb/install-nvidia-drivers-ubuntu

## 避免更新为desktop
https://linuxconfig.org/how-to-disable-enable-gui-on-boot-in-ubuntu-20-04-focal-fossa-linux-desktop

```shell
# sudo apt-get install --no-install-recommends ubuntu-desktop
sudo apt-get remove gnome-shell

sudo apt-get remove gnome 

sudo apt-get autoremove
sudo apt-get purge gnome
```

## 终端上网

Clash Premium

