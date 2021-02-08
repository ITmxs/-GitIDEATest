 Another exception was thrown: RenderBox was not laid out: RenderViewport#90a83 NEEDS-LAYOUT NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE



#### 使用了ListView.builder 布局空白,log提示RenderBox was not laid out: RenderViewport#680c1 NEEDS-LAYOUT NEEDS-PAINT的问题.

在进行Flutter布局绘制时,不可避免的使用ListView.builder().
但是这个很大概率在使用上会嵌套在Row或者Column组件内.
就会出现预期的页面一片空白,Log日志会提示:

```
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: RenderViewport#90a83 NEEDS-LAYOUT NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: RenderViewport#90a83 NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: RenderIgnorePointer#09ea7 relayoutBoundary=up13 NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: RenderSemanticsAnnotations#7d7af relayoutBoundary=up12 NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: RenderPointerListener#36d79 relayoutBoundary=up11 NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: RenderSemanticsGestureHandler#f530d relayoutBoundary=up10 NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: RenderPointerListener#d9553 relayoutBoundary=up9 NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: _RenderScrollSemantics#73825 relayoutBoundary=up8 NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: RenderRepaintBoundary#1ad2c relayoutBoundary=up7 NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: RenderCustomPaint#15238 relayoutBoundary=up6 NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: RenderRepaintBoundary#50f3b relayoutBoundary=up5 NEEDS-PAINT NEEDS-COMPOSITING-BITS-UPDATE
I/flutter (22398): Another exception was thrown: RenderBox was not laid out: RenderRepaintBoundary#50f3b relayoutBoundary=up5 NEEDS-PAINT

Performing hot reload...                                               -I/flutter (22398): ══╡ EXCEPTION CAUGHT BY RENDERING LIBRARY ╞═════════════════════════════════════════════════════════
```

具体原因就是:

### 在Flutter 的Column或者Row内使用ListView.builder()需要对改ListView的大小进行指定.

### 具体的解决办法就是 在该ListView.builder()外嵌套一个SizeBox或者Container即可…

比如:

```
 @override
  Widget build(BuildContext context) {
    return ListView.builder(
        itemCount: 10,
        itemBuilder: (context, index) => showRecommendItem(index));
  }
123456
```

就不行,直接整个页面白屏,然后log提示RenderBox 相关的信息,
但是在外面嵌套一个SizeBox,指定一下高度,就可以了.

```
 @override
  Widget build(BuildContext context) {
    return Container(
      height: 500,
      child: ListView.builder(
          itemCount: 3,
          itemBuilder: (context, index) => showRecommendItem(index)),
    );
  }
```