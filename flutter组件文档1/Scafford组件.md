## 简介

`Scaffold` 实现了基本的 `Material` 布局。只要是在 `Material` 中定义了的单个界面显示的布局控件元素，都可以使用 `Scaffold` 来绘制。

提供展示**抽屉（drawers，比如：左边栏）**、**通知（snack bars）** 以及 **底部按钮（bottom sheets）**。

我们可以将 Scaffold 理解为一个布局的容器。可以在这个容器中绘制我们的用户界面。

## 源码图示

![源码图示](https://mxszs.oss-cn-beijing.aliyuncs.com/img/20180829105942301)

## 说明

### Scaffold 主要的属性说明

- **appBar**：显示在界面顶部的一个 AppBar
  相关连接：https://flutterchina.club/catalog/samples/
- **body**：当前界面所显示的主要内容
- **floatingActionButton**： 在 Material 中定义的一个功能按钮。
- **persistentFooterButtons**：固定在下方显示的按钮。https://material.google.com/components/buttons.html#buttons-persistent-footer-buttons
- **drawer**：侧边栏控件
- **bottomNavigationBar**：显示在底部的导航栏按钮栏。可以查看文档：[Flutter学习之制作底部菜单导航](https://blog.csdn.net/qq_18948359/article/details/81409861)
- **backgroundColor**：背景颜色
- **resizeToAvoidBottomPadding**： 控制界面内容 body
  是否重新布局来避免底部被覆盖了，比如当键盘显示的时候，重新布局避免被键盘盖住内容。默认值为 true。

### 代码示例

```js
class Scaffold extends StatefulWidget {
  /// Creates a visual scaffold for material design widgets.
  const Scaffold({
    Key key,
    this.appBar, //横向水平布局，通常显示在顶部（*）
    this.body, // 内容（*）
    this.floatingActionButton, //悬浮按钮，就是上图右下角按钮（*）
    this.floatingActionButtonLocation, //悬浮按钮位置
    //悬浮按钮在[floatingActionButtonLocation]出现/消失动画
    this.floatingActionButtonAnimator, 
    //在底部呈现一组button，显示于[bottomNavigationBar]之上，[body]之下
    this.persistentFooterButtons,
    //一个垂直面板，显示于左侧，初始处于隐藏状态（*）
    this.drawer,
    this.endDrawer,
    //出现于底部的一系列水平按钮（*）
    this.bottomNavigationBar,
    //底部持久化提示框
    this.bottomSheet,
    //内容背景颜色
    this.backgroundColor,
    //弃用，使用[resizeToAvoidBottomInset]
    this.resizeToAvoidBottomPadding,
    //重新计算布局空间大小
    this.resizeToAvoidBottomInset,
    //是否显示到底部，默认为true将显示到顶部状态栏
    this.primary = true,
    //
    this.drawerDragStartBehavior = DragStartBehavior.down,
  }) : assert(primary != null),
       assert(drawerDragStartBehavior != null),
       super(key: key);

```

### Scaffold.of 使用说明

关于 `Scaffold.of` 函数的说明：https://docs.flutter.io/flutter/material/Scaffold/of.html

显示 snackbar 或者 bottom sheet 的时候，需要使用当前的 BuildContext 参数调用 Scaffold.of 函数来获取 ScaffoldState 对象，然后使用 ScaffoldState.showSnackBar 和 ScaffoldState.showBottomSheet 函数来显示。

来自官方源码上面的例子。使用 SnackBar 的写法。

```js
@override
 Widget build(BuildContext context) {
   return new RaisedButton(
     child: new Text('SHOW A SNACKBAR'),
     onPressed: () {
       Scaffold.of(context).showSnackBar(new SnackBar(
         content: new Text('Hello!'),
       ));
     },
   );
 }
```

当 Scaffold 实际上是在同一个构建函数中创建时，构建函数的 BuildContext 参数不能用于查找 Scaffold（因为它位于返回的小部件的“上方”）。 因为在源码中 使用的是 `return new Scaffold(app:xxxx)`，在这种情况下面，通过在 Scaffold 中使用一个 `Builder` 来提供一个新的 BuildContext：

```js
@override
Widget build(BuildContext context) {
  return new Scaffold(
    appBar: new AppBar(
      title: new Text('Demo')
    ),
    body: new Builder(
      // Create an inner BuildContext so that the onPressed methods
      // can refer to the Scaffold with Scaffold.of().
      builder: (BuildContext context) {
        return new Center(
          child: new RaisedButton(
            child: new Text('SHOW A SNACKBAR'),
            onPressed: () {
              Scaffold.of(context).showSnackBar(new SnackBar(
                content: new Text('Hello!'),
              ));
            },
          ),
        );
      },
    ),
  );
}

```

另外呢，按照官方的说法，可以将我们的构建函数拆分到多个 `Widgets`中。分别引入新的 BuildContext 来获取 Scaffold.