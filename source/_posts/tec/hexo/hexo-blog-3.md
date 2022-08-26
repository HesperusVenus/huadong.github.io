---
title: Hexo 博客搭建教程 Ⅲ
avatar: /images/avatar.jpeg
xauthor: Hesperus｜Venus
cover: 'http://wx3.sinaimg.cn/large/0069lHOigy1g2g628veuqj30b40b4dfn.jpg'
photos: 'http://wx3.sinaimg.cn/large/0069lHOigy1g2g628veuqj30b40b4dfn.jpg'
date: 2020-02-22 14:42:00
authorLink: hehuadong.cn
authorAbout: 
authorDesc: 
comments: true
tags: 
  - /
  - hexo
keywords: Hexo
description: 炫酷的样式，多样化的功能！买定离手，CV拿走|●´∀`|σ
---

## # 添加Js/Css/Fonts

在 `<root>\themes\<theme-name>\source` 下添加自定义文件(根据个人习惯资源归类，方便查找)，然后在使用的地方添加如下内容：

```html
<!--CSS-->
<link rel="stylesheet" href="/<file-name>" />

<!--JS-->
<script src="/<file-name>"></script>

```

## # 我使用的自定义文件
#### Aplayer
· 概述：APlayer 是一个可爱的 HTML5 音乐播放器
· 依赖：需要 [Meting.js](https://github.com/metowolf/MetingJS) 支持
· 下载：[GitHub 传送门](https://github.com/DIYgod/APlayer#readme)
· 文档：[传送门](https://aplayer.js.org/#/home)

#### Dplayer
· 概述：DPlayer是一个可爱的HTML5弹幕视频播放器，帮助人们轻松构建视频和弹幕。
· 依赖：需要 [Meting.js](https://github.com/metowolf/MetingJS) 支持
· 下载：[GitHub 传送门](https://github.com/DIYgod/DPlayer)
· 文档：[传送门](https://dplayer.diygod.dev/zh/)
· 工具：[FFmpeg](https://ffmpeg.org/)切片工具
· 文章：<a href="/tec/code/ffmpeg视频切片">Dplayer + ffmpeg视频切片</a>

#### Valine
· 概述：一款快速、简洁且高效的无后端评论系统。
· 特点：快速、安全、Emoji😄、无后端实现、`Markdown` 支持、文章阅读量统计
· 依赖：依赖于`LeanClound`应用，需要登陆/注册来获取 App ID 和 App Key
· 应用：[LeanClound 传送门](https://console.leancloud.cn/login?from=%2Fapps)
· 文档：[传送门](https://valine.js.org/quickstart.html)
· 使用：
```html
<head>
    ..
    <script src='//unpkg.com/valine/dist/Valine.min.js'></script>
    ...
</head>
<body>
    ...
    <div id="vcomments"></div>
    <script>
        new Valine({
            el: '#vcomments',
            appId: 'Your appId',
            appKey: 'Your appKey'
        })
    </script>
</body>

```
#### Live2D看板酱

使用live2d看板酱，建议live2d模型资源文件放置Github资源仓库中，通过CDN的方式引入使用

##### live2d模型站（可以选择你喜欢的模型）：https://mx.paul.ren/
##### 项目地址：
Typecho 版（保罗的小宇宙做的）
· 特点：自带 Pio 模型，支持后台更换其他模型，还可自定义换肤，可放在插件目录下或外部引用
· 依赖：无
· 下载：[GitHub 传送门](https://github.com/Dreamer-Paul/Pio)
· 文档：[传送门](https://docs.paul.ren/pio/#/)
 
WordPress 版（喵喵做的）
· 特点：自带 2233 模型，不支持后台更换，但包含一言、文字交互等功能
· 依赖：需要 JQuery 支持
· 下载：[GitHub 传送门](https://github.com/xb2016/33-live2d-wp)

Emlog 版（广树做的）
· 特点：自带 Histoire 模型，不支持后台更换，但可以接入图灵机器人实现互动
· 依赖：需要 JQuery 支持
· 下载：[传送门](https://www.wikimoe.com/?post=75)

Z-Blog 版（FGHRSH 做的）
· 特点：自带 Pio、Tia 模型，支持换肤。不支持后台更换，但包含一言、文字交互等功能
· 依赖：需要 JQuery 支持
· 下载：[传送门](https://www.fghrsh.net/post/123.html)

##### 使用
```yaml
#live2d 看板娘
live2d:
  enable: true  # 是否开启
  model: umaru  # 看板娘模型 nero<尼洛> || umaru<干物妹小埋> || Violet<薇尔莉特> || platelet<血小板>
  mode: fixed  # 模式  static || fixed || draggable
  position: left # 位置
  hidden: true, # 隐藏
  width: 200
  height: 300
  dialog: '3em'

