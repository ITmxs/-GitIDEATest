实现效果，可以选择并最后提交数据，这里讲的是一种原理，一种思路。

实现效果：

<img src="https://luckly007.oss-cn-beijing.aliyuncs.com/images/20210208135438.png" alt="image-20210208135438241" style="zoom:25%;" />

### 入口：

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import '../GridViewBean.dart';
import '../utils/log_utils.dart';

import 'GridViewItemWidget.dart';

main() {
  runApp(MaterialApp(
    home: GridViewDemoPage(),
  ));
}

///代码清单
class GridViewDemoPage extends StatefulWidget {
  @override
  _GridViewDemoPageState createState() => _GridViewDemoPageState();
}

class _GridViewDemoPageState extends State<GridViewDemoPage> {
  //数据源
  List<GridBean> _list = [];

  @override
  void initState() {
    super.initState();

    for (int i = 0; i < 10; i++) {
      GridBean gridBean = new GridBean(title: "测试$i");
      _list.add(gridBean);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("GridView "),
        actions: [
          TextButton(
            onPressed: () {
              //获取完成的数据
              //获取选中的数据
              List<String> selectList = [];

              //筛选数据
              _list.forEach((GridBean element) {
                //选中标识
                if(element.isSelect){
                  selectList.add(element.title);
                }
              });

              LogUtils.e("${selectList.toString()}");

            },
            child: Text(
              "提交数据",
              style: TextStyle(
                color: Colors.white,
                fontSize: 16,
              ),
            ),
          )
        ],
      ),
      backgroundColor: Colors.white,

      ///填充布局
      body: GridView.builder(
        //子Item的个数
        itemCount: _list.length,
        //子布局构建器
        itemBuilder: (BuildContext context, int index) {
          //取出每个数据
          GridBean bean = _list[index];

          return GridViewItemWidget(bean:bean);
        },
        //子布局排列方式
        //按照固定列数来排列
        gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
          //主方向的Item间隔 竖直方向
          mainAxisSpacing: 12,
          //次方向的Item间隔
          crossAxisSpacing: 12,
          //子Item 的宽高比
          childAspectRatio: 2.1,
          //每行4列
          crossAxisCount: 4,
        ),
      ),
    );
  }
}

```



### 数据源GridBean.dart

```dart


class GridBean {
  String title;
  bool isSelect;

  GridBean({this.title, this.isSelect = false});
}

```

### GridViewItemWidget.dart

```dart
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

import 'GridViewBean.dart';

/// 代码清单
///GridView使用的子Item
class GridBean extends StatefulWidget {
  final GridBean bean;

  const GridViewItemWidget({Key key, this.bean}) : super(key: key);

  @override
  _GridViewItemWidgetState createState() => _GridViewItemWidgetState();
}

class _GridViewItemWidgetState extends State<GridViewItemWidget> {
  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.white,
      child: Stack(
        alignment: Alignment.center,
        children: [
          Positioned(
            top: 6,
            bottom: 6,
            left: 10,
            right: 10,
            //点击事件
            child: GestureDetector(
              onTap: () {
                //相反取值
                widget.bean.isSelect = !widget.bean.isSelect;
                setState(() {});
              },
              child: buildContainer(),
            ),
          )
        ],
      ),
    );
  }

  Container buildContainer() {
    Color normalColor = Colors.grey[200];

    //如果是选中
    if (widget.bean.isSelect) {
      normalColor = Colors.redAccent;
    }

    return Container(
      //子Widget 居中
      alignment: Alignment.center,
      decoration: BoxDecoration(
        color: normalColor,
        borderRadius: BorderRadius.all(
          Radius.circular(22),
        ),
      ),
      child: buildText(),
    );
  }

  buildText() {
    //字体颜色
    Color normalColor = Colors.black;

    //如果是选中
    if (widget.bean.isSelect) {
      normalColor = Colors.white;
    }

    return Text(
      "${widget.bean.title}",
      style: TextStyle(color: normalColor),
    );
  }
}

```

