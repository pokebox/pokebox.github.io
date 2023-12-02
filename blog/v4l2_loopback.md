### v4l2 loopback的应用

用ffmpeg串流到本地回环设备并加上时间戳，可以当网络摄像头用。

一些参考：https://forum.odroid.com/viewtopic.php?t=30752

https://php.developreference.com/article/24015293/GStreamer+-+MJPEG+stream+to+file

从mjpg-streamer串流到设备：

```
ffmpeg -loglevel 8 -r 25 -f mjpeg -i "http://username:passwd@url:port/?action=stream" -an -c:v rawvideo -r 25 -pix_fmt yuyv422 -vf "drawtext=fontfile=/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf: text='Webcam feed %{localtime\\:%F %T}': fontcolor=white@0.9: x=7: y=5" -f v4l2 /dev/video20
```

复制串流到多个设备（video20和video21）：

```
ffmpeg -loglevel 8 -r 25 -f mjpeg -i "http://username:passwd@url:port/?action=stream" -an -c:v rawvideo -r 25 -pix_fmt yuyv422 -vf "drawtext=fontfile=/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf: text='Webcam feed %{localtime\\:%F %T}': fontcolor=white@0.9: x=7: y=5" -f v4l2 /dev/video20 -c copy -f v4l2 /dev/video21
```

串流桌面：

```
ffmpeg -f x11grab -framerate 25 -video_size 1280x720 -i :0.0+0,0 -f v4l2 /dev/video20
```

用mplayer播放：

```
mplayer tv:// -tv driver=v4l2:device=/dev/video25:fps=25:outfmt=yuy2
```

小插曲：删除已经安装的v4l2loopback模块：`sudo modprobe -r v4l2loopback`
`find "/lib/modules/$(uname -r)" | grep v4l2loopback | sudo xargs rm`