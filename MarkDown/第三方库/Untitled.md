# audioplayers

https://www.jianshu.com/p/288f869690f0

是因为我用的本地音频，所以AudioPlayer播发不了，它只能播放非本地的，本地的用AudioCache就可以了

shake_animation_widget

share

# Sharedpreferences 

https://www.jianshu.com/p/1158d1128084?utm_campaign=haruki

今天主要讲的内容是其他博主都讲到的  Sharedpreferences  的基础用法 以及  其他博主没有讲到的Sharedpreferences  使用技巧：

#### Sharedpreferences    基本用法

存储基本数据类型：
 int 类型



```csharp
  onPressed: ()async{
                SharedPreferences prefs = await SharedPreferences.getInstance();
                int counter =  1;
                await prefs.setInt('counter', counter);
              },
```

String类型



```dart
  onPressed: ()async{
                SharedPreferences prefs = await SharedPreferences.getInstance();
                String  counter =  "1";
                await prefs.setString('counter', counter);
              },
```

bool类型



```csharp
  onPressed: ()async{
                SharedPreferences prefs = await SharedPreferences.getInstance();
                bool counter =false;
                await prefs.setBool('counter', counter);
              },
```

double类型



```csharp
  onPressed: ()async{
                SharedPreferences prefs = await SharedPreferences.getInstance();
                double counter =0.01;
                await prefs.setDouble('counter', counter);
              },
```

list<String>data类型



```dart
   onPressed: ()async{
                SharedPreferences prefs = await SharedPreferences.getInstance();
                List<String>counter=["1","2"];
                await prefs.setStringList('counter', counter);
              },
```

#### 取值基本用法



```dart
 onPressed: ()async{
                SharedPreferences prefs = await SharedPreferences.getInstance();
                 int  counterint =prefs.getInt("counter");
                 String  counter =prefs.getString("counter");
                 bool  counterbool =prefs.getBool("counter");
                double  counterdouble =prefs.getDouble("counter");
                List  counterlist =prefs.getStringList("counter");
              },
```

### 删除指定数据

其中key就是你存贮的名称，value就是你存储的值



```csharp
 SharedPreferences prefs = await SharedPreferences.getInstance();
  prefs.remove(key); //删除指定键
```

### 清空整个缓存：



```csharp
 SharedPreferences prefs = await SharedPreferences.getInstance();
 prefs.clear();//清空键值对
```

以上是Sharedpreferences   的基础用法 ，但是我们发现没有每次写一大推重复代码 这时候我们就对Sharedpreferences   进行简单封装是我们减少重复代码的编写



```csharp
  /***
   *
   * 存数据
   */

  static Object savePreference(BuildContext context , String key , Object value) async {
   SharedPreferences prefs = await SharedPreferences.getInstance();
   if(value is  int ){
     await prefs.setInt(key, value);
     }else if(value is double){
     await prefs.setDouble(key, value);
     }else if(value is bool){
     await prefs.setBool(key, value);
     }else if(value is String){
     await prefs.setString(key, value);
     }else if(value is List){
     await prefs.setStringList(key, value);
   }  else {
     throw new Exception("不能得到这种类型");
   }
 }
  /***
   * 取数据
   *
   */
     static Future  getPreference( Object context , String key ,Object defaultValue) async{
   SharedPreferences prefs = await SharedPreferences.getInstance();
   if(defaultValue is  int) {
     return prefs.getInt(key);
      }
   else if(defaultValue is  double) {
     return prefs.getDouble(key);
      }
   else if(defaultValue is bool) {
     return prefs.getBool(key);
     }
   else if(defaultValue is String) {
     return prefs.getString(key);
   }
   else if(defaultValue is List) {
     return prefs.getStringList(key);
     }
   else {
     throw new Exception("不能得到这种类型");
   }
 }
  /***
   * 删除指定数据
   */
  static void    remove(String key)async{
     SharedPreferences prefs = await SharedPreferences.getInstance();
     prefs.remove(key); //删除指定键
   }
  /***
   * 清空整个缓存
   */
  static void    clear()async{
    SharedPreferences prefs = await SharedPreferences.getInstance();
    prefs.clear(); ////清空缓存
  }
```

