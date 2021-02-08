  示



```
import 'package:flutter/material.dart';

void main(List<String> args) {
  runApp(MyApp());
}

//自定义组件
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text("Flutter Demo")),
        body: HomeContent(),
      ),
    );
  }
}

//自定义组件2、
class HomeContent extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return GridView.count(
      crossAxisCount: 3,
      children: <Widget>[ //一行的widget数量
        Text("这是一个文本"),
        Text("这是一个文本"),
        Text("这是一个文本"),
        Text("这是一个文本"),
        Text("这是一个文本"),
        Text("这是一个文本"),
        Text("这是一个文本"),
        Text("这是一个文本"),
      ],
    );
  }
}

```

