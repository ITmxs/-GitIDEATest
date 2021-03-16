Flutter的ListView或Column或Row嵌套ListView，往往会报下面的错误：

RenderBox was not laid out: RenderFlex.....

这是因为ListView或Column或Row嵌套ListView，会有问题，解决办法如下：

## 一、ListView嵌套ListView问题：

```dart
ListView(
    children: <Widget>[
        ListView(
            shrinkWrap: true, //为true可以解决子控件必须设置高度的问题
            physics:NeverScrollableScrollPhysics(),//禁用滑动事件
        ),
    ],
)
```

## 如果需要两个listview同时滑动，则需要向他们传同一个scrollcontroller

```dart
ScrollController _scrollController;
  ListView(
    controller:_scrollController,
    children: <Widget>[
        ListView(
            controller:_scrollController,
        ),
    ],
)
```

## 二、Column嵌套ListView问题，给listView套个有高度的Container或SizedBox：

```dart
Column(
    children: <Widget>[
        Container(
            height: ScreenUtil().setHeight(800),
            child: ListView.builder(
                scrollDirection: Axis.vertical,
                itemBuilder:(context,index){
                    return _getListItem(index);
                },
                itemCount: list.length,
            ),
        ),
    ],
)
```

上面的问题也能用expanede解决，更加灵活，

以上两个问题，都是因为嵌套着和被嵌套着都存在滚动问题，滚动冲突了。

# Flutter Column等容器嵌套ListView报错

在用flutter做个效果时，需要用column嵌套ListView，这时会报错：

*I/flutter ( 4625): EXCEPTION CAUGHT BY RENDERING LIBRARY*
*I/flutter ( 4625): The following assertion was thrown during performResize():*
*I/flutter ( 4625): Vertical viewport was given unbounded height.*
*I/flutter ( 4625): Viewports expand in the scrolling direction to fill their container.In this case, a vertical*
*I/flutter ( 4625): viewport was given an unlimited amount of vertical space in which to expand. This situation*
*I/flutter ( 4625): typically happens when a scrollable widget is nested inside another scrollable widget.*
*I/flutter ( 4625): If this widget is always nested in a scrollable widget there is no need to use a viewport because*
*I/flutter ( 4625): there will always be enough vertical space for the children. In this case, consider using a Column*
*I/flutter ( 4625): instead. Otherwise, consider using the “shrinkWrap” property (or a ShrinkWrappingViewport) to size*
*I/flutter ( 4625): the height of the viewport to the sum of the heights of its children.*

这时，在listview外面嵌套一个expanded，或者一个container就可以了，尺寸计算的问题，expande就是listview有多大就有多大,container就是container多大listview就有多大，可以滚动，也不会报错了：

```
new Expanded(
    child: new ListView(
        .....
    )
);
```

这告诉ListView尽可能地获取宽度和高度。