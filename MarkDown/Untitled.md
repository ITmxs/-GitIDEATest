2020

复制黏贴，转发，删除

https://blog.csdn.net/guanzhong12345/article/details/106265942/







https://www.jianshu.com/p/35ec105aec41





https://www.jianshu.com/p/d301e94761c3







http://www.ptbird.cn/flutter-listview-snackbar-delete.html





qq登录



https://github.com/rxreader/tencent_kit





微信登录



https://github.com/rxreader/wechat_kit





Starting a Gradle Daemon, 1 busy and 1 incompatible Daemons could not be reused, use --status for details



















```
 ClipRRect(  //11111
                  borderRadius: BorderRadius.circular(8),
                  child: MessageItemFactory(
                    message: message,
                    voiceStep: _voiceStep,
                    voiceLen: _voiceCurrentLen.toInt(),
                  ),
                )
```









```
Container(
            padding: dir == Direction.SEND
                ? EdgeInsets.fromLTRB(120.w, 6.w, 15.w, 10.w)
                : EdgeInsets.fromLTRB(15.w, 6.w, 120.w, 10.w),
            alignment: message.direction == Direction.SEND
                ? Alignment.centerRight
                : Alignment.centerLeft,
            child: WPopupMenu(
              onValueChanged: (int value) {
                Scaffold.of(context).showSnackBar(SnackBar(
                  content: Text(actions[value]),
                  duration: Duration(milliseconds: 500),
                ));
              },
              pressType: PressType.singleClick,
              actions: actions,
              child: UnconstrainedBox(
                child: Container(
                    height: 40,
                    color: Colors.cyan,
                    alignment: Alignment.center,
                    child: ClipRRect(
                      //11111
                      borderRadius: BorderRadius.circular(8),
                      child: MessageItemFactory(
                        message: message,
                        voiceStep: _voiceStep,
                        voiceLen: _voiceCurrentLen.toInt(),
                      ),
                    )),
              ),
            ),
          ),
```



: The following assertion was thrown building Builder(dirty, dependencies: [MediaQuery]):
I/flutter (22496): Looking up a deactivated widget's ancestor is unsafe.
I/flutter (22496): At this point the state of the widget's element tree is no longer stable.
I/flutter (22496): To safely refer to a widget's ancestor in its dispose() method, save a reference to the ancestor by
I/flutter (22496): calling dependOnInheritedWidgetOfExactType() in the widget's didChangeDependencies() method.
I/flutter (22496):



气泡：

http://www.flutterj.com/?post=159







smart_bubble



```
SmartBubble(
    title: Text("192.168.31.1"),
    arrowDirection: ArrowDirection.left,
    child: MessageItemFactory(
                    message: message,
                    voiceStep: _voiceStep,
                    voiceLen: _voiceCurrentLen.toInt(),
                  ),
),
```

