###  State

在说到StatefulWidget之前，先说下State。State的作用有两点：

1. 在widget构建的时候可以被同步读取；
2. 在widget的生命周期中可能会被改变。

####  State生命周期

State的生命周期有四种状态：

* created：当State对象被创建时候，State.initState方法会被调用；
* initialized：当State对象被创建，但还没有准备构建时，State.didChangeDependencies在这个时候会被调用；
* ready：State对象已经准备好了构建，State.dispose没有被调用的时候；
* defunct：State.dispose被调用后，State对象不能够被构建。

![State LifeCycle](http://whysodiao.com/images/State LifeCycle.png)

完整生命周期如下：

* 创建一个State对象时，会调用StatefulWidget.createState；
* 和一个BuildContext相关联，可以认为被加载了（mounted）；
* 调用initState；
* 调用didChangeDependencies；
* 经过上述步骤，State对象被完全的初始化了，调用build；
* 如果有需要，会调用didUpdateWidget；
* 如果处在开发模式，热加载会调用reassemble；
* 如果它的子树（subtree）包含需要被移除的State对象，会调用deactivate；
* 调用dispose,State对象以后都不会被构建；
* 当调用了dispose,State对象处于未加载（unmounted），已经被dispose的State对象没有办法被重新加载（remount）。

####  setState

State中比较重要的一个方法是`setState`，当修改状态时，widget会被更新。比方说点击CheckBox，会出现选中和非选中状态之间的切换，就是通过修改状态来达到的。

查看setState源码，在一些异常的情况下将会抛出异常：

* 传入的为null；
* 处在defunct阶段；
* created阶段还没有被加载（mounted）；
* 参数返回一个Future对象。

检查完一系列异常后，最后调用代码如下：

```
_element.markNeedsBuild();
```

markNeedsBuild内部，则是通过标记element为dirty，在下一帧的时候重建（rebuild）。可以看出setState并不是立即生效，它只是将widget进行了标记，真正的rebuild操作，则是等到下一帧的时候才会去进行。

### StatefulWidget和StatelessWidget

StatefulWidget和StatelessWidget如下所示



![image-20210207133350980](https://luckly007.oss-cn-beijing.aliyuncs.com/images/image-20210207133350980.png)

一个StatelessWidget可以用多个不同的BuildContext构建，而一个StatefulWidget会为每个BuildContext创建一个State对象。

#### StatelessWidget

对于StatelessWidget，build方法会在如下三种情况下调用，

1. widget第一次被插入到树中；
2. widget的父节点更改了配置（configuration）；
3. widget依赖的InheritedWidget改变了。


```
class GreenFrog extends StatelessWidget {
  const GreenFrog({ Key key }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return new Container(color: const Color(0xFF2DBD3A));
  }
}
```

#### StatefulWidget

StatefulWidget的两个主要类别：

1. 在initState中创建资源，在dispose中销毁，但是不依赖于InheritedWidget或者调用setState方法，这类widget基本上用在一个应用或者页面的root；
2. 使用setState或者依赖于InheritedWidget，这种在营业生命周期中会被重建（rebuild）很多次。

```
class YellowBird extends StatefulWidget {
  const YellowBird({ Key key }) : super(key: key);

  @override
  _YellowBirdState createState() => new _YellowBirdState();
}

class _YellowBirdState extends State<YellowBird> {
  @override
  Widget build(BuildContext context) {
    return new Container(color: const Color(0xFFFFE306));
  }
}
```

