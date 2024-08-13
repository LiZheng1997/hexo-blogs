---
title: ARM平台安装Clash
comments: true
toc: true
toc_number: true
copyright_author: Bluet
mathjax: true
date: 2023-06-08 23:29:42
updated: 2023-06-08 23:29:42
tags: [ARM, RaspberryPi, Jetson, Clash]
categories:
- EdgeComupting
- RaspberryPi
keywords: Clash, ARM
description:
top_img:
cover:
copyright_author_href:
copyright_url:
copyright_info:
---

# ***ARM平台安装Clash***

本文介绍如何在ARM平台上安装Clash，比如Jetson开发板、树莓派开发板等

## ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，转载请说明出处引用，感谢！

---

# 背景介绍

需要在ARM架构的主机上安装Clash，实现科学上网，因为经常需要访问些海外的资源，比如拉取docker镜像等等操作。



# 安装步骤

1. 下载Clash的二进制安装包

安装包链接：[__传送门__](https://github.com/Dreamacro/clash/releases) 注意根据自己的操作系统的情况选择合适的二进制包。

下载好后，将clash文件移动到/usr/local/bin下，给予权限。

```text
$sudo mv ./clash /usr/local/bin
$sudo chmod a+x /usr/local/bin/clash
```

2. 编写自己的代理配置文件

根据自己的平台选择对应的clash二进制安装包，比如armv7、armv8、amd64等等，代理的配置文件可以参考我的如下内容：

```text
# port of HTTP
port: 7890

## port of SOCKS5
socks-port: 7891

# `allow-lan` must be true in your config.yml
allow-lan: true

# set log level to stdout (default is info)
# info / warning / error / debug / silent
log-level: info

# A RESTful API for clash
#使用0.0.0.0可以使用局域网设备访问
external-controller: 0.0.0.0:8080

mode: Rule

Proxy:
#以下省略，由梯子的服务商提供

```

Config文件默认放在~/.config/clash这里，将你自己的机场提供的clash配置文件复制到这里就可以

```text
mkdir ~/.config/clash
mv your/clash/config/file config.yaml
mv config.yaml ~/.config/clash
```

除了配置文件，还需要一个全球IP库，Country.mmdb文件，可以实现各个国家的 IP 信息解析和地理定位，没有这个文件 clash 无法正常启动，会报错找不到，这个配置文件也放在默认路径下：~/.config/clash。

3. 运行clash

```text
$ clash
```



我这里希望在Jetson Xavier NX上Clash，因为它的架构是ARMv8的，所以必须下载ARMv8的版本的clash安装包。

ARMv8: [__https://github.com/Dreamacro/clash/releases/download/v1.11.0/clash-linux-armv8-v1.11.0.gz__](https://github.com/Dreamacro/clash/releases/download/v1.11.0/clash-linux-armv8-v1.11.0.gz)

ARMv7: [__https://github.com/Dreamacro/clash/releases/download/v1.11.0/clash-linux-armv7-v1.11.0.gz__](https://github.com/Dreamacro/clash/releases/download/v1.11.0/clash-linux-armv7-v1.11.0.gz)





# 参考引用

[在树莓派上配置Clash-linux](https://zhuanlan.zhihu.com/p/56050058)

[如何在树莓派上使用Clash](https://mraddict.top/posts/clash-on-rpi/)
