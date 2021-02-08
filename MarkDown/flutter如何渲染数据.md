作为一个前端，如果只会设计界面，那太low了，毕竟，前端的任务不仅仅是设计界面，还要完成数据的交互，比如，你对我这个文章点赞

那么现在我就对这几天困惑我的问题做一梳理

如何利用后端API 来渲染页面数据

## 1.首先引入第三方库

dio以及http

https://pub.dev/packages/dio



关于dio

### 1.Depend on it

Add this to your package's pubspec.yaml file:

```yaml
dependencies:
  dio: ^3.0.10
```

### 2. Install it

You can install packages from the command line:

with pub:

```shell
$ pub get
```

with Flutter:

```shell
$ flutter pub get
```

Alternatively, your editor might support `pub get` or `flutter pub get`. Check the docs for your editor to learn more.

### 3. Import it

Now in your Dart code, you can use:

```dart
import 'package:dio/dio.dart';
```

https://pub.dev/packages/http

关于http

### 1. Depend on it

Add this to your package's pubspec.yaml file:

```yaml
dependencies:
  http: ^0.12.2
```

### 2. Install it

You can install packages from the command line:

with pub:

```shell
$ pub get
```

with Flutter:

```shell
$ flutter pub get
```

Alternatively, your editor might support `pub get` or `flutter pub get`. Check the docs for your editor to learn more.

### 3. Import it

Now in your Dart code, you can use:

```dart
import 'package:http/http.dart';
```



## 2.定义数据模型

可以利用工具：https://github.com/fluttercandies/JsonToDart

或者网站

ProductModel.dart 文件

```
class ProductModel {
  List<ProductItemModel> result;

  ProductModel({this.result});

  ProductModel.fromJson(Map<String, dynamic> json) {
    if (json['result'] != null) {
      result = new List<ProductItemModel>();
      json['result'].forEach((v) {
        result.add(new ProductItemModel.fromJson(v));
      });
    }
  }

  Map<String, dynamic> toJson() {
    final Map<String, dynamic> data = new Map<String, dynamic>();
    if (this.result != null) {
      data['result'] = this.result.map((v) => v.toJson()).toList();
    }
    return data;
  }
}

class ProductItemModel {
  String sId;
  String title;
  String cid;
  Object price;   //所有的类型都继承 Object
  String oldPrice;
  String pic;
  String sPic;

  ProductItemModel(  //构造函数
      {this.sId,
      this.title,
      this.cid,
      this.price, //价格
      this.oldPrice,
      this.pic,
      this.sPic});

  ProductItemModel.fromJson(Map<String, dynamic> json) {
    sId = json['_id'];
    title = json['title'];
    cid = json['cid'];
    price = json['price'];
    oldPrice = json['old_price'];
    pic = json['pic'];
    sPic = json['s_pic'];
  }

  Map<String, dynamic> toJson() {
    final Map<String, dynamic> data = new Map<String, dynamic>();
    data['_id'] = this.sId;
    data['title'] = this.title;
    data['cid'] = this.cid;
    data['price'] = this.price;
    data['old_price'] = this.oldPrice;
    data['pic'] = this.pic;
    data['s_pic'] = this.sPic;
    return data;
  }
}
```

3.

config文件夹里

Config.dart

```
class Config{
  static String domain="http://jd.itying.com/";
}
```



引入：

```
import '../../config/Config.dart';
import 'package:dio/dio.dart';

//轮播图类模型
import '../../model/FocusModel.dart';
```





```

class _HomePageState extends State<HomePage> {
  List _focusData = [];
  List _hotProductList = [];
  List _bestProductList = []; //定义属性

  @override
  void initState() {
    super.initState();
    _getFocusData();
    _getHotProductData();
    _getBestProductData(); //初始化时调用
  }
```



```

  //获取轮播图数据
  _getFocusData() async {
    var api = '${Config.domain}api/focus';
    var result = await Dio().get(api);
    var focusList = FocusModel.fromJson(result.data);
    setState(() {
      this._focusData = focusList.result;
    });
  }







  //获取猜你喜欢的数据
  _getHotProductData() async {
    var api = '${Config.domain}api/plist?qiis_hot=1'; //取出接口
    var result = await Dio().get(api); //请求数据
    var hotProductList = ProductModel.fromJson(result.data); //存入数据 格式化数据
    setState(() {
      this._hotProductList = hotProductList.result; //
    });
  }
  
  
  
  
  
  
  void _getMyTag() async {
    var myTagBody =
        await httpRequest(Config.HTTP_ROOT + Req.REQ_GET_MY_MOMENTTAG, {}); //请求数据
    if (myTagBody.code == 0) {
      var mymodel = TagListModel.fromJson(myTagBody.data); //格式化数据 
      _myTags = mymodel.tags;
      _myTags.insert(0, Tags(id: 2, tag_name: "附近"));
      _myTags.insert(0, Tags(id: 1, tag_name: "热门"));
      _initLists();
    }
  }
  
  
  
  
  
  
  
  
  //获取热门推荐的数据
  _getBestProductData() async {
    var api = '${Config.domain}api/plist?is_best=1';
    var result = await Dio().get(api);
    var bestProductList = ProductModel.fromJson(result.data);    
    setState(() {
      this._bestProductList = bestProductList.result; 
    });
  }

```



