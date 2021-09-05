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

git（bash）：

- 服务端和客户端均需版本 >= 1.8.3
- Windows 系统建议使用最新版

SSH 服务器：

- 如果您只使用 HTTP/HTTPS 的话请忽略此项
- 如果您选择在 Windows 系统使用内置 SSH 服务器，请确保添加`ssh-keygen`到您的`%PATH%`环境变量中
- 推荐 Windows 系统使用 Cygwin OpenSSH 或 Copssh
- Windows 系统 请确保 Bash 是默认的 Shell 程序，而不是 PowerShell

在设置 SSH 服务的时候踩了坑，在本地测试的时候发现 Gogs 需要通过 Bash 开启服务，否则会出现无法克隆仓库的问题；目前也没有找到设置 Windows 默认 Shell 的方法；

### 安装方式

Gogs 有多种安装方式，一般使用源码安装和二进制安装。但是源码安装相对麻烦一点，需要在本地有 Go 语言环境，然后将源码下到本地，再进行编译，期间可能还会遇到其他一些问题。

因此通常直接进行二进制安装，下载最新版本的 Gogs，注意如果是 Windows 环境的话建议直接下载 mws（Managed Windows Service）版，即提供内置 Windows 服务支持，否则还需要手动配置进行注册；

下载好之后依次执行以下步骤：

1. 解压压缩包；
2. 使用命令 cd 进入到刚刚创建的目录；
3. 执行命令 `./gogs web`，检验是否安装成功；
4. 访问`/install`以进行初始配置，Gogs 默认会在端口`3000`启动 HTTP 服务；

初次运行

{% asset_img Gogs运行.png %}

配置界面

{% asset_img Gogs配置.png %}

### 相关配置

通常在初始安装后的初始配置时完成各项配置，后续需要更新配置时可以在根目录下的`custom/conf/app.ini`。详细的配置解释和默认值请参考官方文档，其中关键的配置是下面这些，详细配置放在文末附录中。

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

### 附录

配置文件手册会详尽地描述有关 Gogs 配置文件的选项，帮助您更好地理解和使用 Gogs。
请记住，任何修改都是发生在 custom/conf/app.ini 自定义配置文件中，该文件的具体位置与您的设置有关。
完整的默认设置可以通过 app.ini 文件查看。如果您看到类似 %(X)s 字符，这是由 ini 提供的递归取值的特性。

**概览**

|名称|描述|
|:-----|:-----|
|APP_NAME | 应用名称，可以改成您的组织或公司名称|
|RUN_USER | 运行应用的用户名称，我们建议您使用 git，但如果您在个人计算机上运行 Gogs，请修改为您的系统用户名称。如果没有正确设置这个值，很可能导致您的应用崩溃|
|RUN_MODE | 鉴于性能和其它考虑，建议在部署环境下修改为 prod 模式。在您完成安装操作时，该值也会被设置为 prod|

**服务器 (server)**

|名称|描述|
|:-----|:-----|
|PROTOCOL | http 或 https|
|DOMAIN | 服务器域名|
|ROOT_URL | 公开的完整 URL 路径|
|HTTP_ADDR | 应用 HTTP 监听地址|
|HTTP_PORT | 应用 HTTP 监听端口号|
|UNIX_SOCKET_PERMISSION | Unix 套接字文件的权限|
|LOCAL_ROOT_URL | 用于 Gogs 工作进程（如：SSH）回访应用的本地（DMZ）URL，一般情况下请保持默认值，除非您的 SSH 服务器节点与 HTTP 并不是同一个节点入口|
|DISABLE_SSH | 当 SSH 功能不可用时可以禁用|
|START_SSH_SERVER | 启用该选项来启动内置 SSH 服务器|
|SSH_DOMAIN | 允许公用网络访问 SSH 的域名|
|SSH_PORT | SSH 端口号，如果不为 22 的话可以在此修改|
|SSH_LISTEN_HOST | 内置 SSH 服务器监听的地址|
|SSH_LISTEN_PORT | 内置 SSH 服务器监听的端口|
|SSH_ROOT_PATH | SSH 根目录，一般为 ~/.ssh，但必须填写为 /home/git/.ssh|
|REWRITE_AUTHORIZED_KEYS_AT_START | 激活该选项以在应用启动时自动重写 authorized_keys 文件，该选项在使用内置 SSH 服务器时将会被自动禁用|
|SSH_KEY_TEST_PATH | 用于测试 SSH 公钥的临时目录|
|SSH_KEYGEN_PATH | ssh-keygen 程序的路径，默认为 ssh-keygen 即通过系统路径查找|
|MINIMUM_KEY_SIZE_CHECK | 指定不同类型的公钥的最小密钥大小|
|OFFLINE_MODE | 激活该选项来禁止从 CDN 获取静态资源，同时 Gravatar 服务也将被自动禁用|
|DISABLE_ROUTER_LOG | 激活该选项来禁止打印路由日志|
|CERT_FILE | HTTPS 授权文件路径|
|KEY_FILE | HTTPS 的密钥文件路径|
|STATIC_ROOT_PATH | 模板文件和静态文件的上级目录，默认为应用二进制所在的位置|
|APP_DATA_PATH | 应用内部数据的存放目录|
|ENABLE_GZIP | 激活该选项来启用应用级别 GZIP 支持|
|LANDING_PAGE | 未登录用户的默认首页，可以是 home 或 explore（探索页）|

