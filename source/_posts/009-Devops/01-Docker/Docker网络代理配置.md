---
title: Docker网络代理配置
comments: true
toc: true
toc_number: true
copyright_author: Bluet
mathjax: true
date: 2023-06-04 19:06:25
updated: 2023-06-04 19:06:25
tags: Docker
categories: 
- Devops
- Docker
keywords: Docker, 网络代理
description:
top_img:
cover:
copyright_author_href:
copyright_url:
copyright_info:
---

# ***Docker网络代理配置***

之前一直被Docker容器和镜像的科学上网代理所困惑，搞了好久没搞得很清楚，这次把使用经验记录下来，方便回看。

## ***写在前面：***

本博客属于个人学习笔记，希望通过文章记录，规范整理自己的学习内容，方便对知识复习和分享。如果有错误的地方，还请指出，同时转载请说明出处，感谢！

------

# Docker Pull时的代理

这个代理也称为Dockerd 代理，

Docker pull经常碰到拉取的镜像源在国外，导致要科学上网或者延迟高的问题，所以我们需要给docker配置网络代理，方便快速地拉取镜像到本地。（网速的快慢决定了程序员一天的幸福指数）执行如下指令，添加一个代理配置文件。

```text
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo touch /etc/systemd/system/docker.service.d/proxy.conf
```

然后需要修改proxy.conf文件中的代理服务器的内容

```text
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:8080/"
Environment="HTTPS_PROXY=http://proxy.example.com:8080/"
Environment="NO_PROXY=localhost,127.0.0.1,.example.com"
```

记住：这里需要配置的代理服务器可以使你本地的局域网内运行的代理服务器地址或者你本机上运行的代理服务器，比如Clash的本地服务端，并且是不需要密码的代理服务器。

# Container 代理

需要给容器运行时，内部有科学上网的需求的时候，配置容器内部的代理，这个代理将会在容器科学上网时起作用，是一种用户级别的，配置的文件是~/.docker/config.json，只在Docker17.07及以上版本生效，内容如下：

```text
{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://proxy.example.com:8080",
     "httpsProxy": "http://proxy.example.com:8080",
     "noProxy": "localhost,127.0.0.1,.example.com"
   }
 }
}

```

`config.json` 非常方便，默认在所有配置修改后启动的容器生效，适合个人开发环境。但是在CI/CD的自动构建环境、或者实际上线运行的环境中，这种方法就不太合适，用 `-e` 注入这种显式配置会更好，减轻对构建、部署环境的依赖。

# Docker Build 代理

虽然 `docker build` 的本质，也是启动一个容器，但是环境会略有不同，用户级配置无效。在构建时，需要注入 `http_proxy` 等参数。

```text
docker build . \
    --build-arg "HTTP_PROXY=http://proxy.example.com:8080/" \
    --build-arg "HTTPS_PROXY=http://proxy.example.com:8080/" \
    --build-arg "NO_PROXY=localhost,127.0.0.1,.example.com" \
    -t your/image:tag
```

需要注意的是：无论是 `docker run` 还是 `docker build`，默认是网络隔绝的。如果代理使用的是 `localhost:3128` 这类，则会无效。这类仅限本地的代理，必须加上 `--network host` 才能正常使用。而一般则需要配置代理的外部IP，而且代理本身要开启 Gateway 模式。

# 重启生效配置

`docker build` 代理是在执行前设置的，所以修改后，下次执行立即生效。Container 代理的修改也是立即生效的，但是**只针对以后启动的 Container，对已经启动的 Container 无效。**`dockerd` 代理的修改比较特殊，它实际上是改 `systemd` 的配置，因此需要重载 `systemd` 并重启 `dockerd` 才能生效。

```text
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# 参考引用

[如何优雅的给 Docker 配置网络代理_运维之美的博客-CSDN博客](https://blog.csdn.net/easylife206/article/details/114826425)

