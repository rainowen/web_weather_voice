# 利用爬虫和树莓派3打造自己的语音天气闹钟

forked from [woodenrobot.me](http://link.zhihu.com/?target=http%3A//woodenrobot.me/)

forked from [树莓派设置3.5mm接口输出音频](https://www.jianshu.com/p/674145fe98fa)
## 前言

前段事件给自己买了一个树莓派3 B+。买回来之后当然不能让他落灰，于是就利用学习的爬虫技术+树莓派+小音箱实现了一个定时闹钟外加语音天气播报功能

## 准备

1. 树莓派
2. 小音箱

## 环境

python 2.7.13

安装依赖的软件包


- pip install requests
- pip install BeautifulSoup4
- sudo apt-get install mplayer

## 获取所在位置天气

这里选择墨迹天气获取实时天气信息，地址： [墨迹天气](http://link.zhihu.com/?target=http%3A//tianqi.moji.com/)

进入墨迹天气的页面，墨迹天气会根据你的ip加载相应地区的天气。 这次我们主要抓取温度、天气、湿度、风力、空气质量和天气提示这几个数据。

<img src=".\resources\moji.jpg" style="zoom:100%">

## 文字转语音

主要是通过百度的文字转换语音API，地址：http://yuyin.baidu.com/#try 还可以选各种声音，调节语速。虽然它没有给出直接的api接口，但是我们利用Chrome浏览器的开发者模式可以找到api。

打开开发者模式，点击播放的按钮，在network里就可以找到刚刚发出的请。 http://tts.baidu.com/text2audio?idx=1&amp;amp;tex=1&amp;amp;cuid=baidu_speech_demo&amp;amp;cod=2&amp;amp;lan=zh&amp;amp;ctp=1&amp;amp;pdt=1&amp;amp;spd=5&amp;amp;per=4&amp;amp;vol=5&amp;amp;pit=5  就我们要找的百度文字转语音API,其中per是参数是语音的类型，spd是语速，vol是音量，而tex则是需要转换的文字。通过以下代码就可以实现将特定的文字转换为语音。

## 树莓派设置音频输出

在终端下通过raspi-config设置

```pi@raspberrypi:~ $sudo raspi-config```

<img src=".\resources\1338443-7c867ff7a906173a.png" style="zoom:100%">

移动光标选择第7项 Advanced Options 进入选项中

<img src=".\resources\1338443-679099f89127f95e.png" style="zoom:100%">

选择第4项 Audio 进入选项

<img src=".\resources\1338443-78c18f22cbec9b79.png" style="zoom:100%">

选择 Forece 3.5mm('headphone') jack。确定然后保存退出.

## 测试功能

```git clone https://github.com/rainowen/web_weather_voice.git```

```python web_weather.py```

## 实现定时播放语音

使用crontab来实现每天定时启动程序(每天8点到20点，每个一小时播报)

```0 8-20/1        * * *        pi            python ~/working/web_weather/web_weather.py```