#CDN
CDN:  
  #live2d
  live2d: https://cdn.jsdelivr.net/gh/HesperusVenus/live2d@master/live2d.js
  live2d_piocss: https://cdn.jsdelivr.net/gh/HesperusVenus/live2d@1.01/pio.css
  live2d_piojs: https://cdn.jsdelivr.net/gh/HesperusVenus/live2d@1.01/pio.js
  live2d_model: https://cdn.jsdelivr.net/gh/HesperusVenus/live2d@latest/models

```
ejs模板语言可以使用主题目录下`_config.yml`下的配置
```ejs
<% if (theme.live2d && theme.live2d.enable){ %>
  <!-- 引用看板娘交互所需的样式表 -->
  <link href="<%- theme.CDN.live2d_piocss %>" rel='stylesheet' type='text/css'/>
   <div class="pio-container <%- theme.live2d.position %>">
    <div class="pio-action"></div>
    <div class="pio-btn"></div>
    <canvas id="pio" width="<%- theme.live2d.width %>" height="<%- theme.live2d.height %>"></canvas>
  </div>
  <!-- 引用 Live2D 核心组件 -->
  <script src="<%- theme.CDN.live2d %>"></script>
  <!-- 引用看板娘交互组件 -->
  <script src="<%- theme.CDN.live2d_piojs %>"></script>
  <!-- Live2D看板娘设置 -->
  <script>
    const live2d_model = "<%- theme.CDN.live2d_model %>/<%- theme.live2d.model %>/live2d_model.json"
    var pio = new Paul_Pio({
      "mode": "<%- theme.live2d.mode %>",
      "hidden": "<%- theme.live2d.hidden %>",
      "content": {
          "welcome": ["欢迎来到Hesperus的小宇宙！", "今天天气不错，一起来玩吧！", "博主每天都有些折腾记录，欢迎前往他的小窝阅读~"],
          "touch": ["你这个绅士！", "别碰我！"],
          "home": "点击这里回到首页！",
          "link": `${location.href}`,
          "close": "QWQ 有缘再会吧~",
          "referer": "你通过 %t 来到了这里",
          "custom": [
              {"selector": ".comment-form", "text": "欢迎参与本文评论，别发小广告噢~"},
              {"selector": ".home-social a:last-child", "text": "在这里可以了解博主的日常噢~"},
              {"selector": ".post-item a", "type": "read"},
              {"selector": ".post-content a, .page-content a", "type": "link"},
              {"selector": "#pio", "type": "", "text": ["你在干什么？", "再摸我就报警了！", "HENTAI!", "不可以这样欺负我啦！","干嘛动我呀！小心我咬你！","110吗！这里有个怪蜀黍一直在戳我 (｡ ́︿ ̀｡)","再碰我就告诉Hesperus!!!"]},
          ]
      },
      "night": "this.night()",
      "model": [ live2d_model ]
  });
  // DIY设置模态框位置
  document.querySelector('.pio-container .pio-dialog').style.top = "<%- theme.live2d.dialog %>"
  </script> 
<% } %>
```
