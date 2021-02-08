



参数详解

| 属性                 | 详解                                                |
| -------------------- | --------------------------------------------------- |
| child                | 子组件                                              |
| onTap                | 点击监听（）                                        |
| onDoubleTap          | 双击监听                                            |
| onLongPress          | 长按监听                                            |
| onTapDown            | 点击监听                                            |
| onTapCancel          | 取消点击监听                                        |
| onHighlightChanged   | 当材料的这一部分突出显示或停止突出显示时调用        |
| onHover              | 当指针进入或退出墨水响应区域时调用                  |
| focusColor           | 获取焦点颜色                                        |
| hoverColor           | 指针悬停时颜色                                      |
| highlightColor       | 长按颜色                                            |
| splashColor          | 水波纹颜色                                          |
| splashFactory        | 自定义水波纹                                        |
| radius               | 水波纹半径                                          |
| borderRadius         | 飞溅半径                                            |
| customBorder         | 覆盖borderRadius的自定义剪辑边框                    |
| enableFeedback       | 检测到的手势是否应该提供声音和/或触觉反馈，默认true |
| excludeFromSemantics | 是否将此小部件引入的手势从语义树中排除。默认false   |

代码示例

常规使用方式：

```
Material(
  color: Colors.red,
  child: new InkWell(
    onTap: (){print('--InkWell--');},
    child: Container(
      alignment: Alignment.center,
      child: Text('我是InkWell'),
    ),
  )
)
```

在flutter 开发中用InkWell或者GestureDetector将某个组件包起来，可添加点击事件。

GestureDetector 使用点击无水波纹出现，InkWell可以实现水波纹效果。
正常情况下使用 ：

```java
 InkWell(
 		//单击事件响应
      onTap: () {
      },
      child: Container(
        alignment: Alignment(0, 0),
        height: 28,
        width: 120,
        child: Text("InkWell单击事件"),
      ),
    ),
```

如果在InkWell的上下都出现的颜色的设置，如上中的Container中如果加入了color:Colors.white,或者是Container中的其他widget设置了coloro属性，这时候InkWell的水波纹效果会无效。

#### 1 widget 设置水波纹点击效果 并设置widget背景

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190707220233666.gif)

```java
new Center(
         child: new Material(
           // 设置背景颜色 默认矩形
           color: Colors.purple,
           child: new InkWell(
             //点击事件回调
             onTap: () {},
             //不要在这里设置背景色，for则会遮挡水波纹效果,如果设置的话尽量设置Material下面的color来实现背景色
             child: new Container(
               width: 300.0,
               height: 50.0,
               //设置child 居中
               alignment: Alignment(0, 0),
               child: Text("登录",style: TextStyle(color: Colors.white,fontSize: 16.0),),
             ),
           ),
         ),
       ),
123456789101112131415161718
```

或者 可以使用 Ink来设置，与Material设置color 的区别是，Ink可设置背景的形状样式。

```java
new Center(
     child: new Material(
       //INK可以实现装饰容器，实现矩形  设置背景色
       child: new Ink(
         //设置背景  默认矩形
         color: Colors.purple,
         child: new InkWell(
           //点击事件回调
           onTap: () {},
           child: new Container(
             width: 300.0,
             height: 50.0,
             //设置child 居中
             alignment: Alignment(0, 0),
             child: Text("登录",style: TextStyle(color: Colors.white,fontSize: 16.0),),
           ),
         ),
       ),
     ),
   ),

```

#### 2 圆角widget 设置水波纹点击效果

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019070721574435.gif)

```java
        new Center(
          child: new Material(
            //INK可以实现装饰容器
            child: new Ink(
              //用ink圆角矩形
            // color: Colors.red,
              decoration: new BoxDecoration(
               //不能同时”使用Ink的变量color属性以及decoration属性，两个只能存在一个
                color: Colors.purple,
                //设置圆角
                borderRadius: new BorderRadius.all(new Radius.circular(25.0)),
              ),
              child: new InkWell(
                //圆角设置,给水波纹也设置同样的圆角
                //如果这里不设置就会出现矩形的水波纹效果
                borderRadius: new BorderRadius.circular(25.0), 
                //设置点击事件回调
                onTap: () {

                },
                child: new Container(
                  width: 300.0,
                  height: 50.0,
                  //设置child 居中
                  alignment: Alignment(0, 0),
                  child: Text("登录",style: TextStyle(color: Colors.white,fontSize: 16.0),),
                ),
              ),
            ),
          ),
        ),

```

