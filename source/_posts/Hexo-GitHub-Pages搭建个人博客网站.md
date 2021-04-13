---
title: Hexo + GitHub Pages搭建个人博客网站
date: 2021-04-05 20:34:29
tags: [Hexo,GitHub Pages]
categories: blog
---


### Hexo简介

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。本文记录了如何利用Hexo和Github Pages快速搭建个人博客网站的整个过程。

中文官网：<https://hexo.io/zh-cn/>

<!-- more -->

### 环境搭建

1. 安装nodejs

官网地址：<https://nodejs.org/zh-cn/download/>
选择相应版本安装后分别执行`node -v`和`npm -v`查看是否正确安装。

```bash
> node -v
v12.16.2
> npm -v
6.14.4
```

2. 安装git

官网地址：<https://git-scm.com/downloads>
同样在安装好后检查安装的版本信息

```bash
> git --version
git version 2.29.2.windows.2
```

3. 安装hexo

全局安装hexo-cli

```bash
> npm install -g hexo-cli
```

检查hexo版本

```bash
> hexo -v
hexo-cli: 4.2.0
os: Windows_NT 10.0.18363 win32 x64
node: 12.16.2
v8: 7.8.279.23-node.34
uv: 1.34.2
zlib: 1.2.11
brotli: 1.0.7
ares: 1.15.0
modules: 72
nghttp2: 1.40.0
napi: 5
llhttp: 2.0.4
http_parser: 2.9.3
openssl: 1.1.1e
cldr: 36.0
icu: 65.1
tz: 2019c
unicode: 12.1
```

然后创建一个空文件夹作为自己博客的文件夹，然后使用hexo初始化博客文件夹，`hexo init myblog`或者在该文件夹下执行`hexo init`。等待hexo的默认文件从github上下载完成后，在该文件夹下执行npm install，下载hexo所有的第三方依赖。得到的文件目录如下：

```code
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

_config.yml是网站的配置文件。
package.json是项目的依赖、描述、版本等相关信息。
scaffolds文件夹下用于存放模板，在新建文章时，Hexo以此来建立文件。
source是资源文件夹，用于存放用户资源，其中_post存放md文档。
themes设置Hexo主题。

本地预览

```bash
> hexo generate/g
> hexo server/s
```

在浏览器中访问 <http://localhost:4000> 查看

4. 创建GitHub仓库并通过ssh key关联

- 配置git

在GitHub中创建一个新的同名仓库，coley48.github.io。然后需要在本地创建ssh key并关联到GitHub账号，如果初次使用git，需要设置用户名和邮箱。打开git bash，设置全局用户名和邮箱。

```bash
$ git config --global user.name coley48
$ git config --global user.email coley48@qq.com
```

查看用户名和邮箱

```bash
$ git config user.name && git config user.email
coley48
coley48@qq.com
```

创建ssh密钥，不设置密码，全部回车：

```bash
$ ssh-keygen -t rsa -C "coley48@qq.com"
```

默认创建在用户文件夹下的.ssh文件夹中。
然后查看生成的密钥：

```bash
$ cd ~/.ssh && ls
id_rsa  id_rsa.pub
```

其中id_rsa是私钥，id_rsa.pub是公钥，用于上传到github。

将ssh key关联到github账号
在git bash中复制id_rsa.pub

```bash
$ cat id_rsa.pub > /dev/clipboard
```

或在cmd终端中复制id_rsa.pub

```bash
> clip < id_rsa.pub
```

在github的中添加ssh key

完成后测试ssh连接

```bash
$ ssh -T git@github.com
Warning: Permanently added the RSA host key for IP address '192.30.255.113' to the list of known hosts.
Hi Coley48! You've successfully authenticated, but GitHub does not provide shell access.
```

首次连接会出现警告，此后git会将github的解析地址存到.ssh下的known_host中。

GitHub文档：使用 SSH 连接到 GitHub
<https://docs.github.com/cn/github/authenticating-to-github/connecting-to-github-with-ssh>

修改_config.yml，将hexo关联到github

```yml
deploy:
  type: git
  repo: https://github.com/Coley48/coley48.github.io.git
  branch: master
```

配置完git后，再安装hexo的部署插件

```bash
> npm install hexo-deployer-git --save
```

更新网站

```bash
> hexo clean
> hexo generate
> hexo deploy
```

绑定个人域名

1. 在域名解析控制台添加解析记录
2. 在github.io仓库中设置Custom Domain为blog.coley48.cn
3. 在hexo项目中的/source文件夹下创建一个CNAME文本文件，存放域名
4. 重新deploy网站

### 配置hexo

|        参数 | 描述                           |
| ----------: | :----------------------------- |
|       title | 网站标题                       |
|    subtitle | 网站副标题                     |
| description | 网站描述                       |
|    keywords | 网站的关键词。支援多个关键词。 |
|      author | 作者名字                       |
|    language | 网站使用的语言。               |
|    timezone | 网站时区。默认使用电脑的时区。 |