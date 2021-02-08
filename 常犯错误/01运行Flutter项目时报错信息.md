

今日下载了flutter，并完成了环境配置，用flutter医生检查时：

```
Microsoft Windows [版本 10.0.17763.1339]
(c) 2018 Microsoft Corporation。保留所有权利。

C:\Users\Luckly>flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 1.22.3, on Microsoft Windows [Version 10.0.17763.1339], locale zh-CN)
[√] Android toolchain - develop for Android devices (Android SDK version 30.0.2)
[√] Android Studio (version 3.6)
[!] Connected device
    ! Device a4b2501f is not authorized.
      You might need to check your device for an authorization dialog.

! Doctor found issues in 1 category.
```





C:\Users\Luckly>flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 1.22.3, on Microsoft Windows [Version 10.0.17763.1339], locale zh-CN)

[√] Android toolchain - develop for Android devices (Android SDK version 30.0.2)
[√] Android Studio (version 3.6)
[√] Connected device (1 available)

• No issues found!

```
C:\Users\Luckly>netstat -aon|findstr 5037
  TCP    127.0.0.1:5037         0.0.0.0:0              LISTENING       8288
  TCP    127.0.0.1:5037         127.0.0.1:56370        TIME_WAIT       0
  TCP    127.0.0.1:5037         127.0.0.1:56371        TIME_WAIT       0

  TCP    127.0.0.1:56466        127.0.0.1:5037         ESTABLISHED     6448
  TCP    127.0.0.1:56472        127.0.0.1:5037         ESTABLISHED     6448
```

检查8288



```
C:\Users\Luckly>tasklist /fi "PID eq 8288"

映像名称                       PID 会话名              会话#       内存使用
========================= ======== ================ =========== ============
nox_adb.exe                   8288 Console                    1      8,856 K
```

停止8288

```
C:\Users\Luckly>taskkill /pid 8288 /f
成功: 已终止 PID 为 8288 的进程。
```

方式二
自己配置 adb server 端口，使用一个生僻的值。
很简单，只要在系统环境变量中定义 ANDROID_ADB_SERVER_PORT 的值即可。
最好选择一个5位数的端口号（10000 ~ 65535），不易重复。
win下只要在环境变量中增加一个ANDROID_ADB_SERVER_PORT ，值填你自己定义的端口。
linux下只要 export $ANDROID_ADB_SERVER_PORT = 自定义端口，即可