如果 InkWell 与Ink 不同时设置相同的圆角，例如 lnk 设置的圆角为20，而Ink没有设置,就会出现 矩形的水波纹点击效果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190707215243206.gif)

#### 3 圆角widget 设置自定义水波纹颜色点击效果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190707221527967.gif)
InkResponse组件也可实现InkWell组件相同的功能，因为InkWell继承于InkResponse，相比之下，InkResponse可以选择控制水波纹以及高亮底色的形状和剪裁效果。基本使用代码如下：

```java
new Center(
  child: new Material(
    child: new Ink(
      //设置背景
      decoration: new BoxDecoration(
        color: Colors.purple,
        borderRadius: new BorderRadius.all(new Radius.circular(25.0)),
      ),
      child: new InkResponse(
        borderRadius: new BorderRadius.all(new Radius.circular(25.0)),
        //点击或者toch控件高亮时显示的控件在控件上层,水波纹下层
        //highlightColor: Colors.yellowAccent,
        //点击或者toch控件高亮的shape形状
        highlightShape: BoxShape.rectangle,
        //.InkResponse内部的radius这个需要注意的是，我们需要半径大于控件的宽，如果radius过小，显示的水波纹就是一个很小的圆，
        //水波纹的半径
        radius: 300.0,
        //水波纹的颜色
        splashColor: Colors.black,
        //true表示要剪裁水波纹响应的界面   false不剪裁  如果控件是圆角不剪裁的话水波纹是矩形
        containedInkWell: true,
        //点击事件
        onTap: () {
          print("click");
        },
        child: new Container(
          //不能在InkResponse的child容器内部设置装饰器颜色，否则会遮盖住水波纹颜色的，containedInkWell设置为false就能看到是否是遮盖了。
          width: 300.0,
          height: 50.0,
          //设置child 居中
          alignment: Alignment(0, 0),
          child: Text("登录",style: TextStyle(color: Colors.white,fontSize: 16.0),),
        ),

      ),
    ),
  ),
),

```

#### 4 圆角widget 设置高亮颜色点击效果

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190707221656935.gif)

```dart
new Center(
  child: new Material(
    child: new Ink(
      //设置背景
      decoration: new BoxDecoration(
        color: Colors.purple,
        borderRadius: new BorderRadius.all(new Radius.circular(30.0)),
      ),
      child: new InkResponse(
        borderRadius: new BorderRadius.all(new Radius.circular(30.0)),
        //点击或者toch控件高亮时显示的控件在控件上层,水波纹下层
        highlightColor: Colors.purple[800],
        //点击或者toch控件高亮的shape形状
        highlightShape: BoxShape.rectangle,
        //.InkResponse内部的radius这个需要注意的是，我们需要半径大于控件的宽，如果radius过小，显示的水波纹就是一个很小的圆，
        //水波纹的半径
        radius: 0.0,
        //水波纹的颜色 设置了highlightColor属性后 splashColor将不起效果
        splashColor: Colors.red,
        //true表示要剪裁水波纹响应的界面   false不剪裁  如果控件是圆角不剪裁的话水波纹是矩形
        containedInkWell: true,

        onTap: () {
          print(
              'click');
        },
        child: new Container(
          //不能在InkResponse的child容器内部设置装饰器颜色，否则会遮盖住水波纹颜色的，containedInkWell设置为false就能看到是否是遮盖了。
          width: 300.0,
          height: 50.0,
          //设置child 居中
          alignment: Alignment(0, 0),
          child: Text("登录",style: TextStyle(color: Colors.white,fontSize: 16.0),),
        ),
      ),
    ),
  ),
),
```

