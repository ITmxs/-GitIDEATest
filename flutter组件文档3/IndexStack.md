

见名知意：

IndexStack和Stack是类似的，但是他们还是有区别的

1. IndexedStack只能够显示widget列表中单个子组件，默认0位
2. IndexedStack的大小在没有父约束的情况下是由内部尺寸最大且不为Positioned子组件决定的；那内部要是没有呢，那就完全依据IndexedStack父控件约束来控制自己的尺寸。
3. sizing属性对IndexedStack其实没有什么影响，因为IndexedStack重写了Stack的createRenderObject与updateRenderObject方法，sizing就是就是Stack的fit，再重写方法里面fit没有被赋值，之作用在了debugFillProperties方法上
4. IndexedStack在处理溢出组件上就是Clip.hardEdge，不可设置其他的

代码示例：

```dart
  _myStackChildren() {
    return [
      Container(
        width: 50,
        height: 50,
        color: Colors.red,
      ),
      Positioned(
        bottom: 20,
        child: Container(
          width: 80,
          height: 80,
          color: Colors.green,
        ),
      ),
      Container(
        width: 60,
        height: 60,
        color: Colors.cyan,
      ),
      Positioned(
        left: 30,
        top: 80,
        width: 2000,
        height: 20,
        child: Container(
          color: Colors.black,
        ),
      ),
    ];
  }
      body: Container(
        color: Colors.blue,
        child: IndexedStack(
          alignment: AlignmentDirectional.center,
          sizing: StackFit.expand,
          index: 0,
          children: _myStackChildren(),
        ),
      ),
```

这里是显示的第0个子组件，大小是50/50；IndexedStack大小是60/60，因为他的大小是由第2个60/60的子组件决定的，Positioned的子组件大小是不会影响到IndexedStack大小的；当然这个情况也是需要在遵循IndexedStack父约束的情况下，IndexedStack首先还是响应父约束的，例如设置父Container宽高为300，那么IndexedStack就是300/300