---
title: Hexo 博客搭建教程 Ⅱ
avatar: /images/avatar.jpeg
xauthor: Hesperus｜Venus
cover: 'http://wx3.sinaimg.cn/large/0069lHOigy1g2g628veuqj30b40b4dfn.jpg'
photos: 'http://wx3.sinaimg.cn/large/0069lHOigy1g2g628veuqj30b40b4dfn.jpg'
date: 2020-02-20 15:24:01
authorLink: hehuadong.cn
authorAbout: 
authorDesc: 
comments: true
tags: 
  - /
  - blog
keywords: Hexo
description: Hexo 选择巴适的主题 (￣▽￣)~*
---

## 安装主题

首先在<font color="#2add9c">[这里](https://hexo.io/themes/)</font>选择一个主题
以[ParticleX](https://github.com/argvchs/hexo-theme-particlex)主题为参考

 · 进入`themes`文件夹: `cd theme`
 · 克隆主题到`themes`文件夹: `git clone https://github.com/argvchs/hexo-theme-particlex.git themes\particlex`
 · 修改根目录下的_config.yml配置主题: `theme: landscape`  ➜  `theme: particlex`
 · 生成静态资源，启动服务: `hexo g` `hexo s`
 · 访问页面`localhost:4000` 默认端口号: 4000；s 当端口号被占用，可使用命令: `hexo s -p 3000`
 · 切换主题：`themes`文件夹下安装多个主题，只需修改根目录下`_config.yml` <font color="#2add9c">theme</font> 参数即可切换主题

### 主题目录介绍
```bash
<root>
|---public # 静态网页文件
|---source # 资源文件夹,md文章存放处（不要和主题文件夹下的source弄混）
|---themes # 主题
    |---<theme-name>
        |---layout # 布局文件
        |   |---layout.ejs # 网页的基本布局
        |---source # 主题资源文件，里面的内容会生成到静态文件下

```

### [EJS](https://ejs.bootcss.com/#promo)模板语言

EJS 是一套简单的模板语言，帮你利用普通的 JavaScript 代码生成 HTML 页面。
hexo初始化建站，默认安装依赖`hexo-renderer-ejs` ,`hexo-renderer-stylus`
一般主题使用什么模板语言开发，博主使用对应的模板语言，减少不必要的代码重构

```ejs
// 基础使用
<% if (user) { %>
  <h2><%= user.name %></h2>
<% } %>

/*--  导入文件  --*/
<%- partial('partial/header') %>
<div id="content">Home page</div>
```
<b>常用标签含义</b>

<div class="table-wrapper">

| 标签           | 含义、作用      |
| ------------- | ------------- |
| <%            | '脚本' 标签，用于流程控制，无输出。  |
| <%_           | 删除其前面的空格符  |
| <%=           | 输出数据到模板（输出是转义 HTML 标签） |
| <%-           | 输出非转义的数据到模板  |
| <%#           | 注释标签，不执行、不输出内容  |
| <%%           | 输出字符串 '<%'  |
| %>            | 一般结束标签  |
| -%>           | 删除紧随其后的换行符  |
| _%>           | 将结束标签后面的空格符删除  |

</div>

### 自定义网站配置
打开博客根目录下的 `_config.yml`，下面是主要参数的介绍

```yaml
#----  站点  ----#
title: # 标题
subtitle: # 副标题
description: # 描述
keywords: # 关键字
author: # 作者
language: # 语言
timezone: # 时区

#----- 路由路径 -----#
url: # 网址
root: # 根目录
permalink: # 文章链接格式
permalink_defaults: # 链接默认值
pretty_urls:
  trailing_index: # 是否在永久链接中保留尾部的 index.html，设置为 false 时去除
  trailing_html: # 是否在永久链接中保留尾部的 .html, 设置为 false 时去除 (对尾部的 index.html无效)

#----- 文件目录  -----#
source_dir: # 资源文件夹，这个文件夹用来存放内容
public_dir: # 公共文件夹，这个文件夹用于存放生成的站点文件
tag_dir: # 标签文件夹
archive_dir:  # 归档文件夹
category_dir: # 分类文件夹
code_dir: # Include code 文件夹，source_dir 下的子目录
i18n_dir: # 国际化（i18n）文件夹
skip_render: # 跳过指定文件的渲染。匹配到的文件将会被不做改动地复制到 public 目录中 

#------  文章  -------#
new_post_name:  # 新文章的文件名
default_layout:  # 预设布局
titlecase: false # 标题转化为首字母大写
external_link:
  enable:  # 开启，新的标签页打开外部链接
  field:  # 适用于整个站点, 设置 post, 仅对文章（post）生效
  exclude:  # 需要排除的域名。主域名和子域名如 www 需分别配置
filename_case: 0 # 把文件名称转换为 (1) 小写或 (2) 大写

#-------  首页文章配置  -------#
index_generator:
  per_page:  # 每页显示的帖子。(0 =禁用分页)
  order_by:  # 文章的顺序。(默认按日期递减排序)

#------  日期 / 时间格式  ------#
date_format: YYYY-MM-DDs
time_format: HH:mm:ss

#------  分页  ------#
per_page: # 每页显示的文章量 (0 = 关闭分页功能)
pagination_dir: # 分页目录

#----  包括 / 不包括目录或文件  ----#
include: # 包括隐藏文件
exclude: # 排除文件/文件夹
ignore: # 忽略文件/文件夹

#------  主题  -----#
theme: #主题

#------  部署  -----#
deploy:
  type: # 部署方式
  repo: # 部署仓库
    coding: # 部署的仓库地址
  branch: # 部署分支

```