**仓库 (repository)**

|名称|描述|
|:-----|:-----|
|ROOT | 用户仓库存储根目录，必须为绝对路径，默认为 ~/<user name>/gogs-repositories|
|SCRIPT_TYPE | 系统脚本类型，一般情况下均为 bash，但有些用户反应只能使用 sh|
|ANSI_CHARSET | 当遇到无法识别的字符集时使用的默认字符集|
|FORCE_PRIVATE | 强制要求所有新建的仓库都是私有的|
|MAX_CREATION_LIMIT | 全局默认的每个用户可创建创建仓库上限，-1 表示无限制|
|PREFERRED_LICENSES | 建议用户首选的授权类型|
|DISABLE_HTTP_GIT | 激活该选项来禁止用户通过 HTTP 对 Git 仓库进行交互操作，即用户只能通过 SSH 操作|
|ENABLE_LOCAL_PATH_MIGRATION | 激活该选项来启用本地路径迁移仓库功能。启动后默认只有管理员可以使用，普通用户必须经由管理员授权|
|ENABLE_RAW_FILE_RENDER_MODE | 激活该选项来启用在查看原始数据时对页面进行真实渲染，例如渲染实际的 HTML 页面，有潜在安全隐患|

**仓库 - 编辑器 (repository.editor)**

|名称|描述|
|:-----|:-----|
|LINE_WRAP_EXTENSIONS | 需要显示为行包装的文件名后缀，通过逗号分隔。如果是无后缀名的文件，则单独放置一个逗号，例如：.txt,|

**仓库 - 文件上传 (repository.upload)**

|名称|描述|
|:-----|:-----|
|ENABLED | 激活该选项来启用仓库文件上传功能|
|TEMP_PATH | 文件上传的临时存放目录|
|ALLOWED_TYPES | 允许上传的文件类型（例如：”image/jpeg\|image/png”），留空表示允许上传任意类型的文件|
|FILE_MAX_SIZE | 单个上传的文件的最大体积，以 MB 为单位|
|MAX_FILES | 单次同时上传的最多文件个数|

**版本发布 - 附件 (release.attachment)**