### 工具类具体调用 ：

##### 存储数据：



```dart
  onPressed: (){
                String counter  = "1";
                SharedPreferencesUtils.savePreference(context, "counter", counter);
              },
            ),
```

##### 取值

直接调用并赋值给定义的变量



```dart
 onPressed: ()async{
      String  counter = await (SharedPreferencesUtils.getPreference(context, "counter", "1")) as String ;
                 print("counter  -- > "+counter);
     },
```

通过then方式调用



```csharp
 onPressed: ()async{
                 SharedPreferencesUtils.getPreference(context, "counter", "1").then((value){
                   print("value   --->"  +value);
                 });
              },
```

#### 删除指定key的缓存数据调用：



```csharp
   SharedPreferencesUtils.remove("counter");
```

#### 清空整个SharedPreferences缓存：



```css
   SharedPreferencesUtils.clear();
```

基本数据类型的封装使用我们就说完了 大家发现没有有时候我们需要存一个model其实
 SharedPreferences 本身是不支持我们要怎么做

我们需要把用户名和密码起来保存 我们用传统 SharedPreferences  来做肯定不OK  当然有同学肯定想到说用  sqlite 本地数据库来实现 ，sqlite  确实可以实现 但是本身代码就多不够灵活 而且我们这边存储的字段并不多 我们这边选择在  SharedPreferences  上面做稍微的改造就能实现上面的需求的

##### 之前的实现方式  ：



```dart
     onPressed: ()async{
                      User user=new User();
                      user.username=_username;
                      user.password=_password;
                      datalsit.add(user);
                       String jsonStringA = jsonEncode(datalsit);
                      print("jsonStringA   --------- >"+ jsonStringA);
                      SharedPreferences prefs = await SharedPreferences.getInstance();
                      prefs.setString("data",jsonStringA);
                    },
```

#### 取值转换



```dart
    onPressed: ()async{
                      SharedPreferences prefs = await SharedPreferences.getInstance();
                      getdata = await prefs.getString("data");
                     List  list= json.decode(getdata);
                    },
```

获取到值以后我们要通过   json.decode 将那倒json字符串转成list然后再来取值，这是我们传统的做法 代码量其实很大 而且如果我们有很多类似的数据要存储肯定做法不够简洁 我们这里也对于之前实现方式简单的封装 代码如下



```dart
  /**
   * 存储  List<Object> phoneList
   *
   * List<Object> phoneList
   */
  static void setSelectBeanList(BuildContext context,List<Object> phoneList, String key) async{
    String jsonStringA = jsonEncode(phoneList);
    print("jsonStringA   --------- >"+ jsonStringA);
    SharedPreferences prefs = await SharedPreferences.getInstance();
    prefs.setString(key,jsonStringA);
  }

  /**
   * 获取  List<Object> phoneList
   *
   * List<Object> phoneList
   */
   static Future   getSelectBean(BuildContext context, String key) async {
     SharedPreferences prefs = await SharedPreferences.getInstance();
     String  getdata = await prefs.getString("data");
      List  list= json.decode(getdata);
     return list;
  }
```

#### 具体调用：

##### 存储model:



```csharp
   onPressed: ()async{
                      User user=new User();
                      user.username=_username;
                      user.password=_password;
                      _userlsit.add(user);
                       SharedPreferencesUtils.setSelectBeanList(context, _userlsit, "data");
                    },
```

##### 取值：



```dart
onPressed: ()async{
     List datalist= await SharedPreferencesUtils.getSelectBean(context, "data");
    },
```

到此整个 SharedPreferences库 数据存储的基础用法和特别技巧我们就讲完了。

## 最后总结：

通过这一期的博客讲解 我希望同学能能够灵活运用  SharedPreferences库来处理实战开发中的各种数据的缓存，以及使用 SharedPreferences的简单封装来简化代码和存储SharedPreferences库本身不支持的数据类型的操作 ，代码相对简单 有兴趣的同学可以下载完整代码来多尝试，最后希望我的文章能帮助到各位解决问题 ，以后我还会贡献更多有用的代码分享给大家。各位同学如果觉得文章还不错 ，麻烦给关注和star，小弟在这里谢过啦。