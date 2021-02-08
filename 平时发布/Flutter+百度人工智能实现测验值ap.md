# Flutter 颜值大师

基于 **Flutter + 百度人工智能** 开发出的一款测颜值的 App。

**最重要的一点：一颗满怀学习热情的心**





# 项目核心知识点

## 1.渲染头部区域

```dart
// 头部 AppBar 区域
appBar: AppBar(
  title: Text(
  "人脸识别",
    // 设置标题文字样式
    style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
  ),
  // 设置标题居中显示
  centerTitle: true,
)
```

## 2. 渲染多个浮动按钮

正常情况下，一个页面中，通过 `floatingActionButton` 选项，默认只能渲染一个浮动按钮。 如果需要渲染多个浮动按钮，可以通过 `ButtonBar` 控件来实现，代码示例如下：

```dart
floatingActionButton: ButtonBar(
  // alignment 属性用来指定子元素如何在横轴上进行排列
  // MainAxisAlignment.spaceAround 表示分散对齐
  alignment: MainAxisAlignment.spaceAround,
  // 子元素
  children: <Widget>[
    // 第一个浮动按钮
    FloatingActionButton(
      onPressed: () {},
      tooltip: 'takephoto',
      child: Icon(Icons.photo_camera),
    ),
    // 第二个浮动按钮
    FloatingActionButton(
      onPressed: () {},
      tooltip: 'takepicture',
      child: Icon(Icons.photo_library),
    )
  ],
)
```

## 3. 使用第三方插件实现选择照片的功能

一些特殊的功能，可以在插件商店中搜索对应的插件，从而轻松实现，插件商店的地址为 https://pub.dev/flutter

1. 在 `pubspec.yaml` 的 `dependencies` 节点中，新增插件如下：

```yaml
dependencies:
  image_picker: ^0.6.7+4
```

1. 在 `lib/main.dart` 文件的头部，导入对应的插件：

```dart
import 'package:image_picker/image_picker.dart';
```

1. 在 `_MyHomePageState` 这个状态管理类中，定义 `_image` 私有数据，用来存储用户选择的照片：

```dart
class _MyHomePageState extends State<MyHomePage> {
  // 用户通过摄像头或图片库选择的照片
  File _image;
}
```

1. 在 `lib/main.dart` 文件的头部，导入 `File` 类对应的类库：

```dart
import 'dart:io';
```

1. 在 `lib/main.dart` 中，定义函数 `choosePic` 来实现选取照片的功能：

```dart
// 点击按钮，选择图片
// 形参中的 source 为选取照片的方式，有两种，分别为：
//    ImageSource.camera   从相机拍照并得到照片
//    ImageSource.gallery  从本地相册选择照片
void choosePic(source) async {
  // 得到选取的照片
  var image = await ImagePicker.pickImage(source: source);

  setState(() {
    _image = image;
    faceInfo = null;
  });

  // 如果选取的照片为空，则不执行后续人脸检测的业务逻辑
  if (image == null) {
    return;
  }
}
```

1. 在浮动按钮的 `onPressed` 事件处理函数中，调用第 5 步中定义的 `choosePic` 函数，并把选取照片的方式传递到函数中：

```dart
floatingActionButton: ButtonBar(
  alignment: MainAxisAlignment.spaceAround,
  children: <Widget>[
    FloatingActionButton(
      onPressed: () {
+++     choosePic(ImageSource.camera);
      },
      tooltip: 'takephoto',
      child: Icon(Icons.photo_camera),
    ),
    FloatingActionButton(
      onPressed: () {
+++     choosePic(ImageSource.gallery);
      },
      tooltip: 'takepicture',
      child: Icon(Icons.photo_library),
    )
  ],
)
```

## 4. 把用户选择的照片渲染到页面

1. 将 `Scaffold` 控件的 `body` 参数，修改成 `renderBody()` 函数的调用，通过 `renderBody()` 函数，返回被渲染的页面结构，具体代码如下：

```dart
@override
Widget build(BuildContext context) {
  return Scaffold(
    // 头部 AppBar 区域
    appBar: AppBar(),
    // 中间页面主体区域
    body: renderBody(),
    // 浮动按钮区域
    floatingActionButton: ButtonBar()
  )
}
```

1. 定义 `renderBody()` 函数如下：

```dart
// 渲染页面主体区域
Widget renderBody() {
  // 如果用户没有选择任何图片，则只渲染文本
  if (_image == null) {
    return Center(
      child: Text('暂无图片！'),
    );
  }
  // 在页面上渲染对应的图片
  return Stack(
    children: <Widget>[
      Image.file(
        _image, // 被渲染的图片
        height: double.infinity, // 图片高度撑满整个页面的高度
        fit: BoxFit.cover, // 图片的填充模式
      )
    ]);
}
```

