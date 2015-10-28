title: "TYPlayer - 为网站添加视频背景"
date: 2015-07-22 21:53:52
tags: ["Background Vedio", "Youtube"]
categories: "MISC"
---

![You Tube](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-723be3ca1ce754fc_zps26e4w1pc.png)

最近突然想把网站的背景图片换成视频玩一玩，瞬间觉得高大上有木有！虽然...loading的有点慢...不过也没有深入研究，只是在Github看到了这个插件。觉得很好用，大牛绕过，小白们走起

<!-- more -->
duang duang duang 就是它 - [jquery.mb.YTPlayer](https://github.com/pupunzi/jquery.mb.YTPlayer/wiki)
* 视频效果要基于一个web server， 也就是说本地的话需要MAMP, WAMP之类的....直接把`index.html`拽到浏览器里面是不行的
* 给予你充分的创作空间，只要是youtube上的视频都能使用。可以作为整个网站背景，或者仅仅header背景等等
* 兼容多种浏览器

# 首先， head里添加
```
<link href="css/YTPlayer.css" media="all" rel="stylesheet" type="text/css">

<script src="http://ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.js"></script>
<script src="inc/jquery.mb.YTPlayer.js"></script>
```

# JS call:
```
$(function(){
      $(".player").YTPlayer();
    });
```

# HTML constructor:
```
<a id="bgndVideo" class="player" data-property="{videoURL:'http://youtu.be/BsekcY04xvQ',containment:'body',autoPlay:true, mute:true, startAt:0, opacity:1}">My video</a>
```

* 初始化一个最简单的播放器
data-property中的`containment`tag设置为`self`

* 初始化一个背景播放器
data-property中的`containment`tag设置为`body`, `autoPlay`设置为`true`，`opacity`设置为`1`

* 初始化某个HTML元素的背景播放器
data-property中的`containment`tag设置为`#ElementID`, `opacity`设置为`1`

# data-property中还有更多已定义设置
举几个常用的：
* **mute**: `true`或者`false` 设置是否禁声
* **showControls**: `true`或者`false` 设置是否显示控制栏
* **quality**: `default, small medium, large, hd720, hd1080, highres` 设置视频的清晰度
* **opacity**: `0-1` 设置视频的透明度
* **loop**: `true`或`false` 设置视频结束后是否要循环
* **startAt**: `0-20` 设置视频起始时间点，时间被分成20等份
* **stopAt**: `1-20` 设置视频结束时间点，时间被分成20等份 
* **autoPlay**: `true`或者`false` 设置是否自动播放

# 浏览器支持
* Mozilla Firefox 2+
* Google Chrome
* Opera
* Safari
* IE 6+