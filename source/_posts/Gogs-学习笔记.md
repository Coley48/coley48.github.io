---
title: Gogs-学习笔记
tags: [Gogs, notes, 学习笔记]
date: 2021-09-05 10:22:03
categories: notes
---

### 什么是 Gogs?

Gogs 是一款极易搭建的自助 Git 服务。可以根据操作系统平台下载二进制运行，或者通过 Docker 或 Vagrant，以及包管理安装。Gogs 是用 Go 语言开发，因此任何 Go 语言支持的平台都可以运行 Gogs，包括 Windows、Mac、Linux 以及 ARM。一个廉价的树莓派的配置足以满足 Gogs 的最低系统硬件要求。甚至还可以将 Gogs 运行在 NAS 设备上。

Gogs 的目标是打造一个最简单、最快速和最轻松的方式搭建自助 Git 服务。使用 Go 语言开发使得 Gogs 能够通过独立的二进制分发，并且支持 Go 语言支持的 所有平台，包括 Linux、Mac OS X、Windows 以及 ARM 平台。

<!-- more -->

### 环境要求

数据库（选择以下一项）：

- MySQL：版本 >= 5.7
- PostgreSQL
- TiDB（实验性支持，使用 MySQL 协议连接）
- 或者什么都不安装，直接使用 SQLite3

Git（bash）：

- 服务端和客户端均需版本 >= 1.8.3
- Windows 系统建议使用最新版

SSH 服务器：

- 如果您只使用 HTTP/HTTPS 的话请忽略此项
- 如果您选择在 Windows 系统使用内置 SSH 服务器，请确保添加`ssh-keygen`到您的`%PATH%`环境变量中
- 推荐 Windows 系统使用 Cygwin OpenSSH 或 Copssh
- Windows 系统 请确保 Bash 是默认的 Shell 程序，而不是 PowerShell

### 准备工作

1. 安装好 Git，MySQL，NSSM；
2. 新增 git 系统用户，并修改其权限，具体可以参考[这篇](https://o-r.github.io/2018/05/12/Gogs%E5%9C%A8windows%E4%B8%8A%E7%9A%84%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE/)；

版本信息：

- Git：`2.29.2`；
- MySQL：`15.1`；
- NSSM：`2.24`；

其中数据库我用的是 [MariaDB](https://mariadb.org/)；

### 安装方式

Gogs 有多种安装方式，一般使用源码安装和二进制安装。但是源码安装相对麻烦一点，需要在本地有 Go 语言环境，然后将源码下到本地，再进行编译，期间可能还会遇到其他一些问题。

通常直接进行二进制安装，下载最新版本的 Gogs，在 Windows 环境下有两种安装方式；

#### 手动安装

下载 Gogs mws（Minimal Windows Service）版，即提供内置 Windows 服务支持版本；

下载完成之后依次执行以下步骤：

1. 解压压缩包；
2. 使用命令 cd 进入到刚刚创建的目录；
3. 执行命令 `./gogs web`；
4. 访问`/install`以进行初始配置，默认在`3000`端口启动 HTTP 服务；
5. 配置服务器访问端口；

初次运行

{% asset_img Gogs运行.png %}

配置界面

{% asset_img Gogs配置.png %}

在本地测试的时候发现手动命令行启动 Gogs 时需要通过 Bash 开启服务，否则会出现无法克隆仓库的问题，后面才发现是环境变量不完整；

#### 自动安装

而更推荐的做法是下载非 mws 版，这样可以使用 NSSM 将 Gogs 注册成 Windows 服务在后台运行；

然后通过 NSSM 注册服务并开启后，同样会出现无法克隆仓库的问题；
最后发现环境变量中要有 ssh-keygen 的路径，可以是 git 下的路径（`e.g. C:\Program Files\Git\usr\bin`）
当然也可以在 NSSM 注册服务时，通过 Environment 栏添加包含 ssh-keygen 的路径；

其中在使用 NSSM 注册服务时，可以按照[官方教程](https://gogs.io/docs/installation/run_as_windows_service)进行手动配置；
而更简单的做法是，使用 Gogs 中自带的脚本进行自动注册；

1. 找到`C:\gogs\scripts\windows`下的`install-as-service.bat`文件；
2. 右键以管理员身份打开；
3. 然后看到注册成功的提示信息则说明服务已经注册并开启；
4. 直接访问`localhost:3000`，进入初始配置页面进行配置；

### 相关配置

通常在初始安装后的初始配置时完成各项配置，后续需要更新配置时可以在根目录下的`custom/conf/app.ini`。详细的配置解释和默认值请参考官方文档，其中关键的配置是下面这些，详细配置放在{% post_link Gogs-配置文件手册 %}中。

|配置|说明|
|:-----|:-----|
|RUN_USER | 默认是 git，指定 Gogs 以哪个用户运行|
|ROOT | 所有仓库的存储根路径|
|PROTOCOL | 如果你使用 nginx 反代的话请使用 http，如果直接裸跑对外服务的话随意|
|DOMAIN | 域名，会影响 SSH clone 地址|
|ROOT_URL | 完整的根路径，会影响访问时页面上链接的指向，以及 HTTP clone 的地址|
|HTTP_ADDR | 监听地址，使用 nginx 的话建议 127.0.0.1，否则 0.0.0.0 也可以|
|HTTP_PORT | 监听端口，默认 3000|
|INSTALL_LOCK | 锁定安装页面|
|Mailer | 相关的选项，可以使用 Mailgun 的免费邮件发送服务，将 Mailgun 的 SMTP 配置填入到配置中|

此外除了基础配置外，还可以通过自定义模板更改默认的页面主题外观或新增一些额外的自定义功能；注意：所有针对自定义模板的修改都需要重启 Gogs 实例。

- 重载 HTML 模板：通过在`custom/templates/`目录下创建一个自定义版本的副本实现 HTML 模板重载（包括邮件模板）
- 重载静态文件：通过在`custom/public/`目录下创建一个自定义版本的副本实现静态文件重载（CSS、JS、图片等等）
- 注入自定义内容到模板中：通过在`custom/templates/inject/head.tmpl`或`custom/templates/inject/footer.tmpl`文件中分别添加自定义头部 meta 信息和引入自定义静态资源（如 CSS）或自定义 JS 和分析代码；

### 参考资料

- [官方文档](https://gogs.io/docs)
- [历史版本](https://github.com/gogs/gogs/releases)
- [使用 Gogs 搭建自己的 Git 服务器](https://blog.mynook.info/post/host-your-own-git-server-using-gogs/)
- [注入自定义头部和尾部](https://discuss.gogs.io/t/how-to-inject-custom-head-and-footer/943)
- [Gogs在windows上的安装和配置](https://o-r.github.io/2018/05/12/Gogs%E5%9C%A8windows%E4%B8%8A%E7%9A%84%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE/)
- [gogs配置详解](https://dashdream.github.io/p/7dd2a61d.html)
- [win10操作系统Gogs配置解决ssh方式git免密下载上传问题](https://www.cxyzjd.com/article/e891377/109302884)
- [Docker-Ubuntu-Gogs部署及配置时遇到的问题](https://www.itfanr.cc/2017/03/24/docker-ubuntu-gogs-problems/)
- [Windows下安装Gogs](https://www.jianshu.com/p/b78b385cf168)
- [Git服务器Gogs简易安装-Windows环境](https://www.cnblogs.com/configure/p/7878501.html)