---
title: UglifyJS
avatar: /images/avatar.jpeg
comments: true
author: Hesperus｜Venus
cover: 'http://wx3.sinaimg.cn/large/0069lHOigy1g2g628veuqj30b40b4dfn.jpg'
photos: 'http://wx3.sinaimg.cn/large/0069lHOigy1g2g628veuqj30b40b4dfn.jpg'
date: 2022-09-02 14:24:01
authorLink: hehuadong.cn
tags: 
  - /
  - tools
keywords: 技术 · 工具
description: 想要拥有体积更小的js吗，UglifyJS可以帮你做到，轻轻松松压缩js！
---

### UglifyJS

UglifyJS 是一个 JavaScript 解析器、压缩器、压缩器和美化工具包。

Github: [传送门](https://github.com/mishoo/UglifyJS)
Ps: 需要`node`环境

安装：
```bash
 npm install uglifyjs -g 
 # 或
 npm install uglify-js

```

使用：
```bash
# -c  --compress  开启压缩
# -m  --mangle  混淆的名称或混淆的选项
# -o  -output <file>  输出路径、文件名称
uglifyjs <file-name>.js -c -m -o <file-name>.min.js
```

批量压缩本地js文件，保存bat文件. 适用于window
```bat
@echo off
:: 设置压缩JS文件的根目录，脚本会自动按树层次查找和压缩所有的JS
SET JSFOLDER=C:\work\test\
echo 正在查找JS文件
chdir /d %JSFOLDER%
for /r . %%a in (*.js) do (
    @echo 正在压缩 %%~a ...
    uglifyjs %%~fa  -m -o %%~fa
)
echo 完成!
pause & exit
```