# GridView

- GridView
- GridView.builder
- GridView.count
- GridView.custom
- GridView.extent
- 

```dart
GridView({
    Key key,
    Axis scrollDirection = Axis.vertical,//滚动视图滚动的轴
    bool reverse = false,//滚动视图内容是否反转
    ScrollController controller,//一个可用于控制滚动视图滚动到的位置的对象
    bool primary,//是否是与父级关联的主滚动视图
    ScrollPhysics physics,//滚动视图给用户的响应方式
    bool shrinkWrap = false,//应该根据正在查看的内容确定滚动视图的范围
    EdgeInsetsGeometry padding,//滚动视图的padding
    @required this.gridDelegate,//设置滚动视图的属性
    bool addAutomaticKeepAlives = true,
    bool addRepaintBoundaries = true,
    bool addSemanticIndexes = true,
    double cacheExtent,//视口在可见区域之前和之后有一个区域，用于缓存用户滚动时即将可见的项目。
    List<Widget> children = const <Widget>[],
    int semanticChildCount,
});
```

```dart
GridView.count({
    Key key,
    Axis scrollDirection = Axis.vertical,//滚动方向
    bool reverse = false,//组件反向排序
    ScrollController controller,//滚动监听
    bool primary,//值为false，内容不足不可滑动，值为true，内容不足可以尝试滑动
    ScrollPhysics physics,//滑动类型设置
    bool shrinkWrap = false,//内容适配
    EdgeInsetsGeometry padding,//内边距
    @required int crossAxisCount,//列数
    double mainAxisSpacing = 0.0,//主轴的间距
    double crossAxisSpacing = 0.0,//横轴的间距
    double childAspectRatio = 1.0,//
    bool addAutomaticKeepAlives = true,
    bool addRepaintBoundaries = true,
    bool addSemanticIndexes = true,
    double cacheExtent,//设置预加载区域
    List<Widget> children = const <Widget>[],//子组件
    int semanticChildCount,//提供语义信息的子组件数量
    DragStartBehavior dragStartBehavior = DragStartBehavior.start,
    ScrollViewKeyboardDismissBehavior keyboardDismissBehavior = ScrollViewKeyboardDismissBehavior.manual,
    String restorationId,
    Clip clipBehavior = Clip.hardEdge,
  }) : gridDelegate = SliverGridDelegateWithFixedCrossAxisCount(
         crossAxisCount: crossAxisCount,
         mainAxisSpacing: mainAxisSpacing,
         crossAxisSpacing: crossAxisSpacing,
         childAspectRatio: childAspectRatio,
       ),
       childrenDelegate = SliverChildListDelegate(
         children,
         addAutomaticKeepAlives: addAutomaticKeepAlives,
         addRepaintBoundaries: addRepaintBoundaries,
         addSemanticIndexes: addSemanticIndexes,
       ),
       super(
         key: key,
         scrollDirection: scrollDirection,
         reverse: reverse,
         controller: controller,
         primary: primary,
         physics: physics,
         shrinkWrap: shrinkWrap,
         padding: padding,
         cacheExtent: cacheExtent,
         semanticChildCount: semanticChildCount ?? children.length,
         dragStartBehavior: dragStartBehavior,
         keyboardDismissBehavior: keyboardDismissBehavior,
         restorationId: restorationId,
         clipBehavior: clipBehavior,
       );
```

