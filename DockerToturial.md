## Quick start of Docker
[原文 -> Github](https://github.com/WenruiShen/MyQuant)

[转载请注明！]

Author: Shen Wenrui

Email:  Thomas.shen3904@qq.com


### 1 Installation:
[链接][MacOS Docker 安装](http://www.runoob.com/docker/macos-docker-install.html)

> brew cask install docker

检查安装后的 Docker 版本。

> docker --version

镜像加速:

鉴于国内网络问题，后续拉取 Docker 镜像十分缓慢，我们可以需要配置加速器来解决，我使用的是网易的镜像地址：
http://hub-mirror.c.163.com。

在任务栏点击 ``Docker for mac`` 应用图标 -> ``Perferences...`` -> ``Daemon`` -> ``Registry mirrors``。
在列表中填写加速器地址即可。修改完成之后，点击 ``Apply & Restart`` 按钮，
Docker 就会重启并应用配置的镜像地址了。

可以通过 docker info 来查看是否配置成功。

> docker info
    
    ...
    Registry Mirrors:
     http://hub-mirror.c.163.com
    Live Restore Enabled: false


### 2 Basic commends:










### 3 References:
[1].[Docker 教程 (菜鸟学院)](http://www.runoob.com/docker/docker-tutorial.html)