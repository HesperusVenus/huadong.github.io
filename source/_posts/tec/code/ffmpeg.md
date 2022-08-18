---
title: GitHub + jsDelivr视频切片测试
avatar: /images/avatar.jpeg
comments: true
author: Hesperus｜Venus
cover: 'https://m1314.cn/wp-content/themes/Sakura/images/random/d-1.jpg'
date: 2020-07-21 13:29:01
authorLink: hehuadong.cn
tags: 
  - /
  - code
keywords: 切片FFmpeg
description: GitHub + jsDelivr视频切片测试
photos: 'https://cdn.jsdelivr.net/gh/HesperusVenus/assets@master/assets/img/cover/06.jpg'

---
  利用 jsDeliver + GitHub 实现免费 CDN 加速静态资源，例如图片、CSS、JS等，相信大家都知道。也有人想过存放视频，但是 jsDeliver 不支持加载超过 20M 的资源，所以视频需要压缩到 20M 以下。如果想要放部电影，那就需要用到 HLS切片 了。这里我用了b站下载的视频素材做测试，整部视频大小为 176M，以 10S 为一段共切了 38 段。

  GitHub 并没有太限制项目库大小，所以理论来讲只要保证不会受到 GitHub 警告，你就拥有无限空间。

  PS：当你的版本号写为master时，只需要第一次发布release即可，后面直接用master分支的文件，也没有50MB的文件包大小限制

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
        mutex: true,
        video: {
          // url: `https://cdn.jsdelivr.net/gh/HesperusVenus/assets@master/assets/video/bg_video/全体起立！为巨人最终季献上心脏吧！/video.m3u8`,
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
        subtitle: {
        url: 'dplayer.vtt',
        type: 'webvtt',
        fontSize: '25px',
        bottom: '10%',
        color: '#b7daff',
        },
        danmaku: {
          id: '797072183',
          api: 'https://dplayer.moerats.com/',
          token: 'tokendemo',
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

### 切片命令

 我使用的 [FFmpeg](https://github.com/FFmpeg/FFmpeg) 工具来切片

 1. 对视频进行转码(转为 mp4)，将视频文件转为视频编码 h.264，音频编码 aac 格式的 mp4 文件，mp4 视频文件不是 h.264 编码到后面切片的时候可能会遇到很多莫名其妙的问题

```bash
# infile.mp4 是待转码的文件(可以是其他格式，比如 avi…… 之类的)
# outfile.mp4 是转码输出文件
# libx264 转为 h.264 编码
 
ffmpeg -i infile.mp4  -c:v libx264 -strict -2 outfile.mp4
```

 2. 将 mp4 切片，并生成 m3u8 文件

```bash
# output.mp4 需要切片的视频文件
# playlist.m3u8 待生成的 m3u8 文件名
# 5 切片时间，表示隔几秒进行切一个文件
# output%03d.ts 生成切割ts文件名，output%03d.ts 代表生成 output001.ts、output002.ts 这样的格式，03d 可以随意修改，占位符
 
ffmpeg -i output.mp4 -c copy -map 0 -f segment -segment_list playlist.m3u8 -segment_time 5 output%03d.ts
```

 3. 这样就算切片成功了，视频被切割成你想要长度的 ts 文件，只要低于 20M 就可以放入 GitHub 了
![ffmpeg切片](/images/ffmpeg切片.png)

---