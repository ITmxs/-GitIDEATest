使用：

Flow需要自己实现子widget的位置转换，在很多场景下首先要考虑的是Wrap是否满足需求。Flow主要用于一些需要自定义布局策略或性能要求较高(如动画中)的场景。

Flow优点：

- 性能好；Flow是一个对child尺寸以及位置调整非常高效的控件，Flow用转换矩阵（transformation matrices）在对child进行位置调整的时候进行了优化：在Flow定位过后，如果child的尺寸或者位置发生了变化，在FlowDelegate中的paintChildren()方法中调用context.paintChild 进行重绘，而context.paintChild在重绘时使用了转换矩阵（transformation matrices），并没有实际调整Widget位置。
- 灵活；由于我们需要自己实现FlowDelegate的paintChildren()方法，所以我们需要自己计算每一个widget的位置，因此，可以自定义布局策略。

缺点：

1. 使用复杂.
2. 不能自适应子widget大小，必须通过指定父容器大小或实现TestFlowDelegate的getSize返回固定大小。

源码：

```dart
 Flow({
    Key key,
    @required this.delegate,/// delegate 用于布局childWidget
    List<Widget> children = const <Widget>[],/// childWidget集合
  })
```

```dart
import 'package:flutter/material.dart';

class FlowWidgetPage extends StatefulWidget {
  FlowWidgetPage({Key key}) : super(key: key);

  @override
  _FlowWidgetPageState createState() => _FlowWidgetPageState();
}

double boxSize = 80.0;

class _FlowWidgetPageState extends State<FlowWidgetPage> {
  @override
  Widget build(BuildContext context) {
    return Flow(
      delegate: MyFlowDelegate(),
      children: List.generate(10, (index) {
        return Box(index);
      }),
    );
  }

  /*一个带渐变颜色的正方形*/
  Widget Box(index) => Container(
        width: boxSize,
        height: boxSize,
        alignment: Alignment.center,
        decoration: BoxDecoration(
          gradient: LinearGradient(
              colors: [Colors.orangeAccent, Colors.orange, Colors.deepOrange]),
        ),
        child: Text(
          index.toString(),
          style: TextStyle(
            color: Colors.white,
            fontSize: 20,
            fontWeight: FontWeight.bold,
          ),
        ),
      );
}

class MyFlowDelegate extends FlowDelegate {
  @override
  void paintChildren(FlowPaintingContext context) {
    /*屏幕宽度*/
    var screenW = context.size.width;

    double padding = 5; //间距
    double offsetX = padding; //x坐标
    double offsetY = padding; //y坐标

    for (int i = 0; i < context.childCount; i++) {
      /*如果当前x左边加上子控件宽度小于屏幕宽度  则继续绘制  否则换行*/
      if (offsetX + boxSize < screenW) {
        /*绘制子控件*/
        context.paintChild(i,
            transform: Matrix4.translationValues(offsetX, offsetY, 0));
        /*更改x坐标*/
        offsetX = offsetX + boxSize + padding;
      } else {
        /*将x坐标重置为margin*/
        offsetX = padding;
        /*计算y坐标的值*/
        offsetY = offsetY + boxSize + padding;
        /*绘制子控件*/
        context.paintChild(i,
            transform: Matrix4.translationValues(offsetX, offsetY, 0));
      }
    }
  }

  @override
  bool shouldRepaint(FlowDelegate oldDelegate) {
    return true;
  }
}

```



```dart
import 'package:flutter/material.dart';
import 'package:vector_math/vector_math_64.dart' as Vector;

class FlowPage extends StatefulWidget {
  FlowPage({Key key}) : super(key: key);

  @override
  _FlowPageState createState() => _FlowPageState();
}

class _FlowPageState extends State<FlowPage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text("搜索记录"),
        ),
        body: Flow(
          delegate: TestFlowDelegate(margin: EdgeInsets.all(10.0)),
          children: <Widget>[
            new Container(width: 80.0, height: 80.0, color: Colors.red),
            new Container(width: 80.0, height: 80.0, color: Colors.yellow),
            new Container(width: 80.0, height: 80.0, color: Colors.green),
            new Container(width: 80.0, height: 80.0, color: Colors.blue),
            new Container(width: 80.0, height: 80.0, color: Colors.lightBlue),
            new Container(width: 80.0, height: 80.0, color: Colors.black),
            new Container(width: 80.0, height: 80.0, color: Colors.blueGrey),
            new Container(width: 80.0, height: 80.0, color: Colors.brown),
            new Container(width: 80.0, height: 80.0, color: Colors.black12),
          ],
        ));
  }
}

class TestFlowDelegate extends FlowDelegate {
  EdgeInsets margin = EdgeInsets.zero;

  TestFlowDelegate({this.margin});

  @override
  void paintChildren(FlowPaintingContext context) {
    var x = margin.left;
    var y = margin.top;
    //计算每一个子widget的位置
    for (int i = 0; i < context.childCount; i++) {
      var w = context.getChildSize(i).width + x + margin.right;
      if (w < context.size.width) {
        context.paintChild(i,
            transform: new Matrix4.compose(
                Vector.Vector3(x, y, 0.0),
                Vector.Quaternion(0.0, 0.0, 0.3, 0.1),
                Vector.Vector3(1.0, 1.0, 1.0)));
        x = w + margin.left;
      } else {
        x = margin.left;
        y += context.getChildSize(i).height + margin.top + margin.bottom;
        //绘制子widget(有优化)
        context.paintChild(i,
            transform: Matrix4.translationValues(x, y, 0.0) //位移
            );
        x += context.getChildSize(i).width + margin.left + margin.right;
      }
    }
  }

  getSize(BoxConstraints constraints) {
    //指定Flow的大小
    return Size(double.infinity, double.infinity);
  }

  @override
  bool shouldRepaint(FlowDelegate oldDelegate) {
    return oldDelegate != this;
  }
}

```