|名称|描述|
|:-----|:-----|
|ENABLED | 激活该选项来启用版本发布附件功能|
|PATH | 存放附件的路径|
|ALLOWED_TYPES | 允许上传的 MIME 类型，例如 “image/jpeg|image/png”，使用 */* 允许所有类型的文件|
|MAX_SIZE | 最大允许上传的附件体积，单位为 MB，例如 32|
|MAX_FILES | 最大允许一次性上传的附件个数，例如 10|

**Markdown (markdown)**

|名称|描述|
|:-----|:-----|
|ENABLE_HARD_LINE_BREAK | 指示是否启用硬性换行扩展|
|CUSTOM_URL_SCHEMES | 允许被解析为链接的自定义 URL 方案，例如 git（用于 git://）和magnet（用于 magnet://）|
|FILE_EXTENSIONS | 需要被渲染为 Markdown 格式的文件名后缀，通过逗号分隔。如果是无后缀名的文件，则单独放置一个逗号，例如：.markdown,|

**Smartypants (smartypants)**

|名称|描述|
|:-----|:-----|
|ENABLED | 指示是否启用 Smartypants 扩展|

**HTTP (http)**

|名称|描述|
|:-----|:-----|
|ACCESS_CONTROL_ALLOW_ORIGIN | 头信息 Access-Control-Allow-Origin 的自定义值，默认为空，即不响应此头信息|

**数据库 (database)**

|名称|描述|
|:-----|:-----|
|DB_TYPE | 数据库类型，可以是 mysql、postgres、mssql 或 sqlite3|
|HOST | 数据库主机地址与端口|
|NAME | 数据库名称|
|USER | 数据库用户名|
|PASSWD | 数据库用户密码|
|SSL_MODE | 仅限 PostgreSQL 使用|
|PATH | 仅限 SQLite3 使用，数据库文件路径|

**应用管理 (admin)**

|名称|描述|
|:-----|:-----|
|DISABLE_REGULAR_ORG_CREATION | 激活该选项来禁止普通用户（非管理员）创建组织|

**安全 (security)**

|名称|描述|
|:-----|:-----|
|INSTALL_LOCK | 用于指示是否允许访问安装页面（该页面可以设置管理员帐号，因此该选项非常重要）|
|SECRET_KEY | 全局的加密密钥，务必修改该值以确保您的服务器安全（会在每次安装时自动生成随机字符串）|
|LOGIN_REMEMBER_DAYS | 记住登录的天数|
|COOKIE_USERNAME | 记录用户名的 Cookie 名称|
|COOKIE_REMEMBER_NAME | 记录用户自动登录信息的 Cookie 名称|
|REVERSE_PROXY_AUTHENTICATION_USER | 反向代理认证用户的 Header 字段名|

**服务 (service)**

|名称|描述|
|:-----|:-----|
|ACTIVE_CODE_LIVE_MINUTES | 激活码的有效期，单位为分钟|
|RESET_PASSWD_CODE_LIVE_MINUTES | 重置密码的有效期，单位为分钟|
|REGISTER_EMAIL_CONFIRM | 激活该选项来要求注册用户必须验证邮箱，要求已启用 Mailer|
|DISABLE_REGISTRATION | 激活该选项来禁止用户注册功能，只能由管理员创建帐号|
|SHOW_REGISTRATION_BUTTON | 用于指示是否显示注册按钮|
|REQUIRE_SIGNIN_VIEW | 激活该选项来要求用户必须登录才能浏览任何页面|
|ENABLE_CACHE_AVATAR | 激活该选项来缓存 Gravatar 的头像|
|ENABLE_NOTIFY_MAIL | 激活该选项来发送通知邮件给关注者，例如创建 issue 时，要求已启用 Mailer|
|ENABLE_REVERSE_PROXY_AUTHENTICATION | 激活该选项来开启反向代理用户认证，请从 #165 了解更多信息|
|ENABLE_REVERSE_PROXY_AUTO_REGISTRATION | 激活该选项来开启反向代理用户认证的自动注册功能|
|DISABLE_MINIMUM_KEY_SIZE_CHECK | 激活该选项来禁止检查响应类型的密钥最小长度|
|ENABLE_CAPTCHA | 激活该选项以在用户注册时要求输入验证码|

**Web 钩子 (webhook)**

|名称|描述|
|:-----|:-----|
|TYPES | 启动的 Web 钩子类型，可以是 gogs、slack、discord或dingtalk|
|QUEUE_LENGTH | Web钩子队列长度，如果发现部分Webhook未能成功触发，可考虑增加该值|
|DELIVER_TIMEOUT | 发送通知的超时时间，以秒为单位|
|SKIP_TLS_VERIFY | 指示是否允许向具有非信任证书的地址发送通知|
|PAGING_NUM | Web 钩子历史页面每页显示记录条数|

**邮件 (mailer)**

|名称|描述|
|:-----|:-----|
|ENABLED | 启用该选项以激活邮件服务|
|SUBJECT_PREFIX | 邮件标题的前缀|
|HOST | SMTP 主机地址与端口|
|DISABLE_HELO | 禁用 HELO 操作|
|HELO_HOSTNAME | HELO 操作的自定义主机名|
|SKIP_VERIFY | 不验证自签发证书的有效性|
|FROM | 邮箱的来自地址，遵循 RFC 5322规范，可以是一个单纯的邮箱地址或者 "名字" <email@example.com> 的形式|
|USER | 邮箱用户名|
|PASSWD | 邮箱密码|
|USE_PLAIN_TEXT | 使用 text/plain 作为邮件内容格式|

备注：Gogs 仅支持使用 STARTTLS 的 SMTP 协议

**缓存 (cache)**

|名称|描述|
|:-----|:-----|
|ADAPTER | 缓存引擎适配器，可以为 memory、redis 或 memcache。如果您使用 redis 或 memcache，请确保使用 -tags 选项重新构建所有依赖，例如：go build -tags='redis'|
|INTERVAL | 仅限内存缓存使用，GC 周期，单位为秒|
|HOST | 仅限 redis 和 memcache 使用，主机地址和端口号|
|- | Redis：network=tcp,addr=127.0.0.1:6379,password=macaron,db=0,pool_size=100,idle_timeout=180|
|- | Memache：127.0.0.1:9090;127.0.0.1:9091|

**会话 (session)**

|名称|描述|
|:-----|:-----|
|PROVIDER | Session 引擎提供者，可以是 memory、file、redis 或 mysql|
|PROVIDER_CONFIG | 如果提供者为 file，则为文件根目录；如果为其它提供者，则为主机地址和端口号|
|COOKIE_SECURE | 激活该选项以要求所有 session 操作均通过 HTTPS|
|GC_INTERVAL_TIME | GC 周期，单位为秒|

**图片 (picture)**

|名称|描述|
|:-----|:-----|
|AVATAR_UPLOAD_PATH | 存放用户上传头像的目录|
|GRAVATAR_SOURCE | 可以是 gravatar、duoshuo 或任何 URL，例如：http://cn.gravatar.com/avatar/|
|DISABLE_GRAVATAR | 激活该选项来仅使用本地头像|
|ENABLE_FEDERATED_AVATAR | 激活该选项来启用 Federated 头像服务（http://www.libravatar.org），当 Gravatar 被禁用时此选项无法生效|

**附件 (attachment)**

|名称|描述|
|:-----|:-----|
|ENABLED | 激活该选项以允许用户上传附件|
|PATH | 存放附件的路径|
|ALLOWED_TYPES | 允许上传的 MIME 类型，例如 “image/jpeg|image/png”，使用 */* 允许所有类型的文件|
|MAX_SIZE | 最大允许上传的附件体积，单位为 MB，例如 4|
|MAX_FILES | 最大允许一次性上传的附件个数，例如 5|

