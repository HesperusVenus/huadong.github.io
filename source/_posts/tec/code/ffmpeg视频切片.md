---
title: GitHub + FFmpeg视频切片
avatar: /images/avatar.jpeg
comments: true
author: Hesperus｜Venus
cover: ""
date: 2020-07-21 13:29:01
authorLink: hehuadong.cn
tags:
  - /
  - code
keywords: 切片FFmpeg
description: GitHub + jsDelivr视频切片测试
photos: "https://cdn.jsdelivr.net/gh/HesperusVenus/assets@master/assets/images/cover/06.jpg"
---

### 前言

利用 jsDeliver + GitHub 实现免费 CDN 加速静态资源，例如图片、CSS、JS 等，相信大家都知道。也有人想过存放视频，但是 jsDeliver 不支持加载超过 20M 的资源，所以视频需要压缩到 20M 以下。如果想要放部电影，那就需要用到 HLS 切片 了。这里我用了 b 站下载的视频素材做测试，整部视频大小为 176M，以 10S 为一段共切了 38 段。

> GitHub 并没有太限制项目库大小，所以理论来讲只要保证不会受到 GitHub 警告，你就拥有无限空间。
> 当你的版本号写为 master 时，只需要第一次发布 release 即可，后面直接用 master 分支的文件，也没有 50MB 的文件包大小限制

### 播放测试

{% raw %}

<div id="dplayer"></div>
  <script>
    var dp = new DPlayer({
        container: document.getElementById('dplayer'),
        autoplay: false,
        theme: '#1bd1a5',
        loop: true,
        lang: 'zh-cn',
        screenshot: true,
        hotkey: true,
        preload: 'auto',
        logo: '/images/favicon.ico',
        volume: 0.7 ,
        video: {
          url: `https://cdn.jsdelivr.net/gh/HesperusVenus/assets@master/assets/video/bg_video/全体起立！为巨人最终季献上心脏吧！/video.m3u8`,
          pic: `https://cdn.jsdelivr.net/gh/HesperusVenus/assets@master/assets/video/bg_video/全体起立！为巨人最终季献上心脏吧！/pic.jpg`,
          type: 'customHls',
          customType: {
            customHls: function (video, player) {
                const hls = new Hls();
                hls.loadSource(video.src);
                hls.attachMedia(video);
            },
          },
        },
        // subtitle: {
        //   url: 'dplayer.vtt',
        //   type: 'webvtt',
        //   fontSize: '25px',
        //   bottom: '10%',
        //   color: '#b7daff',
        // },
        danmaku: {
          id: '797072183',
          api: 'https://dplayer.moerats.com/',
          // token: 'tokendemo',
          maximum: 10000000,
          addition: ['https://dplayer.moerats.com/v3/bilibili?aid=797072183&cid=236040406'],
          bottom: '15%',
          unlimited: true,
        },
        contextmenu: [
          {
              text: 'custom1',
              link: 'https://github.com/DIYgod/DPlayer',
          },
          {
              text: 'custom2',
              click: (player) => {
                  console.log(player);
              },
          },
        ],
        highlight: [
          {
            time: 20,
            text: '这是第 20 秒',
          },
          {
            time: 120,
            text: '这是 2 分钟',
          },
        ],
      });
    $(document).on('pjax:start', function () {
      console.log("dplayer","pjax")
      if (window.dplayers) {
          for (let i = 0; i < window.dplayers.length; i++) {
              window.dplayers[i].destroy();
          }
          window.dplayers = [];
      }
    });
  </script>
{% endraw %}

### [FFmpeg](https://ffmpeg.org/)切片工具

