---
title: Hexo + GitHub Pages搭建个人博客网站
date: 2021-04-05 20:34:29
tags: [Hexo,GitHub Pages]
categories: blog
---


### Hexo 简介

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。本文记录了如何利用 Hexo 和 Github Pages 快速搭建个人博客网站的整个过程。以下代码均已我的博客和仓库为例，请自行更改为自己的。

中文官网：<https://hexo.io/zh-cn/>

<!-- more -->

### 环境搭建

#### 安装 nodejs

官网地址：<https://nodejs.org/zh-cn/download/>
选择相应版本安装后分别执行`node -v`和`npm -v`查看是否正确安装。

```sh
> node -v
v12.16.2
> npm -v
6.14.4
```

#### 安装 Git

官网地址：<https://git-scm.com/downloads>
同样在安装好后检查安装的版本信息

```sh
> git --version
git version 2.29.2.windows.2
```

#### 安装 Hexo

全局安装`hexo-cli`

```sh
> npm install -g hexo-cli
```

检查 Hexo 版本，会得到以下输出：

```sh
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

然后依次执行以下命令：

```sh
> mkdir myBlog # 创建一个空的博客文件夹
> cd myBlog # 进入到博客文件夹中
> hexo init # 使用 hexo init 命令下在 Hexo 项目初始化文件
> npm install # 下载 Hexo 项目的第三方依赖
```

最后得到的文件目录如下：

```txt
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

| 文件         | 描述         |
| :----------- | :----------- |
| _config.yml  | 网站配置文件 |
| package.json | 项目配置文件 |
| scaffolds    | 模板文件夹   |
| source       | 资源文件夹   |
| themes       | 主题文件夹   |

本地预览

```sh
> hexo generate
> hexo server
```

在浏览器中访问 <http://localhost:4000> 即可查看网站默认样式。

#### 关联 GitHub 仓库

- 创建仓库

在 GitHub 中创建一个同名仓库，命名为`coley48.github.io`，这也将成为网站的域名。

- 配置 Git

如果初次使用Git，需要设置用户名和邮箱。打开Git Bash，设置全局用户名和邮箱。

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

- 创建 SSH 密钥

`ssh`还可以设置密码，通常方便起见，这里不设置密码，全部回车。

```bash
$ ssh-keygen -t rsa -C "coley48@qq.com"
```

默认创建在用户文件夹下的`.ssh`文件夹中，查看生成的密钥：

```bash
$ cd ~/.ssh && ls
id_rsa  id_rsa.pub
```

其中`id_rsa`是私钥，`id_rsa.pub`是公钥，用于上传到 GitHub。

将`ssh key`关联到 GitHub 账号
在 Git Bash 中复制`id_rsa.pub`

```bash
$ cat id_rsa.pub > /dev/clipboard
```

或在 cmd 终端中复制`id_rsa.pub`

```sh
> clip < id_rsa.pub
```

- 添加 SSH 密钥

在 <https://github.com/settings/keys> 页面中选择`New SSH key`新建`ssh key`，标题自定义，然后将上一步复制的公钥粘贴进去，点击`Add SSH key`完成添加。
{% asset_img 添加ssh-key.png %}

- 测试 SSH 连接

```bash
$ ssh -T git@github.com
Warning: Permanently added the RSA host key for IP address '192.30.255.113' to the list of known hosts.
Hi Coley48! You've successfully authenticated, but GitHub does not provide shell access.
```