```

  //轮播图
  Widget _swiperWidget() {
    if (this._focusData.length > 0) {
      return Container(
        child: AspectRatio(
          aspectRatio: 2 / 1,
          child: Swiper(
              itemBuilder: (BuildContext context, int index) {
               
               
                pic = Config.domain + pic.replaceAll('\\', '/');
                return new Image.network(
                  "${pic}",
                  fit: BoxFit.fill,
                );
              },
              itemCount: this._focusData.length,
              pagination: new SwiperPagination(),
              autoplay: true),
        ),
      );
    } else {
      return Text('加载中...');
    }
  }
  
   //热门商品

  Widget _hotProductListWidget() {
    if (this._hotProductList.length > 0) { //首先判断
      return Container(
        height: ScreenAdaper.height(234),
        padding: EdgeInsets.all(ScreenAdaper.width(20)),
        child: ListView.builder(
          scrollDirection: Axis.horizontal,
          itemBuilder: (contxt, index) {
            //处理图片
            String sPic = this._hotProductList[index].sPic;
            sPic = Config.domain + sPic.replaceAll('\\', '/');  //好处理

            return Column(
              children: <Widget>[
                Container(
                  height: ScreenAdaper.height(140),
                  width: ScreenAdaper.width(140),
                  margin: EdgeInsets.only(right: ScreenAdaper.width(21)),
                  child: Image.network(sPic, fit: BoxFit.cover), 
                ),
                Container(
                  padding: EdgeInsets.only(top: ScreenAdaper.height(10)),
                  height: ScreenAdaper.height(44),
                  child: Text(
                    "¥${this._hotProductList[index].price}",  //看仔细
                    style: TextStyle(color: Colors.red),
                  ),
                )
              ],
            );
          },
          itemCount: this._hotProductList.length,
        ),
      );
    } else {
      return Text("");
    }
  }

  //推荐商品
  Widget _recProductListWidget() {
   
    var itemWidth = (ScreenAdaper.getScreenWidth() - 30) / 2;
    return Container(
      padding: EdgeInsets.all(10),
      child: Wrap(
        runSpacing: 10,
        spacing: 10,
        children: this._bestProductList.map((value) {

          //图片
          String sPic=value.sPic;
          sPic=Config.domain+sPic.replaceAll('\\', '/');

          return Container(
            padding: EdgeInsets.all(10),
            width: itemWidth,
            decoration: BoxDecoration(
                border: Border.all(
                    color: Color.fromRGBO(233, 233, 233, 0.9), width: 1)),
            child: Column(
              children: <Widget>[
                Container(
                  width: double.infinity,
                  child: AspectRatio(
                    //防止服务器返回的图片大小不一致导致高度不一致问题
                    aspectRatio: 1 / 1,
                    child: Image.network(
                      "${sPic}",
                      fit: BoxFit.cover,
                    ),
                  ),
                ),
                Padding(
                  padding: EdgeInsets.only(top: ScreenAdaper.height(20)),
                  child: Text(
                    "${value.title}",
                    maxLines: 2,
                    overflow: TextOverflow.ellipsis,
                    style: TextStyle(color: Colors.black54),
                  ),
                ),
                Padding(
                  padding: EdgeInsets.only(top: ScreenAdaper.height(20)),
                  child: Stack(
                    children: <Widget>[
                      Align(
                        alignment: Alignment.centerLeft,
                        child: Text(
                          "¥${value.price}",
                          style: TextStyle(color: Colors.red, fontSize: 16),
                        ),
                      ),
                      Align(
                        alignment: Alignment.centerRight,
                        child: Text( "¥${value.oldPrice}",
                            style: TextStyle(
                                color: Colors.black54,
                                fontSize: 14,
                                decoration: TextDecoration.lineThrough)),
                      )
                    ],
                  ),
                )
              ],
            ),
          );
        }).toList(),
      ),
    );
  }
```







```
  // if (myDynamics.length > 0) {
    //   if (index != 0) {
    //     return Column(
    //       children: [MyDynamicItem()],
    //     );
    //   }
    //   index--;

    //   if (index < myDynamics.length) {
    //     return _createItem(myDynamics[index]);
    //   } else {
    //     index -= myDynamics.length;
    //   }
    // }

    // return MyDynamicItem();

```

