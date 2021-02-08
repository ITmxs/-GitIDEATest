1.



```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      appBar: AppBar(title: Text('上班') //指定应用程序在任务栏上显示的标题
          ),
      body: Text("你好，昝茜",
          style: TextStyle(fontSize: 36)), //参数1：将要渲染的文本内容  参数2：文本内容的样式
    ));
  }
}

```

2.center

```
Center(//可将内部的子元素，呈现到屏幕正中央进行显示
              child: Text("你好，flutter",
                  style:
                      TextStyle(fontSize: 36)), //参数1：将要渲染的文本内容  参数2：文本内容的样式),
            )
```

3.

# Scaffold

该组件是页面结构的脚手架，包含了页面的基本组成单元，例如：

- appBar【头部导航条区域】

- body【页面主题内容区域】

  <img src="https://mxszs.oss-cn-beijing.aliyuncs.com/img/components_toolbars.png" alt="img" style="zoom: 25%;" />

- drawer【侧边栏抽屉区域】

  <img src="https://mxszs.oss-cn-beijing.aliyuncs.com/img/patterns_navigation_drawer.png" alt="img" style="zoom: 25%;" />

- bottomNavigationBar【底部tabBar区域】

  <img src="https://mxszs.oss-cn-beijing.aliyuncs.com/img/components_bottom_navigation.png" alt="img" style="zoom:25%;" />

- floatingActionButton【右下角浮动按钮区域】

  <img src="https://mxszs.oss-cn-beijing.aliyuncs.com/img/components_buttons_fab.png" alt="img" style="zoom:25%;" />

```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
    appBar: AppBar(
      title: Text('页面标题'),
    ),
    body: Center(
      child: Text('主体内容'),
    ),
    floatingActionButton: FloatingActionButton(
      onPressed: () {},
      child: Icon(Icons.add),
    ),
    drawer: Drawer(),
  ),
  // 主题颜色
  theme: ThemeData(primarySwatch: Colors.red),
);
  }
}

```







```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          title: Text('页面标题'),
        ),
        body: Row(
          mainAxisAlignment:MainAxisAlignment.end,
          children: <Widget>[
            Expanded(
              
              child: Text('主体内容3'),
              flex: 10,
            ),
            Expanded(
              child: Text('主体内容2'),
              flex: 10,
            ),
            Expanded(//主要用来控制 flex 布局的占位宽度。需要用在 Row 或 Column 子组件内部。


              child: Text('主体内容1'),
              flex: 10,
            )
          ],
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {},
          child: Icon(Icons.add),
        ),
        drawer: Drawer(),
      ),
      // 主题颜色
      theme: ThemeData(primarySwatch: Colors.red),
    );
  }
}

```

