

依赖



```
cupertino_icons: ^0.1.2
  dio: ^3.0.10
  device_info: ^1.0.0
  amap_location: ^0.2.0
  image_picker: ^0.6.7+14
  video_player: ^0.10.0+2
  chewie: ^0.9.7
```





| 构造属性                          | 说明                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| videoPlayerController             | 视频的控制器                                                 |
| autoInitialize                    | 在启动时初始化视频。 这将准备播放视频。                      |
| startAt                           | 在特定位置开始播放视频                                       |
| autoPlay                          | 显示视频后立即播放                                           |
| looping                           | 视频是否应循环播放                                           |
| showControlsOnInitialize          | 初始化小部件时是否显示控件。                                 |
| showControls                      | 是否完全显示控件                                             |
| customControls                    | 定义自定义控件                                               |
| errorBuilder                      | 当视频播放出现错误时，您可以构建自定义                       |
| aspectRatio                       | 视频的宽高比。 重要的是要获得正确的尺寸，将回退到适合的空间内。 |
| cupertinoProgressColors           | 用于iOS控件的颜色。 默认情况下，iOS播放器使用，从原始iOS 11设计中采样的颜色。 |
| materialProgressColors            | 物料进度条要使用的颜色。 默认情况下，材质 播放器使用主题中的颜色。 |
| placeholder                       | 初始化之前，占位符显示在视频下方                             |
| overlay                           | 在视频和控件之间放置的小部件                                 |
| fullScreenByDefault               | 定义按下播放器时播放器是否以全屏启动                         |
| allowedScreenSleep                | 定义播放器是否全屏睡眠                                       |
| allowFullScreen                   | 定义是否应显示全屏控件                                       |
| isLive                            | 定义控件是否应用于实时流视频                                 |
| allowMuting                       | 定义是否应显示静音控件                                       |
| systemOverlaysAfterFullScreen     | 定义退出全屏后可见的系统覆盖                                 |
| deviceOrientationsAfterFullScreen | 退出全屏后定义一组允许的设备方向                             |
| routePageBuilder                  | 为全屏定义自定义RoutePageBuilder                             |