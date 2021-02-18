 

首先可以看[官方文档](https://github.com/rrousselGit/provider/blob/master/resources/translations/zh-CN/README.md)



不同组件之间的数据通信和共享

**Provider的三个好兄弟：**

老大 -- MultiProvider（供货商）

作用：

包裹根节点，将我们准备好的Model和View建立联系；可以想象成一只巨大章鱼包裹了一棵大树，他可以将触手随意的伸向某一节树枝；

老二 -- Providers（货源）

作用：

负责预先定义好，需要被共享的Model； 

**大儿子**：provider 不需要被监听，有的常量或者方法，根本不需要“牵一发而动全身”，也就是说他们不会被要求随着变动而变动，这样的需求就用大儿子定义；

**二儿子**： ChangeNotifierProvider 对这个儿子父亲要求的就比较多了，它会随着某些数据改变而被通知更新，也就是说，比如这个Model被用在多个page，那么当其中一处被改变时，他就应该告诉其他的地方，改更新了，这样的需求就是用二儿子定义；

**三儿子**： ChangeNotifierProxyProvider  对这个儿子要求就更高了，所以它的名字都比老二长一截，他不仅要像老二一样，通知更新，还要协调Model与Model之间的更新，比如一个ModelA依赖另一个ModelB，ModelB更新，他就要让依赖它的ModelA也随之更新，这就是老三的活；

老三 -- Provider.of<T>(context) / Widget Consumer（消费者）

作用：负责从各个节点取出数据来使用的

几个概念理解一下。

\- ChangeNotifier（被观察对象）

Provider借助于ChangeNotifier实现发布者-订阅者模式。

```javascript
class ChangeNotifier implements Listenable {
  List listeners=[];
  @override
  void addListener(VoidCallback listener) {
     //添加监听器
     listeners.add(listener);
  }
@override
void removeListener(VoidCallback listener) {
//移除监听器
    listeners.remove(listener);
  }
void notifyListeners() {
//通知所有监听器，触发监听器回调
    listeners.forEach((item)=>item());
  }

  ...
}
```

具体的细节我们不再具体去探讨，今天就来看看如何使用。

\- Provider（被观察对象提供者）

\- Consumer（观察者）

\- Provider.of

首先，我们假定这样一个场景，第一个界面显示用户的昵称，然后我们在第二个界面修改昵称再返回观察第一个界面的显示情况。

首先我们建立一个用户信息操作类UserInfoModel使它继承ChangeNotifier

```javascript
class UserInfoModel with ChangeNotifier {
  String _nickName = "userName";
  // 读方法
String get nickName => _nickName;
// 写方法
void updateNickName(String nickName) {
    _nickName=nickName;
    notifyListeners();// 通知听众刷新
  }
}
```

#### **数据更新**

可以看到我们在UserInfoModel中定义了_nickName属性并设置相关获取与设置属性的方法，在设置属性方法中我们通过notifyListeners方法告知数据刷新。

因为Provider 是InheritedWidget实现的，所以数据也是有流向的，所以我们需要把ChangeNotifierProvider.value放在两个界面上面的位置，这样我们一旦更新一个页面的数据另外一个页面就也可以获取到。

首先，我们定一个入口

```javascript
void main() {
  runApp(new MaterialApp(
    home:   MyApp(),
  ));
}
```

然后定义入口Widget

```javascript
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider.value(
        value: UserInfoModel(),
        child: MaterialApp(
          home: FirstPage(),
        )
    );
  }
}
```

第一个界面我们定义一个按钮和一个Text用来显示第二个界面更新的数据

我们使用context.watch()方法来获取到对象，并监听

```javascript
class FirstPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {

    return Scaffold(
      appBar: AppBar(
        title: Text("ProviderTitle"),
      ),
      body: Center(
        child: Column(
          children: [
            Text( context.watch<UserInfoModel>().nickName),
            RaisedButton(
              child: Text("去设置界面"),
              onPressed: () {
                Navigator.push(context, MaterialPageRoute(builder: (context) {
                  return SecondPage();
                }));
              },
            )
          ],
        ),
      ),
    );
  }
}
```

第二个界面我们定义一个输入框和一个按钮，点击按钮就把输入框的值设置给Provider

我们使用 Provider.of(context)方法来获取监听对象并进行修改操作。

```javascript
class SecondPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    TextEditingController _unameController = TextEditingController();
  var userModel=  Provider.of<UserInfoModel>(context);
    return Scaffold(
      appBar: AppBar(
        title: Text("ProviderTitle"),
      ),
      body: Column(
        children: [
          TextField(
            autofocus: true,
            controller: _unameController,
            decoration: InputDecoration(
                labelText: "用户名",
                hintText: "用户名或邮箱",
                prefixIcon: Icon(Icons.person)),
          ),
          RaisedButton(
            onPressed: () {
              userModel.updateNickName(_unameController.text);

            },
            child: Text("设置"),
          )
        ],
      ),
    );
  }
}
```



#### **同时管理多个数据**

在上面我们介绍了如何通过Provider来管理用户名数据，那么如果涉及多个数据我们该如何来管理呢？

通常情况下我们可以把多个数据封装成一个完整的数据来进行操作，这种方法在数据间相互关联性比较接近的情况下是可以实现的，但是如何遇到数据关系不大的情况下还采用这种方法的话就会造成界面Widget不必要的重绘。

当然，Provider也为我们提供了解决方法，MultiProvider可以让我们同时管理多个数据。

还是以上面的例子来进行说明，我们在前面用户名的基础上又增加了一个“家庭地址”，在第一个界面新增一个Text用来显示家庭地址，在第二个界面新增一个输入框用来输入家庭地址。

首先我们定义一个用来管理地址的Model

```javascript
class UserLocationModel with ChangeNotifier {
  String _address = "address";

  // 读方法
Stringget address => _address;

// 写方法
void setAddress(String address) {
    _address = address;
    notifyListeners(); // 通知听众刷新,bao
  }
}
```

然后我们使用MultiProvider来管理多个Model

```javascript
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    // 通过 Provider 组件封装数据资源
return MultiProvider(providers: [
      ChangeNotifierProvider.value(value: UserInfoModel()),
      ChangeNotifierProvider.value(value: UserLocationModel())
    ], child: MaterialApp(home: FirstPage()));
  }
}
```

然后在第一个界面接收并显示数据

```javascript
class FirstPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("ProviderTitle"),
      ),
      body: Center(
        child: Column(
          children: [
            Text("用户名:${context.watch<UserInfoModel>().nickName}"),
            Text("家庭地址:${context.watch<UserLocationModel>().address}"),
            RaisedButton(
              child: Text("去设置界面"),
              onPressed: () {
                Navigator.push(context, MaterialPageRoute(builder: (context) {
                  return SecondPage();
                }));
              },
            )
          ],
        ),
      ),
    );
  }
}
```

在第二个界面设置数据

```javascript
class SecondPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    TextEditingController _unameController = TextEditingController();
    TextEditingController _homeController = TextEditingController();
    var userModel = Provider.of<UserInfoModel>(context);
    var homeModel = Provider.of<UserLocationModel>(context);
    return Scaffold(
      appBar: AppBar(
        title: Text("ProviderTitle"),
      ),
      body: Column(
        children: [
          TextField(
            autofocus: true,
            controller: _unameController,
            decoration: InputDecoration(
                labelText: "用户名",
                hintText: "用户名或邮箱",
                prefixIcon: Icon(Icons.person)),
          ),

          RaisedButton(
            onPressed: () {
                userModel.updateNickName(_unameController.text);
            },
            child: Text("设置用户名"),
          ),
          TextField(
            autofocus: true,
            controller: _homeController,
            decoration: InputDecoration(
                labelText: "家庭地址",
                hintText: "请输入家庭地址",
                prefixIcon: Icon(Icons.person)),
          ),
          RaisedButton(
            onPressed: () {
            homeModel.setAddress(_homeController.text);

            },
            child: Text("设置家庭地址"),
          ),

        ],
      ),
    );
  }
}
```



当然我们也可以使用Consumer2方法来获取多个数据的传递,这样就不需要再创建UserInfoModel和UserLocationModel了。

```javascript
class FirstPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("ProviderTitle"),
      ),
      body: Center(
          child: Consumer2<UserInfoModel, UserLocationModel>(
//builder 函数以参数的形式提供了数据资源
              builder: (context, UserInfoModel userInfoModel,
                      UserLocationModel userLocationModel, _) =>
                  Column(
                    children: [
                      Text("用户名:${userInfoModel.nickName}"),
                      Text("家庭地址:${userLocationModel.address}"),
                      RaisedButton(
                        child: Text("去设置界面"),
                        onPressed: () {
                          Navigator.push(context,
                              MaterialPageRoute(builder: (context) {
                            return SecondPage();
                          }));
                        },
                      )
                    ],
                  ))),
    );
  }
}
```

### 小结

- Provider是对InheritedWidget的封装方便我们在多个界面间传递数据
- Provider支持同时管理多个数据的状态
- 可以借助与Consumer-Consumer6方法来管理多个数据状态





一个组件中的多个子组件共享数据