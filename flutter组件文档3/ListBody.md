ListBody常常配合Row实现宽度不同的水平排列，或者配合Column实现高度不通的垂直排列，还可以根据自身的reverse属性实现列表内容的正序或倒叙排列。也可以和ListView配合使用：

```dart
@override
Widget build(BuildContext context) {
  return Column(
    children: [
      ListBody(
        //对齐方式，注意，父元素是Column，这里只能是vertical垂直排列，如果像用Axis.horizontal水平排列，父元素必须是Row
        mainAxis: Axis.vertical,
        //内容是否反向
        reverse: true,
        children: [
          Container(
            color: Colors.red,
            width: 50.0,
            height: 50.0,
            child: Text(
              'A',
              style: TextStyle(
                color: Colors.white,
              ),
            ),
          ),
          Container(
            color: Colors.orange,
            width: 100.0,
            height: 100.0,
            child: Text(
              'B',
              style: TextStyle(
                color: Colors.white,
              ),
            ),
          ),
          Container(
            color: Colors.blue,
            width: 150.0,
            height: 150.0,
            child: Text(
              'C',
              style: TextStyle(
                color: Colors.white,
              ),
            ),
          ),
        ],
      )
    ],
  );
}
```

