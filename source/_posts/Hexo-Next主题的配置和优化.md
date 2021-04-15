---
title: Hexo Next主题的配置和优化
date: 2021-04-14 17:38:17
tags: [Hexo, Next]
categories: blog
---

### Next 简介

Next 是 Hexo 最受欢迎的主题之一，而且在 Next 之下还分为4个`scheme`。可以根据个人喜好来设置相应的`scheme`。

{% asset_img Next-theme.png %}

官方文档：<https://theme-next.js.org/>
GitHub 地址：<https://github.com/theme-next/hexo-theme-next>

<!-- more -->

### Next 安装

进入到博客根目录下，将主题项目下载到`themes/`文件夹下：

```sh
> cd myBlog
> git clone https://github.com/theme-next/hexo-theme-next themes/next
```

然后打开根目录下的`_config.yml`配置文件，即网站配置文件，将其中的主题选项，参数改为`next`即可。

```yml
theme: next
```

### Next 配置

这里有两种方式配置 Next 主题，一是更改`/themes/next/`文件夹下的`_config.yml`，即主题配置文件；二是在网站配置文件中添加一个 `theme_config`属性，然后在该属性后面添加配置参数。

这里推荐使用第二种方式，因为网站配置文件中`theme_config`下参数优先级高于主题配置文件，因此会覆盖主题配置文件，同时不用更改 Next 主题源码，当 Next 主题更新时也不需要再修改主题配置文件。

以我的博客为例，主题配置部分如下：

```yml
theme_config: 
  # 页面底部设置
  footer:
    icon: # 设置年份和版权信息
      name: fa fa-heart # 默认心形，也可以设置 Font Awessome 中的其他图标 
      # https://fontawesome.com/icons
      animated: true # 添加动画，默认 false
      color: "#fc6423" # 使用十六进制设置图标颜色

    powered: false # 是否显示 Powered by Hexo & NexT
    since: 2021 # 设置建站时间
    copyright: Hush # 设置版权归属

  # 文章协议
  creative_commons:
    license: by-nc-sa # 协议名
    sidebar: false # 是否显示在侧栏
    post: true # 是否显示在文章中
    language: # 选择语言

  # 主题方案
  scheme: Gemini # 选择主题方案，默认 Muse
  # Muse | Mist | Pisces | Geminim

  # 菜单栏页面设置
  menu:
    home: / || fa fa-home # 主页
    about: /about/ || fa fa-user # 关于
    tags: /tags/ || fa fa-tags # 标签
    categories: /categories/ || fa fa-th # 分类
    archives: /archives/ || fa fa-archive # 归档
    #schedule: /schedule/ || fa fa-calendar # 计划
    #sitemap: /sitemap.xml || fa fa-sitemap # 站点地图
    # commonweal: /404/ || fa fa-heartbeat # 公益404

  # 头像设置
  avatar:
    url: /images/avator.jpg # 设置头像路径
    rounded: true # 头像圆角
    rotated: true # 头像旋转效果

  # 社交设置
  social:
    GitHub: https://github.com/Coley48 || fab fa-github # GitHub
    E-Mail: mailto:coley48@qq.com || fa fa-envelope # 邮箱
    # Weibo | Google | Twitter | FB Page | StackOverflow |  YouTube

  # 打赏功能设置
  reward_settings:
    enable: false # 是否开启打赏功能
    animation: false # 打赏功能动画效果
    #comment: Donate comment here. # 设置打赏语

  # 打赏方式设置
  reward:
    #wechatpay: /images/wechatpay.png
    #alipay: /images/alipay.png
    #paypal: /images/paypal.png
    #bitcoin: /images/bitcoin.png

  # 代码块设置
  codeblock:
    highlight_theme: normal # 高亮主题 
    # normal | night | night eighties | night blue | night bright | solarized | solarized dark | galactic
    copy_button:
      enable: true # 是否开启一键复制功能
      show_result: true # 显示复制成功结果
      style: mac # 三种显示效果 default | flat | mac
    beian: # 备案
      enable: false
      icp: 蜀ICP备 20003974-1号
      # gongan_id: 1234567890
      # gongan_num: 京公网安备 1234567890号
      # gongan_icon_url: /uploads/beian.png

  # 回顶部按钮
  back2top:
    enable: true # 是否开启
    sidebar: true # 在侧栏显示
    scrollpercent: true # 显示百分比

  # 阅读进度条
  reading_progress:
    enable: true # 是否开启
    position: top # 显示位置 top | bottom
    color: "#37c6c0" # 进度条颜色
    height: 3px # 进度条宽度

  # `Follow me on GitHub` banner in the top-right corner.
  github_banner:
    enable: true # 是否显示
    permalink: https://github.com/Coley48 # GitHub 地址
    title: Follow me on GitHub

  # Local Search 本地搜索
  # Dependencies: https://github.com/theme-next/hexo-generator-searchdb
  local_search:
    enable: true
    trigger: auto # 搜索结果显示方式，auto 内容更改显示；manual 手动回车显示
    top_n_per_article: 1 # 设置显示条数，-1 为显示所有
    unescape: false # 反转义 html 字符串
    preload: false # 页面加载时预加载搜索数据

  # 添加友链
  links:
    # Title: http://site.com

```

更多配置详见：[Next 使用文档](http://theme-next.iissnan.com)。

### 参考资料

- [hexo+next 部署各种炫酷博客特效](https://jiyali.github.io/2019/04/15/hexo-next-%E9%83%A8%E7%BD%B2%E5%90%84%E7%A7%8D%E7%82%AB%E9%85%B7%E5%8D%9A%E5%AE%A2%E7%89%B9%E6%95%88/)
- [打造个性超赞博客 Hexo + NexT + GitHub Pages 的超深度优化](https://io-oi.me/tech/hexo-next-optimization/)
