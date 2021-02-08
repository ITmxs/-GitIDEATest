

Column

```dart
ListView(children: <Widget>[
      Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          Text("我是Colum的子控件"),
          Text("CrossAxisAlignment.start"),
        ],
      ),
      Column(
        crossAxisAlignment: CrossAxisAlignment.center,
        children: <Widget>[
          Text("我是Colum的子控件"),
          Text("CrossAxisAlignment.center"),
        ],
      ),
      Column(
        crossAxisAlignment: CrossAxisAlignment.end,
        children: <Widget>[
          Text("我是Colum的子控件"),
          Text("CrossAxisAlignment.end"),
        ],
      ),
    ],)
```

Row

```dart
 ListView(
      children: <Widget>[
        Row(
          mainAxisAlignment: MainAxisAlignment.start,
          children: <Widget>[
            Text("我是Row的子控件  "),
            Text("MainAxisAlignment.start")
          ],
        ),
        Row(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text("我是Row的子控件  "),
            Text("MainAxisAlignment.center")
          ],
        ),
        Row(
          mainAxisAlignment: MainAxisAlignment.end,
          children: <Widget>[
            Text("我是Row的子控件  "),
            Text("MainAxisAlignment.end")
          ],
        ),
        Row(
          crossAxisAlignment: CrossAxisAlignment.start,
          verticalDirection: VerticalDirection.up,
          children: <Widget>[
            Text(" Hello World ", style: TextStyle(fontSize: 30.0),),
            Text(" I am Jack "),
          ],
        )
      ],
    ),
```



参考https://www.jianshu.com/p/8687d52007aa

### 线性布局Row和Column

线性布局其实是指沿水平或垂直方向排布子Widget，Flutter中通过Row来实现水平方向的子Widegt布局，通过Column来实现垂直方向的子Widget布局。他们都继承Flex，所以它们有很多相似的属性。

![Row](https://luckly007.oss-cn-beijing.aliyuncs.com/images/2455429-7c79ba2bb16d5c1e.png)

![Column](https://luckly007.oss-cn-beijing.aliyuncs.com/images/2455429-e2529946bc8092bf.png)

在前端的Flex布局中，默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。与Flutter中MainAxisAlignment和CrossAxisAlignment类似，分别代表主轴对齐和纵轴对齐。

#### 源码属性解读

```dart
  Row({
    .....
    MainAxisAlignment mainAxisAlignment = MainAxisAlignment.start,
    MainAxisSize mainAxisSize = MainAxisSize.max,
    CrossAxisAlignment crossAxisAlignment = CrossAxisAlignment.center,
    TextDirection textDirection,
    VerticalDirection verticalDirection = VerticalDirection.down,
    TextBaseline textBaseline,
    List<Widget> children = const <Widget>[],
  })
 
  Column({
    .....
    MainAxisAlignment mainAxisAlignment = MainAxisAlignment.start,
    MainAxisSize mainAxisSize = MainAxisSize.max,
    CrossAxisAlignment crossAxisAlignment = CrossAxisAlignment.center,
    TextDirection textDirection,
    VerticalDirection verticalDirection = VerticalDirection.down,
    TextBaseline textBaseline,
    List<Widget> children = const <Widget>[],
  }) 

```

- textDirection：表示水平方向子widget的布局顺序(是从左往右还是从右往左)，默认为系统当前Locale环境的文本方向(如中文、英语都是从左往右，而阿拉伯语是从右往左)。
- 主轴方向: Row即为水平方向，Column为垂直方向
- mainAxisAlignment 主轴方向，对child起作用
  - center：将children放置在主轴的中心
  - start：将children放置在主轴的起点
  - end：将children放置在主轴的末尾
  - spaceAround：将主轴方向上的空白区域均分，使children之间的空白区域相等，但是首尾child的靠边间距为空白区域为1/2
  - spaceBetween：将主轴方向上的空白区域均分，使children之间的空白区域相等，首尾child靠边没有间隙
  - spaceEvenly：将主轴方向上的空白区域均分，使得children之间的空白区域相等，包括首尾child
- mainAxisSize max表示尽可能占多的控件，min会导致控件聚拢在一起
- crossAxisAlignment 交叉轴方向，对child起作用
  - baseline：使children baseline对齐
  - center：children在交叉轴上居中展示
  - end：children在交叉轴上末尾展示
  - start：children在交叉轴上起点处展示
  - stretch：让children填满交叉轴方向
- verticalDirection ，child的放置顺序
  - VerticalDirection.down，在Row中就是从左边到右边，Column代表从顶部到底部
  - VerticalDirection.up，相反

### 实际使用



- 如果Row里面嵌套Row，或者Column里面再嵌套Column，那么只有对最外面的Row或Column会占用尽可能大的空间，里面Row或Column所占用的空间为实际大小，如果要让里面的Colum或Row占满外部Colum或Row，可以使用Expanded widget
- 如果使用Column发现超范围，可用SingleChildScrollView包裹，scrollDirection属性设置滑动方向
- 使用Column嵌套ListView/GridView的时候，会报异常信息【Viewports expand in the scrolling direction to fill their container...】，这种情况flutter已给出解决办法，将ListView/GridView的 shrinkWrap属性设为true
- 有的时候修改Row/Column的verticalDirection会得到很好的效果，比如需要页面在底部需要几个按键，也可以用Stack来布局，但是相对麻烦，而且有时还需要知道控件的大小，没有verticalDirection方便