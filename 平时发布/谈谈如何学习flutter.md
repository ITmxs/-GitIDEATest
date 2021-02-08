

[TOC]



## 为什么要使用Flutter?

为什么学习flutter是很多人的疑问，在我个人看来

- 提高开发效率
  - 同一份代码开发iOS和Android
  - 用更少的代码做更多的事情
  - 轻松迭代
    - 在应用程序运行时更改代码并重新加载（通过热重载）
    - 修复崩溃并继续从应用程序停止的地方进行调试
- 创建美观，高度定制的用户体验
  - 受益于使用Flutter框架提供的丰富的Material Design和Cupertino（iOS风格）的widget
  - 实现定制、美观、品牌驱动的设计，而不受原生控件的限制



作为一个零基础的同学。学习flutter无非就是从安装配置环境开始，这个是作为编程人员最基础的了，在这儿，记住一个地址：[https://flutter.cn/](https://flutter.cn/)

### path 环境变量

如果你想在 Windows 控制台中运行 Flutter 命令，需要按照下面的步骤来将 Flutter 的运行文件路径加入到 `PATH` 环境变量。

- 在开始菜单的搜索功能键入「env」，然后选择 **编辑系统环境变量**。
- 在 **用户变量** 一栏中，检查是否有 **Path** 这个条目：
  - 如果存在这个条目，以 `;` 分隔已有的内容，加入 `flutter\bin` 目录的完整路径。
  - 如果不存在的话，在用户环境变量中创建一个新的 `Path` 变量，然后将 `flutter\bin` 所在的完整路径作为新变量的值。

### 运行 `flutter doctor`

## 安装 Android Studio或者VScode

我用 VScode，毕竟轻量化，这儿有几个插件值得推荐

1. flutter
2. dart
3. Bracket Pair Colorizer 2
4. Material Icon Theme
5. Pubspec Assist
6. Better Comments
7. [Gitlens](https://gitlens.amod.io/#)
8. Bonus: Move Bottom Panel to the side

#### Bonus: Move Bottom Panel to the side.

<img src="https://luckly007.oss-cn-beijing.aliyuncs.com/images/20210208103518.png" alt="img" style="zoom:33%;" />

<img src="https://luckly007.oss-cn-beijing.aliyuncs.com/images/20210208103525.png" alt="img" style="zoom:33%;" />

Right looks infinitely better.

Having the Panel at the bottom takes up a lot of space that you could use to read more code. Move your panel to the side instead (your code shouldn't exceed 80 chars anyway) to use space more efficiently. Open Settings by hitting `Ctrl` + `,`. Then, search for `"Panel location"`. Change it from `bottom` to `right`.



这个时候环境搭建基本完成，就开始学习吧，学习flutter，建议首先看看dart语法相关的东西

## [高效 Dart 语言指南：API 设计](https://dart.cn/guides/language/effective-dart/design)

以及类型之间的相互转换，

List Set Map 还是要重点理解一下，

关于流程控制语句，if else while do while break container switch  case 都是有必要的

接下来就是对dart 的一些库的了解

库重名的解决，as  ，以及show 屏蔽其他，hide显示其他 

学习完这些，再去学习flutter相关的，

相信很快就能做一款自己的app了，

flutter相关的，其实也就是一个widget的学习了，widget，分为两种

Flutter 中的自定义控件分为**两类**：

- **StatelessWidget 无状态控件**
  - 纯展示型控件，没有自己的私有数据和业务逻辑
- **StatefulWidget 有状态控件**
  - 业务逻辑控件，可以定义自己的私有数据和业务逻辑

## StatelessWidget 是无状态控件，没有自己的私有数据，是纯展示型的控件，基本定义过程如下：

```dart
class MyMidget extends StatelessWidget {
  // 构造函数：
  // 其中的 this.title 定义的是命名参数
  // @required 用来规定某个数据在初始化的时候是必须要提供的，否则会报错。
  MyApp({Key key, @required this.title}) : super(key: key);

  // final：用于修饰变量，表示单赋值（single-assignment），
  // 使用final修饰的变量必须进行初始化，一旦被赋值之后，不能够再次被赋值,否则编译会报错。
  final String title;

  // build 函数相当于 react.js 中的 render 函数
  // 必须 return 一个 UI 结构，用来构建当前控件对应的UI界面
  // 在 StatelessWidget 控件中，build 函数是必须的
  @override
  Widget build(BuildContext context) {
    return Text(title);
  }
}
```

基本使用过程如下：

```dart
void main() => runApp(
  // MyApp() 函数的调用，是 new MyApp() 的简写形式，
  // 其中 title 是构造函数的参数，
  // 且必须传递 title 数据，因为它已被标记为 @required 必填项
  MyApp(title: 'Flutter初体验鸭')
);
```

StatefulWidget 是有状态控件，这样的控件拥有自己的私有数据和业务逻辑，基本定义过程如下：

## 定义有状态的控件StatefulWidget

```dart
// 定义一个 电影详情 控件，继承自 StatefulWidget
class MovieDetail extends StatefulWidget {
  // 构造函数，初始化当前组件必须的 id 属性
  MovieDetail({Key key, @required this.id}) : super(key: key);

  // 电影的Id值
  final String id;

  // StatefulWidget 控件必须实现 createState 函数
  // 在 createState 函数中，必须返回一个继承自 State<T> 状态类的对象
  // 这里的 _MovieDetailState 就继承自 State<T>
  _MovieDetailState createState() => new _MovieDetailState();
}
```

### 定义继承自 `State<T>` 的状态类

```dart
// 这个继承自 State<T> 的类，专门用来定义有状态控件的 业务逻辑 和 私有数据
class _MovieDetailState extends State<MovieDetail> {
  // build 函数是必须的，用来渲染当前有状态控件对应的 UI 结构
  @override
  Widget build(BuildContext context) {
    // 注意：在这个 _MovieDetailState 状态类中，可以使用 widget 对象访问到 StatefulWidget 控件中的数据并直接使用
    // 例如：widget.id
    return Text('MovieDetail --' + widget.id);
  }
}
```

### 定义和修改私有数据

```dart
// 这个继承自 State<T> 的类，专门用来定义有状态控件的 业务逻辑 和 私有数据
class _MovieDetailState extends State<MovieDetail> {
  // 1. 定义私有状态数据【以 _ 开头的数据，是当前类的私有数据】
  int _count;

  // 2. 通过 initState 生命周期函数，来初始化私有数据
  @override
  void initState() {
    super.initState();
    // 2.1 把 _count 的值初始化为 0
    _count = 0;
  }

  // build 函数是必须的，用来渲染当前有状态控件对应的 UI 结构
  @override
  Widget build(BuildContext context) {
    // 注意：在这个 _MovieDetailState 状态类中，可以使用 widget 对象访问到 StatefulWidget 控件中的数据并直接使用
    // 例如：widget.id
    return Column(
      children: <Widget>[
        Text('MovieDetail --' + widget.id + ' --- ' + _count.toString()),
        RaisedButton(
          child: Icon(Icons.add),
          // 3. 指定点击事件的处理函数为 _add
          onPressed: _add,
        )
      ],
    );
  }

  // 4. 定义 _count 自增的函数【以 _ 开头的函数，是私有函数】
  void _add() {
    // 如果要为私有数据重新赋值，必须调用 setState() 函数
    setState(() {
      // 让私有数据 _count 自增 +1
      _count++;
    });
  }
}
```

两种状态的Widget学习完了，就是对其他的widget的学习了，

## 学习widget

只要记住在他的核心里面，万物皆可widget

我常用的也就是例如Container、Padding、Center、Align、Row、Column、Stack、ListView等也有上十种。

Flutter单节点布局控件功能分类：

<img src="https://luckly007.oss-cn-beijing.aliyuncs.com/images/20210208104449.jpg" alt="Flutter单节点布局控件功能分类" style="zoom:50%;" />

对于这些，一定要会理解，会用，当然，前期的使用还是比较简单的。控件虽然这么多，但是大部分不会挨个去尝试。对于大部分人而言，都是佛系的用法，一个控件能够使用，就一直用到死。在布局上，大方向还是不停的`拆`，把一张设计图，拆成一棵树，每个节点根据需要，选择合适的控件，然后从根部开始不停嵌套，布局就完成了。到了后期，就不仅仅是页面的布局了，而在于对**控件的选择**

控件种类繁多，真正使用的时候该如何去选择呢？有万金油的做法，不管啥都用Container，这也是很多初接触的人经常干的方式。这么做的确可以按照设计图把布局给实现了，但是会涉及到一些性能上的问题。在我看来，控件的选择，`按照控件最小功能的标准去选择`。例如需要将子节点居中，可以使用Container设置alignment的方式，也可以使用Center。但是从功能上，Center是最小级别的，因此选择它的话，额外的开销会最小。将UI实现了，这只是最基本的，当达到这一步了，应该更多的去思考，如何更好的布局，使得性能更高。

而在我对于以下我称为多节点控件

- 列表：GridView、ListView；
- 单列单行或者多列多行：Row、Column、Flow、Wrap、ListBody、Table；
- 显示位置相关：Stack、IndexedStack、CustomMultiChildLayout。

这几个空间还是应该学细一点，GridView、ListView、Row、Column、Stack，这几个控件基本上涵盖了大部分的布局了。所以更应该重点把握。

## 性能优化

刚才也提到，性能优化，关于性能优化，可能仁者见仁，并没有一个统一的说法，毕竟现在Flutter各方面都还不完善。但是，大方向还是有的，尽量使用功能集更小的控件，这个对于渲染效率上还是有所帮助的。

### 优化

在这里我试着去列举一些，并不一定都正确。

- 如果一个布局多个控件都可以完成，则使用功能最小的，可以参照上面控件分类中的功能划分来做取舍；
- 对于多节点控件，如果单节点控件满足需求的话，则去使用单节点控件进行布局；
- 对于ListView，标准构造函数适用于条目比较少的情况，如果条目较多的话，尽量使用ListView.builder；
- 对于GridView，如果需要展示大量的数据的话，尽量使用GridView.builder；
- Flow、Wrap、Row、Column四个控件，单纯论效率的话，Flow是最高效的，但是使用起来是最复杂的；
- 如果是单行或者单列的话，Row、Column比Table更高效；
- Stack和CustomMultiChildLayout如果同时满足需求的话，CustomMultiChildLayout在某些时候效率会更高一些，但是取决于Delegate的实现，且使用起来更加的复杂；

上面所列的比较杂，但是归纳起来，无非这几点：

- 功能越少的控件，效率越高；
- ListView以及GridView的builder构造函数效率更高；
- 实现起来比较复杂的控件，效率一般会更高。

### 选择

控件的选择，个人觉得把握大方向就够了。如果时间紧急，以实现效率最优先，如果时间充裕的话，可以按照一些优化细则，去做一些选择。单纯控件层面，带来性能上的改进毕竟十分有限。



可能需要组建的封装之类的了，良好的代码封装，你会发现可读性不仅好，还能提高编码的乐趣，所以我的目标就是写出诗歌一样的代码。

比如写一个登陆页面：

利用stack结合Positioned，实现相对定位，你就会发现，代码结构好看不说，也利于后期的维护

```dart
 @override
  Widget build(BuildContext context) {
    return Scaffold(
      ///填充布局
      body: Container(
          width: double.infinity,
          height: double.infinity,
          //点击空白区域 隐藏键盘
          child: GestureDetector(
            onTap: () {
              _userNameFocusNode.unfocus();
              _passwordFocusNode.unfocus();
            },
            child: Stack(
              children: [
                //第一层 背景
                buildBackgroundWidget(),
                //第二层 气泡
                Positioned.fill(
                  child: BubbleWidget(),
                ),
                //第三层 高斯模糊
                buildFliterWidget(),
                //第四层 logo
                buildLogoWidget(),
                //第五层来个输入层
                buildInputWidget(context),
                //第六层 关闭按钮
                buildCloseWidget(context)
              ],
            ),
          )),
    );
```

这也仅仅是页面布局，后期很重要的就是与后端的数据进行交互了。在这个时候，或许用http也可以，但是还是强烈建议，利用dio

至于其他的一些插件，比如 shared_preferences，share，permission_handler，provider等等插件

之后对于Android和ios底层还要有一些理解，这样，对于个性化需求，也就不成问题了，学习是个漫长的过程，需要不断坚持，

后期我会结合我的学习，工作出一个系列，希望大家继续关注

## 我的系列内容主要包括

1.各个组建的用法，走一步，看一步，毕竟组件挺多的

2.我平时用到的三方插件，目前就有36种，我会出一个专栏

3.平时用到的一些经验，我会利用Demo进行演示

4.遇到的一些报错，以及解决方案

如果大家还有什么想看的，欢迎在评论区留言，就让我们一起利他共享，求实精进。

明天会出 一个提纲，大家的喜欢和点赞就是我最大的鼓励哦。

