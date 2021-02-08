

```
   //设置滑动方向 Axis.horizontal 水平  默认 Axis.vertical 垂直
        scrollDirection: Axis.vertical,
        //内间距
        padding: EdgeInsets.all(10.0),
        //是否倒序显示 默认正序 false  倒序true
        reverse: false,
        //false，如果内容不足，则用户无法滚动 而如果[primary]为true，它们总是可以尝试滚动。
        primary: true,
        //确定每一个item的高度 会让item加载更加高效
        itemExtent: 50.0,
        //内容适配
        shrinkWrap: true,
        //item 数量
        itemCount: list.length,
        //滑动类型设置
        physics: new ClampingScrollPhysics(),
         //cacheExtent  设置预加载的区域 
         cacheExtent: 30.0, 
        //滑动监听
//        controller ,

```

