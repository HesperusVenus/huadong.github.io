---
title: Hexo 博客搭建教程 Ⅰ
avatar: /images/avatar.jpeg
xauthor: Hesperus｜Venus
cover: 'http://wx3.sinaimg.cn/large/0069lHOigy1g2g628veuqj30b40b4dfn.jpg'
photos: 'http://wx3.sinaimg.cn/large/0069lHOigy1g2g628veuqj30b40b4dfn.jpg'
date: 2020-02-20 13:24:01
authorLink: hehuadong.cn
authorAbout: 
authorDesc: 
comments: true
tags: 
  - /
  - hexo
keywords: Hexo
description: Hexo 快速建站不等待ψ(｀∇´)ψ
---

  > 前言: Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页，即把用户的markdown文件，按照指定的主题解析成静态网页。
  > 学习 Markdown 语法可以看 Github 官方文档：[基本撰写和格式语法](https://docs.github.com/cn/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/about-writing-and-formatting-on-github)

### 安装 Git 和 Node.js
  - Git
    从 https://git-scm.com/downloads 下载ss
    完成后用 `git --version` 命令检查，有提示即安装正确
  - Node.js
    从 https://nodejs.org 下载 Node.js，选择 LTS 稳定版本，安装一路确认即可
    完成后用 `node -v` 和 `npm -v` 两个命令检查，有提示即安装正确

### Github建库

如果你希望你的站点部署到域名 `<name>.github.io` 访问，新建一个仓库，命名为 `<name>.github.io`；
远程项目拉取到本地：`git clone https://github.com/xxx/<name>.github.io.git`

> GitHub新建项目默认分支main,可创建release分支,默认分支切换至release
> `release`: hexo建站的项目文件;
> `main`: 用于发布,打包文件资源指向到该分支, GitHub Pages配置站点页面,分支也需要切换至main分支,配置个人域名即可访问;

### [Hexo](https://hexo.io/zh-cn/)建站
输入命令`npm install -g hexo-cli`,检查安装`hexo -v`，进入`<name>.github.io`文件中，执行`hexo init`初始化新建一个网站。

<b>hexo初始化目录结构：</b>
```bash
<root>
├── _config.landscape.yml # 默认主题landscape配置文件；若配置不同的主题,可删除该文件
├── _config.yml    # 站点配置文件
├── package.json   # hexo依赖,landscape依赖不要删除,hexo默认主题,删除即报错
├── scaffolds      # 模版文件夹
│   ├── draft.md
│   ├── page.md
│   └── post.md
├── source         # 资源文件夹,即makedown文章存放处
│   └── _posts
│       └── hello-world.md
├── themes         # 主题文件夹          
└── yarn.lock
```

Ps: 注意hexo版本安装对应node版本

### Hexo的常用命令
  · 生成静态资源： `hexo g` 或  `hexo generate`
  · 清除静态资源： `hexo cl` 或 `hexo clean`
  · 本地运行： `hexo s` 或 `hexo server`
  · 部署到网站： `hexo d` 或 `hexo deploy`
  · 生成静态文件并部署到网站： `hexo d -g` 或 `hexo g -d`
  · 创建新文章：`hexo new <post-name>`  `<post-name>`为文件名

> 个人习惯：因自建菜单目录较多，本地写好文章，放到对应的文件夹下归档，执行上述命令！
