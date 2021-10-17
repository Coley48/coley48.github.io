---
title: Travis CI + GitHub Pages自动部署Hexo博客
tags: [Hexo, Travis-CI, GitHub Pages, 持续集成]
date: 2021-04-15 22:23:21
categories: blog
---

### 什么是持续集成？

持续集成（Continuous Integration，简称 CI）指的是只要代码有变更，就自动运行构建和测试，反馈运行结果。确保符合预期以后，再将新代码“集成”到主干。

持续集成的好处在于，每次代码的小幅变更，就能看到运行结果，从而不断累积小的变更，而不是在开发周期结束时，一下子合并一大块代码。

Travis CI 提供的是持续集成服务。它绑定 Github 上面的项目，只要有新的代码，就会自动抓取。然后，提供一个运行环境，执行测试，完成构建，还能部署到服务器。

本文记录了利用 Travis CI 搭建自动部署的 Hexo 博客系统的整个过程。

<!-- more -->

### Travis CI 的使用

#### 创建 token

1. 进入配置页面，先在 Github 的设置页面中找到 Developer settings，点进入后选择第三条 Personal access tokens，或者直接点击[这个链接](https://github.com/settings/tokens)跳转配置页面。

2. 生成密钥，点击右上 Generate new token 按钮，可能会需要输入密码验证一下用户身份，然后填写 token 名称用于标识该 token 的用途，然后在下方为 token 设置权限，这里只需要添加 repo 即可。
{% asset_img 生成personal-access-token.png %}
这里需要注意的是，密钥只会在第一次生成时可见，页面刷新后就无法查看密钥的值，因此创建后需要立即保存，否则只能重新生成。

3. 添加密钥，进入 Travis CI 官网 [travis-ci.com](https://travis-ci.com)，首次进入需要通过 Github 来进行仓库的授权访问，完成之后会进入到自己的控制面板，这里可以看到自己 GitHub 上的所有仓库，然后进入需要部署的仓库的 setting 页面，在这里添加环境变量，变量名设置为 GH_TOKEN ，变量值为上一步骤中保存的哈希值。
{% asset_img 添加token到Travis-CI环境变量.png %}
切记不要点开 DISPLAY VALUE IN BUILD LOG 选项，否则 token 值会输出到构建日志中，就会暴露给别人。

#### 添加`travis.yml`

Travis CI 的的部署极其简单，只用在项目根目录中添加一个`travis.yml`配置文件，该文件指定了所有 Travis CI 的构建和部署行为。

下面以我的 Hexo 博客项目为例：

```yml
os: linux # 选择构建的操作系统
language: node_js # 项目语言
node_js:
  - 10  # 使用 nodejs LTS v10
branches:
  only:
    - main # 只监控存放源代码的 main 分支
cache:
  directories:
    - node_modules # 缓存 node_modules 加快构建速度
before_script: # 构建环境搭建，根据所用的主题，这里会有所不同
  - npm install -g hexo-cli # 在 CI 环境内安装 Hexo
  - mkdir themes # 由于 main 分支中没有将 themes/ 文件夹，所以需要重新创建
  - git clone https://github.com/theme-next/hexo-theme-next themes/next #从 Github 上拉取 next 主题
  - npm install # 最后在根目录安装站点需要的依赖 
script: 
  - hexo generate # 生成静态文件
deploy: # 根据个人情况，这里会有所不同
  provider: pages
  skip_cleanup: true # 构建完成后不清除
  strategy: git # 默认使用 git
  token: $GH_TOKEN # 在 Travis CI 中设置的环境变量 token
  keep_history: true # 保存历史
  fqdn: blog.coley48.cn # 自定义域名，使用 username.github.io 可删除
  on:
    branch: main # hexo 站点源文件所在的 branch
  local_dir: public
  target_branch: gh-pages # 存放生成站点文件的 branch，使用 username.github.io 必须是 master
```

#### 提交项目

`git push`命令执行完成之后，稍等片刻 Travis CI 就会开始运行。
在 Travis CI 控制面板中，进入到自己的仓库页面中可以看到项目正在构建，显示黄色：

{% asset_img travis-CI构建中.png %}

在下方`Job log`中还可以查看构建过程中的输出信息：

{% asset_img travis-CI构建过程输出信息.png %}

当输出`Done. Your build exited with 0.`时，表示构建成功，显示为绿色：

{% asset_img travis-CI构建完成.png %}

此后，Travis CI 会自动监听 GitHub 仓库提交，每次有新的代码提交，Travis CI 会自动按照`travis.yml`配置文件中的命令执行，然后将生成的代码推送到指定的分支中。

### 参考资料

- [Travis CI 官网](https://www.travis-ci.com)
- [Travis CI 官方文档](https://www.travis-ci.com/getting_started)
- [持续集成服务 Travis CI 教程](https://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html)
- [Travis CI 极简教程](https://dunwu.github.io/tools/travis-ci%E6%9E%81%E7%AE%80%E6%95%99%E7%A8%8B.html)
- [使用 Github Pages 和 Hexo 搭建自己的独立博客](https://www.itrhx.com/2018/08/15/A02-hexo-blog/)
- [Travis CI + github + hexo 自动化部署](https://segmentfault.com/a/1190000017909815?utm_source=sf-similar-article)
- [Travis CI 加 Hexo 实现自动构建部署 Github Pages 博客](https://segmentfault.com/a/1190000021987832)
- [使用Travis CI自动部署Hexo博客](https://developer.aliyun.com/article/569237)