**时间 (time)**

|名称|描述|
|:-----|:-----|
|FORMAT | 指定日期的输出格式，默认为 RFC1123，其它可选的格式为 ANSIC、UnixDate、RubyDate、RFC822、RFC822Z、RFC850、RFC1123、RFC1123Z、RFC3339、RFC3339Nano、Kitchen、Stamp、StampMilli、StampMicro 和 StampNano。访问 http://golang.org/pkg/time/#pkg-constants 查看详情|

**日志 (log)**

|名称|描述|
|:-----|:-----|
|ROOT_PATH | 日志文件的根目录|
|MODE | 日志记录模式，默认为 console。如果想要开启多模式，请使用逗号分割，例如："console, file"|
|LEVEL | 基本日志级别，默认为 Trace|

**日志 - 控制台 (log.console)**

|名称|描述|
|:-----|:-----|
|LEVEL | 控制台日志级别，留空则继承父值|

**日志 - 文件 (log.file)**

|名称|描述|
|:-----|:-----|
|LEVEL | 文件日志级别，留空则继承父值|
|LOG_ROTATE | 激活该选项以启用日志文件自转|
|DAILY_ROTATE | 激活该选项以进行日常自转|
|MAX_SIZE_SHIFT | 自转需要达到的最大文件体积，使用位左移，默认为 28 即 1 << 28，表示 256MB|
|MAX_LINES | 自转需要达到的最大文件行数，默认为 1000000|
|MAX_DAYS | 保留自转文件的最长期限，默认为 7 天后删除|

**日志 - Slack (log.slack)**

|名称|描述|
|:-----|:-----|
|LEVEL | Slack 日志级别，留空则继承父值|
|URL | Slack Web 钩子 URL|

**日志 - Discord (log.discord)**

|名称|描述|
|:-----|:-----|
|LEVEL | Discord 日志级别，留空则继承父值|
|URL | Discord Web 钩子 URL|
|USERNAME | 在 Web 钩子中显示的用户名|

**Cron (cron)**

|名称|描述|
|:-----|:-----|
|ENABLED | 激活该选项以允许周期性运行 Cron 任务|
|RUN_AT_START | 激活该选项以允许在启动时执行 Cron 任务|

**Cron - 更新镜像 (cron.update_mirrors)**

|名称|描述|
|:-----|:-----|
|SCHEDULE | 定时更新仓库镜像的 Cron 语法，例如：@every 1h|

**Cron - 仓库健康检查 (cron.repo_health_check)**

|名称|描述|
|:-----|:-----|
|SCHEDULE | 定时进行仓库健康检查的 Cron 语法，例如：@every 24h|
|TIMEOUT | 仓库健康检查超时的定义语法，例如：60s|
|ARGS | git fsck 命令的参数，例如：--unreachable --tags|