## 5. 申请百度 AI 开放平台账号并创建人脸识别的应用

1. 浏览器访问 `http://ai.baidu.com/` 后，注册百度 AI 开放平台账号
2. 登录账号，并进入`控制台`，在左侧菜单中选择 `人脸识别` 后，点击 `创建应用` 按钮
3. 填写对应的应用信息后，点击 `立即创建` 按钮，最终获取到对应的 `API Key` 和 `Secret Key`

## 6. 鉴权认证机制

如果要成功调用百度 AI 的接口，必须先通过百度的鉴权认证。百度的鉴权认证非常简单，只要能够成功获取到 `Access Token`，就可以拿着百度颁发给我们的 `Access Token` 访问对应的 AI 接口。百度 AI 鉴权的文档地址为 [https://ai.baidu.com/docs#/Auth/top](https://ai.baidu.com/docs#/Auth/top)

## 7. 通过 dio 发起网络数据请求

插件地址 https://pub.dev/packages/dio ，使用步骤如下：

1. 在 `pubspec.yaml` 的 `dependencies` 节点中，新增插件如下：

```yaml
dependencies:
dio: ^3.0.10
```

1. 在 `lib/main.dart` 头部，引入 dio，并创建实例对象：

```dart
import 'package:dio/dio.dart';
Dio dio = new Dio();
```

1. 通过 `dio.post()` 发起 `post` 请求，代码格式如下：

```dart
// 通过 async 和 await 简化异步 API 调用方式
void getHttp() async {
  // 发起 post 请求
  // 参数1：请求的URL地址【必选】
  // 参数2：通过请求体发送的数据【可选】
  // 参数3：请求配置项【可选】
  var response = await dio.post("请求地址", data: {/* body请求体 */}, options: new Options());

  // 打印服务器返回的数据
  print(response.data);
}
```

## 8. Toast 提示

1. 在 `pubspec.yaml` 的 `dependencies` 节点中，新增插件如下：

```yaml
dependencies:
  toast: ^0.1.3
```

1. 在 `lib/main.dart` 中导入对应的插件：

```dart
import 'package:toast/toast.dart';
```

1. 调用 `Toast.show()` 函数提示消息：

```dart
// 参数1：提示消息
// 参数2：提示消息多久后自动隐藏
// 参数3：位置
Toast.show("鉴权失败！", context, duration: Toast.LENGTH_LONG, gravity: Toast.CENTER);
```

## 9. 图片转 base64 字符串

在调用测颜值的 API 期间，需要先把图片转为 base64 的字符串，转换过程如下：

```dart
// 将照片转换为字节数组
var imageBytes = await image.readAsBytes();
// 将字节数组转换为 base64 格式的字符串
var imageBase64 = base64Encode(imageBytes);
```

## 10. 为 dio 的 post 请求设置 data 和 options

在发送 post 请求期间，如果需要设置 `body` 请求体和 `options` 配置项，可以参考如下代码：

```dart
// 请求的URL地址
var testFaceURL = 'https://aip.baidubce.com/rest/2.0/face/v3/detect?access_token=' + accessResult.data['access_token'];

// 发起请求
var testFaceResult = await dio.post(testFaceURL,
    // 发送到后台的 body 数据
    data: {
      'image': imageBase64,
      'image_type': 'BASE64',
      // face_field 是要获取的人脸信息字段，
      // 年龄，性别，颜值，表情，眼镜，情绪
      'face_field': 'age,gender,beauty,expression,glasses,emotion'
    },
    // 请求配置
      options: new Options(responseType: ResponseType.json));
```

## 11. 渲染人脸信息

1. 修改 `renderBody()` 函数，在 `Stack` 控件中，通过调用 `renderFaceInfo()` 函数，渲染人脸信息区域：

```dart
// 渲染页面主体区域
Widget renderBody() {
  // 如果用户没有选择任何图片，则只渲染文本
  if (_image == null) {
    return Center(
      child: Text('暂无图片！'),
    );
  }
  // 在页面上渲染对应的图片
  return Stack(
    children: <Widget>[
      Image.file(
        _image,
        height: double.infinity,
        fit: BoxFit.cover,
      ),
      // 渲染人脸信息区域
+++   renderFaceInfo()
    ]);
}
```

1. 定义 `renderFaceInfo()` 函数如下：

```dart
  // 渲染识别出来的人脸信息
  Widget renderFaceInfo() {
    // 如果人脸信息为空，则渲染空字符串
    if (faceInfo == null) {
      return Text('');
    }
    // 如果人脸信息不为空，则渲染人脸信息区域
    return Center(
      // 渲染矩形盒子区域
      child: Container(
        decoration: BoxDecoration(
            // 背景颜色【半透明的白色】
            color: Colors.white54,
            // 圆角
            borderRadius: BorderRadius.all(Radius.circular(5))),
        // 盒子的宽度
        width: 300,
        // 盒子的高度
        height: 200,
        // 【列组件】
        child: Column(
          // 子元素在纵轴上分散对齐
          mainAxisAlignment: MainAxisAlignment.spaceAround,
          // 在列组件中，渲染多个【行组件】
          children: <Widget>[
            Row(
              // 子元素在横轴上分散对齐
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: <Widget>[
                Text('年龄：${faceInfo['age']}岁'),
                Text('性别：' + genderMap[faceInfo['gender']['type']]),
              ],
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: <Widget>[
                Text('颜值：${faceInfo['beauty']}分'),
                Text('表情：' + expressionMap[faceInfo['expression']['type']]),
              ],
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: <Widget>[
                Text('眼镜：' + glassesMap[faceInfo['glasses']['type']]),
                Text('情绪：' + emotionMap[faceInfo['emotion']['type']]),
              ],
            )
          ],
        ),
      ),
    );
  }
```

1. 在 `_MyHomePageState` 状态管理类中，定义 `Map` 映射，辅助渲染人脸信息：

```dart
// 性别
Map genderMap = {'male': '男', 'female': '女'};
// 表情
Map expressionMap = {'none': '不笑', 'smile': '微笑', 'laugh': '大笑'};
// 眼镜
Map glassesMap = {'none': '无眼镜', 'common': '普通眼镜', 'sun': '墨镜'};
// 情绪
Map emotionMap = {
  'angry': '愤怒',
  'disgust': '厌恶',
  'fear': '恐惧',
  'happy': '高兴',
  'sad': '伤心',
  'surprise': '惊讶',
  'neutral': '无情绪'
};
```

## 12. 实现 loading 效果

1. 在 `_MyHomePageState` 状态管理类中，定义 `isloading` 数据如下：

```dart
class _MyHomePageState extends State<MyHomePage> {
  // false 为不显示 loading
  // true 为显示 loading
  bool isloading = false;
}
```

1. 修改 `getFaceInfo()` 函数如下，在适当的时机重置 `isloading` 的值：

```dart
// 发起请求，获取人脸信息
void getFaceInfo(image) async {
  // 只要调用这个函数，就立即展示 loading 效果
  setState(() {
    isloading = true;
  });

  // 鉴权
  // ... 省略不必要的代码

  // 鉴权失败
  if (accessResult.data['access_token'] == null) {
    // 鉴权失败，隐藏 loading 效果
    setState(() {
      isloading = false;
    });
    // ... 省略不必要的代码
    return;
  }

  // 鉴权成功
  // 检测人脸信息
  // ... 省略不必要的代码

  // 检测失败
  if (testFaceResult.data['error_msg'] != 'SUCCESS' ||testFaceResult.data['result']['face_num'] <= 0) {
    // 检测失败，隐藏 loading 效果
    setState(() {
      isloading = false;
    });
    // ... 省略不必要的代码
    return;
  }

  // 检测成功，隐藏 loading 效果
  setState(() {
    faceInfo = testFaceResult.data['result']['face_list'][0];
    isloading = false;
  });
}
```

1. 修改 `renderFaceInfo()` 函数，按需渲染 loading 效果：

```dart
// 渲染识别出来的人脸信息
Widget renderFaceInfo() {
  if (faceInfo == null) {
++  //如果 isloading 为 true，就在页面正中央渲染 loading 效果
++  if (isloading) {
++    return Center(child: CircularProgressIndicator());
++  }
    // 否则，直接渲染空字符串
    return Text('');
  }

  // ... 省略不必要的代码
}
```

```dart
// 导入依赖项
import 'package:flutter/material.dart';
import 'package:image_picker/image_picker.dart';
import 'dart:io';
import 'dart:convert';
import 'package:dio/dio.dart';
import 'package:toast/toast.dart';

Dio dio = new Dio();

class MyFacePage extends StatefulWidget {
  MyFacePage({
    Key key,
  }) : super(key: key);
  @override
  _MyFacePageState createState() => _MyFacePageState();
}

class _MyFacePageState extends State<MyFacePage> {
  // 用户通过摄像头或图片库选择的照片
  File _image;
  var faceInfo;
  bool isloading = false;
  Map genderMap = {'male': '男', 'female': '女'};
  Map expressionMap = {'none': '不笑', 'smile': '微笑', 'laugh': '大笑'};
  Map glassesMap = {'none': '无眼镜', 'common': '普通眼镜', 'sun': '墨镜'};
  Map emotionMap = {
    'angry': '愤怒',
    'disgust': '厌恶',
    'fear': '恐惧',
    'happy': '高兴',
    'sad': '伤心',
    'surprise': '惊讶',
    'neutral': '无情绪'
  };

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // 头部 AppBar 区域
      appBar: AppBar(
        title: Text(
          "人脸识别",
          style: TextStyle(fontSize: 16, fontWeight: FontWeight.bold),
        ),
        centerTitle: true,
      ),
      // 中间页面主体区域
      body: renderBody(),
      // 底部浮动按钮区域
      floatingActionButton: ButtonBar(
        alignment: MainAxisAlignment.spaceAround,
        children: <Widget>[
          FloatingActionButton(
            onPressed: () {
              choosePic(ImageSource.camera);
            },
            tooltip: 'takephoto',
            child: Icon(Icons.photo_camera),
          ),
          FloatingActionButton(
            onPressed: () {
              choosePic(ImageSource.gallery);
            },
            tooltip: 'takepicture',
            child: Icon(Icons.photo_library),
          )
        ],
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }

  // 渲染页面主体区域
  Widget renderBody() {
    // 如果用户没有选择任何图片，则只渲染文本
    if (_image == null) {
      return Center(
        child: Text('暂无图片！'),
      );
    }
    // 在页面上渲染对应的图片
    return Stack(
      children: <Widget>[
        Image.file(
          _image,
          height: double.infinity,
          fit: BoxFit.cover,
        ),
        renderFaceInfo()
      ],
    );
  }

  // 渲染识别出来的人脸信息
  Widget renderFaceInfo() {
    if (faceInfo == null) {
      if (isloading) {
        return Center(child: CircularProgressIndicator());
      }
      return Text('');
    }
    return Center(
      child: Container(
        decoration: BoxDecoration(
            // 背景颜色
            color: Colors.white54,
            // 圆角
            borderRadius: BorderRadius.all(Radius.circular(5))),
        width: 300,
        height: 200,
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceAround,
          children: <Widget>[
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: <Widget>[
                Text('年龄：${faceInfo['age']}岁'),
                Text('性别：' + genderMap[faceInfo['gender']['type']]),
              ],
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: <Widget>[
                Text('颜值：${faceInfo['beauty']}分'),
                Text('表情：' + expressionMap[faceInfo['expression']['type']]),
              ],
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceAround,
              children: <Widget>[
                Text('眼镜：' + glassesMap[faceInfo['glasses']['type']]),
                Text('情绪：' + emotionMap[faceInfo['emotion']['type']]),
              ],
            )
          ],
        ),
      ),
    );
  }

  // 点击按钮，选择图片
  void choosePic(source) async {
    // 得到选取的照片
    var image = await ImagePicker.pickImage(source: source);

    setState(() {
      _image = image;
      faceInfo = null;
    });

    // 如果选取的照片为空，则不执行后续人脸检测的业务逻辑
    if (image == null) {
      return;
    }

    // 调用获取人脸信息的函数
    getFaceInfo(image);
  }

  // 发起请求，获取人脸信息
  void getFaceInfo(image) async {
    setState(() {
      isloading = true;
    });
    // 将照片转换为字节数组
    var imageBytes = await image.readAsBytes();
    // 将字节数组转换为 base64 格式的字符串
    var imageBase64 = base64Encode(imageBytes);

    // 人工智能API接口鉴权
    var accessURL =
        "https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id=o631j8eLGBQq88uj8IXsVeGO&client_secret=Pses7CgBpF5Hf9owM6N8v8iX6iLAP7G5";
    var accessResult = await dio.post(accessURL);
    // 判断是否获取到了access_token
    if (accessResult.data['access_token'] == null) {
      setState(() {
        isloading = false;
      });
      Toast.show("鉴权失败！", context,
          duration: Toast.LENGTH_LONG, gravity: Toast.CENTER);
      return;
    }

    // 发起请求，获取检测结果
    var testFaceURL =
        'https://aip.baidubce.com/rest/2.0/face/v3/detect?access_token=' +
            accessResult.data['access_token'];
    var testFaceResult = await dio.post(testFaceURL,
        data: {
          'image': imageBase64,
          'image_type': 'BASE64',
          'face_field': 'age,gender,beauty,expression,glasses,emotion'
        },
        options: new Options(responseType: ResponseType.json));
    // print(testFaceResult.data);

    // 识别失败
    if (testFaceResult.data['error_msg'] != 'SUCCESS' ||
        testFaceResult.data['result']['face_num'] <= 0) {
      setState(() {
        isloading = false;
      });
      Toast.show("人脸识别失败！", context,
          duration: Toast.LENGTH_LONG, gravity: Toast.CENTER);
      return;
    }

    // 识别成功
    setState(() {
      faceInfo = testFaceResult.data['result']['face_list'][0];
      isloading = false;
    });
    print(faceInfo);
  }
}

```

