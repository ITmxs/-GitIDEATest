

## TextStyle style

文本样式，样式属性如表：

| **属性**                            | **说明**                                                     |
| ----------------------------------- | ------------------------------------------------------------ |
| Color color                         | 文本颜色。如果指定了foreground，则此值必须为null。           |
| TextDecoration decoration           | 绘制文本装饰:下划线（TextDecoration.underline）上划线（TextDecoration.overline）删除线（TextDecoration.lineThrough）无（TextDecoration.none） |
| Color decorationColor               | 绘制文本装饰的颜色。                                         |
| TextDecorationStyle decorationStyle | 绘制文本装饰的样式:画一条虚线 TextDecorationStyle.dashed画一条虚线 TextDecorationStyle.dotted画两条线 TextDecorationStyle.double画一条实线 TextDecorationStyle.solid画一条正弦线(波浪线) TextDecorationStyle.wavy |
| FontWeight fontWeight               | 绘制文本时使用的字体粗细:FontWeight.bold 常用的字体重量比正常重。即w700FontWeight.normal 默认字体粗细。即w400FontWeight.w100 薄，最薄FontWeight.w200 特轻FontWeight.w300 轻FontWeight.w400 正常/普通/平原FontWeight.w500 较粗FontWeight.w600 半粗体FontWeight.w700 加粗FontWeight.w800 特粗FontWeight.w900 最粗 |
| FontStyle fontStyle                 | 字体变体：FontStyle.italic 使用斜体FontStyle.normal 使用直立 |
| TextBaseline textBaseline           | 对齐文本的水平线:TextBaseline.alphabetic：文本基线是标准的字母基线TextBaseline.ideographic：文字基线是表意字基线；如果字符本身超出了alphabetic 基线，那么ideograhpic基线位置在字符本身的底部。 |
| String fontFamily                   | 使用的字体名称（例如，Roboto）。如果字体是在包中定义的，那么它将以'packages / package_name /'为前缀（例如'packages / cool_fonts / Roboto'） |
| double fontSize                     | 字体大小(pt、sp)，默认为14个逻辑像素(14pt、14sp)             |
| double letterSpacing                | 水平字母之间的空间间隔（逻辑像素为单位）。可以使用负值来让字母更接近。 |
| double wordSpacing                  | 单词之间添加的空间间隔（逻辑像素为单位）。可以使用负值来使单词更接近。 |
| double height                       | 文本行与行的高度，作为字体大小的倍数（取值1~2，如1.2）       |
| Locale locale                       | 此属性很少设置，用于选择区域特定字形的语言环境               |
| Paint background                    | 文本背景色                                                   |
| Paint foreground                    | 文本的前景色，不能与color共同设置（比文本颜色color区别在Paint功能多，后续会讲解） |
| List<Shadow> shadows                | 详解:[**Flutter Decoration背景设定（边框、圆角、阴影、形状、渐变、背景图像等）**](https://blog.csdn.net/chenlove1/article/details/83627831) |

该 [style](https://docs.flutter.io/flutter/widgets/Text/style.html) 参数可选。省略时，文本将使用最接近的[DefaultTextStyle](https://docs.flutter.io/flutter/widgets/DefaultTextStyle-class.html)的样式。如果给定样式的[TextStyle.inherit](https://docs.flutter.io/flutter/painting/TextStyle/inherit.html)属性为true（默认值），则给定样式将与最接近的[DefaultTextStyle](https://docs.flutter.io/flutter/widgets/DefaultTextStyle-class.html)合并。例如，这种合并行为很有用，可以在使用默认字体系列和大小时使文本变为粗体。

## TextAlign textAlign

文本应如何水平对齐enum：

| **值**            | **说明**                                                     |
| ----------------- | ------------------------------------------------------------ |
| TextAlign.center  | 将文本对齐容器的中心。                                       |
| TextAlign.end     | 对齐容器后缘上的文本。                                       |
| TextAlign.justify | 拉伸以结束的文本行以填充容器的宽度。即使用了decorationStyle才起效 |
| TextAlign.left    | 对齐容器左边缘的文本。                                       |
| TextAlign.right   | 对齐容器右边缘的文本。                                       |
| TextAlign.start   | 对齐容器前缘上的文本。                                       |

## TextDirection textDirection

这个属性估计是给外国人习惯使用，

相对TextAlign中的start、end而言有用（当start使用了ltr相当于end使用了rtl，也相当于TextAlign使用了left）

对于从左到右的文本（[TextDirection.ltr](https://docs.flutter.io/flutter/dart-ui/TextDirection-class.html)），文本从左向右流动；

对于从右到左的文本（[TextDirection.rtl](https://docs.flutter.io/flutter/dart-ui/TextDirection-class.html)），文本从右向左流动。

##  Locale locale

此属性很少设置，用于选择区域特定字形的语言环境

##  bool softWrap

某一行中文本过长，是否需要换行。默认为true，如果为false，则文本中的字形将被定位为好像存在无限的水平空间。

##  TextOverflow overflow

如何处理视觉溢出:

| TextOverflow.clip     | 剪切溢出的文本以修复其容器。 |
| --------------------- | ---------------------------- |
| TextOverflow.ellipsis | 使用省略号表示文本已溢出。   |
| TextOverflow.fade     | 将溢出的文本淡化为透明。     |

##  double textScaleFactor

每个逻辑像素的字体像素数

例如，如果文本比例因子为1.5，则文本将比指定的字体大小大50％。

作为textScaleFactor赋予构造函数的值。如果为null，将使用从环境[MediaQuery](https://docs.flutter.io/flutter/widgets/MediaQueryData/textScaleFactor.html)获取的[MediaQueryData.textScaleFactor](https://docs.flutter.io/flutter/widgets/MediaQuery-class.html) 即手机的像素密度（1.0、1.5、2.0、3.0）

## int maxLines

文本要跨越的可选最大行数,

为1，则文本不会换行。否则，文本将被包裹在框的边缘。

##  String semanticsLabel

图像的语义描述，用于向Andoid上的TalkBack和iOS上的VoiceOver提供图像描述

talkback是一款由谷歌官方开发的系统软件,它的定位是帮助盲人或者视力有障碍的用户提供语言辅助

Voiceover功能是APPLE公司在2009年4月新推出的一种语音辅助程序





```
   Text(
        "Text组件的使用",
        style: TextStyle(
            // 文字颜色
            color: Color(0xfff0000),
            // none 不显示装饰线条，underline 字体下方，overline 字体上方，lineThrough穿过文字
            decoration: TextDecoration.none,
            // solid 直线，double 双下划线，dotted 虚线，dashed 点下划线，wavy 波浪线
            decorationStyle: TextDecorationStyle.solid,
            // 装饰线的颜色
            decorationColor: Colors.red,
            // 文字大小
            fontSize: 15.0,
            // normal 正常，italic 斜体
            fontStyle: FontStyle.normal,
            // 字体的粗细
            fontWeight: FontWeight.bold,
            // 文字间的宽度
            letterSpacing: 1.0,
            // 文本行与行的高度，作为字体大小的倍数（取值1~2，如1.2）
            height: 1,
            //对齐文本的水平线:
            //TextBaseline.alphabetic：文本基线是标准的字母基线
            //TextBaseline.ideographic：文字基线是表意字基线；
            //如果字符本身超出了alphabetic 基线，那么ideograhpic基线位置在字符本身的底部。
            textBaseline: TextBaseline.alphabetic),
        // 段落的间距样式
        strutStyle: StrutStyle(
          fontFamily: 'serif',
          fontFamilyFallback: ['monospace', 'serif'],
          fontSize: 20,
          height: 2,
          leading: 2.0,
          fontWeight: FontWeight.w300,
          fontStyle: FontStyle.normal,
          forceStrutHeight: true,
          debugLabel: 'text demo',
        ),
        // 文字对齐方式
        textAlign: TextAlign.center,
        // 文字排列方向 ltr 左到右，rtl右到左
        textDirection: TextDirection.ltr,
        // 用于选择区域特定字形的语言环境
        locale: Locale('zh_CN'),
        // 软包裹 ，文字是否应该在软断行出断行
        softWrap: false,
        // 如何处理视觉溢出:clip 剪切溢出的文本以修复其容器。ellipsis 使用省略号表示文本已溢出。fade 将溢出的文本淡化为透明。
        overflow: TextOverflow.clip,
        // 文字的缩放比例
        textScaleFactor: 1.0,
        // 文本要跨越的可选最大行数,
        maxLines: 2,
        // 图像的语义描述，用于向Andoid上的TalkBack和iOS上的VoiceOver提供图像描述
        semanticsLabel: 'text demo',
        textWidthBasis: TextWidthBasis.longestLine,
      )
```