GitHub文档：[使用 SSH 连接到 GitHub](https://docs.github.com/cn/github/authenticating-to-github/connecting-to-github-with-ssh)

- Hexo 一键部署

一键部署功能是在本地编辑完成之后，直接使用`hexo deploy`命令就可以完成部署操作。

一键部署功能需要安装 Hexo 的部署插件：

```sh
> npm install hexo-deployer-git --save
```

然后修改_config.yml，为 Hexo 指定部署的 Github 仓库：

```yml
deploy:
  type: git
  repo: https://github.com/Coley48/coley48.github.io.git
  branch: master
```

之后就可以使用以下命令更新网站：

```sh
> hexo clean # 清除之前生成的静态文件，不是必须的
> hexo generate # 生成静态网页文件
> hexo deploy # 部署到远程仓库
```

- 绑定个人域名

GitHub Pages还提供 Custom Domain 可以使用自定义域名。

1. 在域名解析控制台添加 CNAME 解析记录，将 coley48.github.io 解析到 blog.coley48.cn。
2. 同时在 github.io 仓库中设置 Custom Domain 为 blog.coley48.cn。
3. 然后在 /source 文件夹下创建一个 CNAME 文本文件，存放域名。
4. 最后重新生成部署一下网站，稍微等待片刻便可以通过自己的域名访问博客网站了。

Github文档：[配置 GitHub Pages 站点的自定义域](https://docs.github.com/cn/pages/configuring-a-custom-domain-for-your-github-pages-site)

### Hexo 基本配置

Hexo的所有配置都在_config.yml文件中，部分配置如下：

|              参数 | 描述                                    |
| ----------------: | :-------------------------------------- |
|             title | 网站标题                                |
|          subtitle | 网站副标题，可以放座右铭                |
|       description | 网站描述                                |
|          keywords | 网站的关键词                            |
|            author | 作者名字                                |
|          language | 网站使用的语言                          |
|               url | 网址，必须是完整的url                   |
| post_asset_folder | 启动 Asset 文件夹，默认false            |
|     relative_link | 把链接改为与根目录的相对位址，默认false |
|             theme | 设置主题，false禁用主题                 |
|      theme_config | 主题的配置参数，优先级高                |
|            deploy | 部署配置                                |

更多配置详见[官方文档](https://hexo.io/zh-cn/docs/configuration)

### Hexo 的基本使用

- 初始化

```sh
> hexo init [folder]
```

新建网站，未指定 folder 时在当前文件夹下创建。
创建步骤：

1. git 下载包含 hexo-theme-landscape 主题的 hexo-starter 项目。
2. 通过包管理工具安装依赖，默认使用 npm。

- 新建

```sh
> hexo new [layout] <title>
```

新建文章，`layout`默认使用`_config.yml`中`default_layout`参数值，包含空格的标题需要加引号。三种布局：`post`、`page`、`draft`。
参数：
`-p, --path` 自定义文章的路径，默认使用标题作为文章路径。
`-r, --replace` 替换同名文章。
`-s, --slug` 作为新文章的文件名和发布后的 URL。

- 生成

```sh
> hexo generate/g
```

生成网页，将生成的文件放入`public`文件夹下，如果存在`public`，默认只生成改动的文件。
参数：
`-d, --deploy` 文件生成后立即部署网站。
`-w, --watch` 监视文件变动。
`-b, --bail` 生成过程中如果发生任何未处理的异常则爆出异常。
`-f, --force` 强制重新生成文件，等同于`hexo clean && hexo generate`。
`-c, --concurrency` 设置最大同时生成文件的数量，默认无限制。
`--config <custom.yml> [config.json]` 使用自定义配置文件。

- 发布

```sh
> hexo publish [layout] <filename>
```

发表草稿。

- 本地预览

```sh
> hexo server/s
```

启动服务器。默认情况下，网址为：<http://localhost:4000>。
参数：
`-p, --port` 设置端口。
`-s, --static` 只使用静态文件。
`-l, --log` 启动日志记录，使用覆盖记录格式。
`--config <custom.yml>` 使用自定义配置文件。

- 部署

```sh
> hexo deploy/d
```

部署网站
`-g, --generate` 部署之前预先生成静态文件，等同于`hexo g -d`。

- 清除

```sh
> hexo clean
```

清除缓存文件`db.json`和已生成的静态文件夹`public`。

更多命令详见[官方文档](https://hexo.io/zh-cn/docs/commands)。

### 参考资料

- [Hexo 官方文档](https://hexo.io/zh-cn/docs/)
- [Hexo 搭建 Blog 精简笔记](https://www.cnblogs.com/michael-xiang/p/10464518.html)
- [Hexo 史上最全搭建教程](https://blog.csdn.net/sinat_37781304/article/details/82729029?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.baidujs&dist_request_id=1328741.37347.16169781187402753&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.baidujs)
- [Hexo 图片插入](http://www.xinxiaoyang.com/programming/2016-11-25-hexo-image-bug/)
- [YAML 语言教程](https://www.ruanyifeng.com/blog/2016/07/yaml.html)