1. 进入 [这里](https://ffmpeg.org/download.html) 下载切片工具
2. 对视频进行转码(转为 mp4)，将视频文件转为视频编码 h.264，音频编码 aac 格式的 mp4 文件，mp4 视频文件不是 h.264 编码到后面切片的时候可能会遇到很多莫名其妙的问题

```bash
# infile.mp4 是待转码的文件(可以是其他格式，比如 avi…… 之类的)
# outfile.mp4 是转码输出文件
# libx264 转为 h.264 编码

ffmpeg -i infile.mp4  -c:v libx264 -strict -2 outfile.mp4
```

3. 将 mp4 切片，并生成 m3u8 文件

```bash
# output.mp4 需要切片的视频文件
# playlist.m3u8 待生成的 m3u8 文件名
# 5 切片时间，表示隔几秒进行切一个文件
# output%03d.ts 生成切割ts文件名，output%03d.ts 代表生成 output001.ts、output002.ts 这样的格式，03d 可以随意修改，占位符

ffmpeg -i output.mp4 -c copy -map 0 -f segment -segment_list playlist.m3u8 -segment_time 5 output%03d.ts
```

4. 这样就算切片成功了，视频被切割成你想要长度的 ts 文件，只要低于 20M 就可以放入 GitHub 了
   ![ffmpeg切片](https://cdn.jsdelivr.net/gh/HesperusVenus/assets@master/assets/images/_posts/tec/ffmpeg切片.png)

### [Github](https://github.com/)建库

进入 Github 主页,点击右边的按钮 <font color="#2add9c"> New </font>,新建一个仓库<name>.
默认分支以前是 `master`，现在是 `main`，个人觉得 `master` 比较主流,也是码农常用的分支.
创建新分支命名为 `master`,设置为默认分支,删除 `main` 分支. 因为 `master` 分支将作为发版分支使用!

P.s.: 由于视频资源内存过大,不建议放置项目本地存放,将资源提交到GitHub中存放,通过CDN引入资源使用

```bash
# clone <name>
git clone https://github.com/xxx/<name>

# 进入目标文件夹
cd <name>

# 创建文件夹videos
mkdir videos
```

> 将上述切片后生成的.m3u8 文件和.ts 文件挪动到 videos 文件夹下,git 提交到远程仓库中.
> 进入 Github <name> 仓库中,
> 点击进入 Releases,填写发布版本号,如 v1.01,
> 选择需要发版的分支,备注可填,
> 点击底部按钮 <font color="#2add9c">Publish release</font>,即可发版成功.

### [DPlayer](https://dplayer.diygod.dev/zh/)

DPlayer是一个可爱的HTML5弹幕视频播放器，帮助人们轻松构建视频和弹幕。

安装  `npm install dplayer --save` or `yarn add dplayer`

```javascript
<div id="dplayer"></div>

// 获取视频容器的id
const dp = new DPlayer({
    container: document.getElementById('dplayer'),
    video: {
        url: 'https://cdn.jsdelivr.net/gh/xxx/<name>@master/videos/video.m3u8',
    },
});
```
<i>个人使用的参数表, 详细见 <font color="#2add9c">文档</font></i>

<div class="table-wrapper">

| 名称 | 默认值 | 描述 |
| ---------------- | ----------------------------------- | ------------- | 
| container        | document.querySelector('#dplayer')  | 播放器容器元素  |
| autoplay         | false                               | 自动播放       |
| theme            | #b7daff                             | 主题色         |
| loop             | true                                | 循环播放       |
| lang             | navigator.language.toLowerCase()	   | 'zh-cn'       |
| screenshot       | true                                | 截图; 开启,视频和封面需允许跨域 |
| hotkey           | true                                | 热键, 快进、快退、音量、暂停播放 |
| preload          | 'auto'                              | 视频预加载      |
| logo             | -                          	       | 左上角logo,css可控制样式 |
| volume           | 0.7          	                     | 音量          |
| video            | -                                	 | 视频信息       |
| video.url        | -                                	 | 视频链接       |
| video.pic        | -                                	 | 视频封面       |
| video.type       | 'auto'                              | 配置太多,见文档 |
| video.customType | -                               	   | 自定义类型,见文档 |
| subtitle         | -                                	 | 外挂字幕,见文档 |
| danmaku          | -                                	 | 显示弹幕       |
| danmaku.id       | -                                	 | 个人使用的bilibili的播放视频的id:797072183,即弹幕的id |
| danmaku.api      | -                                	 | [弹幕接口]('https://dplayer.moerats.com/') |
| danmaku.token    | -                                	 | 显示弹幕       |
| danmaku.maximum  | -                                	 | 弹幕后端验证 token |
| danmaku.addition | -                                	 | 额外弹幕,见[#bilibili 弹幕](https://dplayer.diygod.dev/zh/guide.html#%E5%BC%B9%E5%B9%95%E6%8E%A5%E5%8F%A3) ps:['https://dplayer.moerats.com/v3/bilibili?aid=797072183&cid=236040406']|
| danmaku.bottom   | -                                	 | 弹幕距离播放器底部的距离，防止遮挡字幕;取值如'10px' '10%' |
| danmaku.unlimited| false                               | 海量弹幕       |
| contextmenu      | -                                	 | 自定义右键菜单  |
| highlight        | -                                	 | 自定义进度条提示点 |

</div>

> 