**Cron - 仓库统计检查 (cron.check_repo_stats)**

|名称|描述|
|:-----|:-----|
|RUN_AT_START | 激活该选项以在启动时执行仓库统计检查|
|SCHEDULE | 定时进行仓库统计检查的 Cron 语法，例如：@every 24h|

**Cron - 仓库归档清理 (cron.repo_archive_cleanup)**

|名称|描述|
|:-----|:-----|
|RUN_AT_START | 激活该选项以在启动时执行仓库归档清理|
|SCHEDULE | 定时进行仓库归档清理的 Cron 语法，例如：@every 24h|
|OLDER_THAN | 仓库归档的文件有效期，过期的归档将被清理，例如：24h|

**Git (git)**

|名称|描述|
|:-----|:-----|
|DISABLE_DIFF_HIGHLIGHT | 激活该选项以禁用行内差异高亮|
|MAX_GIT_DIFF_LINES | 差异对比页面单个文件显示的最大行数|
|MAX_GIT_DIFF_LINE_CHARACTERS | 差异对比页面单行显示的最大字符数|
|MAX_GIT_DIFF_FILES | 差异对比页面文件显示的最多个数|
|GC_ARGS | git gc 命令的参数，例如：--aggressive --auto|

**Git - 超时 (git.timeout)**

|名称|描述|
|:-----|:-----|
|MIGRATE | 仓库迁移操作超时，默认为 600 秒|
|MIRROR | 仓库镜像同步操作超时，默认为 300 秒|
|CLONE | 仓库克隆操作超时，默认为 300 秒|
|PULL | 仓库拉取操作超时，默认为 300 秒|
|GC | 仓库垃圾回收操作超时，默认为 60 秒|

**UI (ui)**

|名称|描述|
|:-----|:-----|
|EXPLORE_PAGING_NUM | 探索页面每页显示仓库的数量|
|ISSUE_PAGING_NUM | 每页显示工单（Issue）的数量（应用到所有以列表形式显示工单的页面）|
|FEED_MAX_COMMIT_NUM | 一条最新活动中显示代码提交（Commit）的最大数量|
|THEME_COLOR_META_TAG | 被用于 Android >= 5.0 版本 “theme-color” 标记的值，无效的值将被忽略并使用默认值（查看详情）|
|MAX_DISPLAY_FILE_SIZE | 显示到页面的最大文件体积（Byte）|

**UI - Admin (ui.admin)**

|名称|描述|
|:-----|:-----|
|USER_PAGING_NUM | 用户管理页面每页显示记录条数|
|REPO_PAGING_NUM | 仓库管理页面每页显示记录条数|
|NOTICE_PAGING_NUM | 系统提示管理页面每页显示记录条数|
|ORG_PAGING_NUM | 组织管理页面每页显示记录条数|

**Prometheus (prometheus)**

|名称|描述|
|:-----|:-----|
|ENABLED | 激活该选项以启用 Prometheus 监控信息，默认为 true|
|ENABLE_BASIC_AUTH | 激活该选项以启用 HTTP 基本认证保护监控信息数据，默认为 false|
|BASIC_AUTH_USERNAME | HTTP 基本认证的用户名|
|BASIC_AUTH_PASSWORD | HTTP 基本认证的密码|

**其他 (other)**

|名称|描述|
|:-----|:-----|
|SHOW_FOOTER_BRANDING | 激活该选项以在页脚显示 Gogs 推广信息|
|SHOW_FOOTER_VERSION | 激活该选项以在页脚显示 Gogs 版本信息|
|SHOW_FOOTER_TEMPLATE_LOAD_TIME | 激活该选项以在页脚显示 Gogs 模板加载时间|

### 参考资料

- [官方文档](https://gogs.io/docs)
- [历史版本](https://github.com/gogs/gogs/releases)
- [使用 Gogs 搭建自己的 Git 服务器](https://blog.mynook.info/post/host-your-own-git-server-using-gogs/)
- [注入自定义头部和尾部](https://discuss.gogs.io/t/how-to-inject-custom-head-and-footer/943)
- [Gogs在windows上的安装和配置](https://o-r.github.io/2018/05/12/Gogs%E5%9C%A8windows%E4%B8%8A%E7%9A%84%E5%AE%89%E8%A3%85%E5%92%8C%E9%85%8D%E7%BD%AE/)
- [gogs配置详解](https://dashdream.github.io/p/7dd2a61d.html)
- [win10操作系统Gogs配置解决ssh方式git免密下载上传问题](https://www.cxyzjd.com/article/e891377/109302884)
