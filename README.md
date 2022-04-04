# 24h-Streaming
24小时推流直播

推流的命令
```
ffmpeg -re -i 阳光电影www.ygdy8.com.侍神令.2021.HD.1080P.国语中英双字.mp4 -vcodec copy -acodec copy -f flv rtmp://192.168.123.144/live/Skn0AyEtO
```

本地播放命令：
```
ffplay rtmp://127.0.0.1/live/Skn0AyEtO
```
```
tmux new-session -t itgoyo

tmux attach-session -t itgoyo
```

无缝播放命令：
```
ffmpeg -re -f concat -safe 0 -i video.txt -vcodec copy -acodec copy -f flv rtmp://192.168.123.144/live/Skn0AyEtO
```

但是上面的只能执行一次，我们需要循环执行，这样子就衍生出了下面这种方式
!!! **直播地址的双引号**很重要""

```
#!/bin/bash
###
 # @Description:自动循环推流脚本
 # @Author: itgoyo
 # @E-mail: itgoyo@gmail.com
 # @Github: itgoyo
###

while true

do

ffmpeg -re -f concat -safe 0 -i video.txt -vcodec copy -acodec copy -f flv "rtmp://live-push.bilivideo.com/live-bvc/?streamname=live_12767066_6265400&key=0c32218670c24b8ea4bb89e45d14b6c2&schedule=rtmp&pflag=1"

done
```

如果你想终止的话，以往的命令是终止不了的，只能先` ps -ef`,找到推流ffmpeg是那个端口，然后使用
`kill -9 端口来终止

如果是在服务器上面的话，ps找不到后台运行的端口号的话，可以使用`ps -ef|grep nohup`或者使用`ps aux &`来查看，一般都是`ffmpeg`的端口号干掉就可以了


![](https://cdn.jsdelivr.net/gh/itgoyo/PicGoRes@master/img/20210520225112.png)

# 加水印

```
#!/bin/bash
###
 # @Description:自动循环推流脚本
 # @Author: itgoyo
 # @E-mail: itgoyo@gmail.com
 # @Github: itgoyo
###

while true

do

ffmpeg -re -f concat -safe 0 -i video.txt -vf "movie=/Users/itgoyo/Downloads/logo.jpg [watermark]; [in][watermark] overlay=10:10 [out]" -c:v libx264 -c:a copy -f flv "rtmp://127.0.0.1/live/HyqY_aVDt"

done
```


