---
title: Gogs-学习笔记
tags: [Gogs, notes, 学习笔记]
date: 2021-08-21 10:22:03
categories: notes
---

### 什么是 Gogs?

Gogs 是一款极易搭建的自助 Git 服务。可以根据操作系统平台下载二进制运行，或者通过 Docker 或 Vagrant，以及包管理安装。Gogs 是用 Go 语言开发，因此任何 Go 语言支持的平台都可以运行 Gogs，包括 Windows、Mac、Linux 以及 ARM。一个廉价的树莓派的配置足以满足 Gogs 的最低系统硬件要求。甚至还可以将 Gogs 运行在 NAS 设备上。

Gogs 的目标是打造一个最简单、最快速和最轻松的方式搭建自助 Git 服务。使用 Go 语言开发使得 Gogs 能够通过独立的二进制分发，并且支持 Go 语言支持的 所有平台，包括 Linux、Mac OS X、Windows 以及 ARM 平台。

<!-- more -->

### 安装方式

#### 源文件安装

1. 克隆 Gogs 仓库

```bash
$ git clone --depth 1 https://github.com/gogs/gogs.git gogs
```

2. 进入仓库文件夹下，执行编译

```go
go build -o gogs
```

3. 测试是否安装正确

```sh
./gogs web
```

{% asset_img Gogs运行.png %}

4. 初始配置，Gogs 默认会在端口`3000`启动 HTTP 服务

#### 二进制安装

步骤

1. 解压压缩包。
2. 使用命令 cd 进入到刚刚创建的目录。
3. 执行命令 `./gogs web`。
4. 访问`/install`以进行初始配置。

### 参考资料

- [官方文档](https://gogs.io/docs)
- [历史版本](https://github.com/gogs/gogs/releases)
