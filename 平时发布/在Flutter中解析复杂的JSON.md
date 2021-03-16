[TOC]

趁着年假，今天在网上看到一篇不错的讲解在Flutter中解析复杂的JSON的文章，于是花了两个小时，翻译，研究，觉得对我收益挺大。分享给大家，欢迎大家点赞转发。，以下为译文，欢迎大家提出疑惑。



我不得不承认，在Flutter / Dart中使用JSON后，我错过了Android的***gson\*** 世界。当我开始在Flutter中使用API时，JSON解析确实使我非常费力。而且我敢肯定，这会让很多初学者感到困惑。

我们将使用`dart:convert`此博客的内置库。这是最基本的解析方法，仅在从Flutter开始或正在构建小型项目时才建议使用。尽管如此，了解Flutter中JSON解析的基础还是很重要的。如果您擅长于此，或者需要处理较大的项目，请考虑使用[json_serializable](https://pub.dartlang.org/packages/json_serializable)等代码生成器库。以及其他的方法，比如 JsontoDart如果可能，我将在以后的文章中找到它们。

Fork this [sample project](https://github.com/PoojaB26/ParsingJSON-Flutter). 它包含您可以尝试的此博客的所有代码。

# JSON结构＃1：简单 map

让我们从[student.json中](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/assets/student.json)的简单JSON结构开始

```
{
  "id":"487349",
  "name":"Pooja Bhaumik",
  "score" : 1000
}
```

**规则1：** **确定结构。Json字符串将具有一个Map（键-值对）或一个Map列表。**

**规则2：从大括号开始？这是一**map.
**以方括号开头？**That’s a List of maps.**

`student.json`显然是map. ( E.g like, `id` is a key, and `487349` is the value for `id`)

让我们为此json结构制作一个PODO（普通的旧式Dart对象？）文件。您可以在示例项目的[student_model.dart](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/lib/model/student_model.dart)中找到此代码。

```
class Student{
  String studentId;
  String studentName;
  int studentScores;

  Student({
    this.studentId,
    this.studentName,
    this.studentScores
 });}
```

完美的！
*是吗 因为json映射与此PODO文件之间没有映射。甚至实体名称都不匹配。
*我知道我知道。我们还没有完成。我们必须完成将这些类成员映射到json对象的工作。为此，我们需要创建一个`factory `方法。根据Dart文档，我们`factory`在实现构造器时使用了关键字，该构造器并不总是创建其类的新实例，而这正是我们现在所需要的。

```
factory Student.fromJson(Map<String, dynamic> parsedJson){
    return Student(
      studentId: parsedJson['id'],
      studentName : parsedJson['name'],
      studentScores : parsedJson ['score']
    );
  }
```

在这里，我们正在创建一个工厂方法`Student.fromJson`，该方法的目标是简单地反序列化您的json。

***我有点菜鸟，您能告诉我有关反序列化的信息吗？\***

当然。首先让我们介绍一下序列化和反序列化。**序列化**只是意味着将数据（可能在对象中）写入字符串，而**反序列化**则相反。它获取原始数据并重建对象模型。在本文中，我们主要将处理反序列化部分。在第一部分中，我们将从中反序列化json字符串`student.json`

So our factory method could be called as our converter method.

还必须注意`fromJson`方法中的参数。这是一个`Map<String, dynamic>`意思是它映射一个`String` **键**和一个`dynamic` **值**。这正是我们需要确定结构的原因。 If this json structure were a List of maps, 则此参数将有所不同

***但是为什么要动态？\****
*让我们先看看另一个json结构来回答您的问题

![Image for post](https://miro.medium.com/max/234/1*aYehHPUoXS4S-CVLWg1NCQ.png)

`name` is a Map<String, String> ,`majors` is a Map of String and List<String> and `subjects` is a Map of String and List<Object>

由于密钥始终是 `string` 并且值可以是任何类型，因此我们将其保持`dynamic`安全。

Check the full code for `student_model.dart` [here](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/lib/model/student_model.dart).

# 访问对象

让我们来编写`student_services.dart`其中的代码，该代码可以调用`Student.fromJson`并从`Student`对象中检索值

## Snippet #1 : imports

```
import 'dart:async' show Future;
import 'package:flutter/services.dart' show rootBundle;
import 'dart:convert';
import 'package:flutter_json/student_model.dart';
```

最后的导入将是模型文件的名称

## **Snippet #2 : **加载Json Asset（可选）**

```
Future<String> _loadAStudentAsset() async {
  return await rootBundle.loadString('assets/student.json');
}
```

在这个特定的项目中，我们的json文件位于assets文件夹中，因此我们必须以这种方式加载json。但是，如果您将json文件存储在云中，则可以进行网络调用。*网络调用不在本文讨论范围之内。*

## Snippet #3 : 加载响应

```
Future loadStudent() async {
  String jsonString = await _loadAStudentAsset();
  final jsonResponse = json.decode(jsonString);
  Student student = new Student.fromJson(jsonResponse);
  print(student.studentScores);
}
```

在此`loadStudent()`方法中，
**Line 1** : 加载原始json字符串。from the assets.
**Line 2** : 解码我们得到的原始json字符串
**Line 3** : 现在，我们通过调用`Student.fromJson`方法来反序列化解码的json响应，以便我们现在可以使用`Student`object访问实体
**Line 4** : 就像我们在这里所做的那样，where we printed `studentScores` from `Student` class.

*检查Flutter控制台以查看所有打印值。*(In Android Studio, its under Run tab)*

瞧！您只是进行了第一个JSON解析（或没有）。
*注意：请记住这里的3个代码段，我们将使用它来进行下一组json解析（仅更改文件名和方法名称），在此我将不再重复代码。但是您仍然可以在示例项目中找到所有内容。*

# JSON结构＃2：具有数组的简单结构

现在，我们征服了一种与上述结构相似的json结构，但它可能不仅具有单个值，而且还可能have an array of values.

```
{
  "city": "Mumbai",
  "streets": [
    "address1",
    "address2"
  ]
}
```

So in this [address.json](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/assets/address.json), we have `city` entity that has a simple `String` value, but `streets` is an array of `String`.
据我所知，Dart没有数组数据类型，但是有 a List<datatype> 所以这里`streets`是一个`List<String>`。

现在我们必须检查**Rule＃1和Rule＃2**。这绝对是一  map 因为它以花括号开头。. `streets` is still a `List` though, 但我们稍后会担心。

所以`address_model.dart`最初看起来像这样

```
class Address {
  final String city;
  final List<String> streets;

  Address({
    this.city,
    this.streets
  });
}
```

现在，由于这是一amap, 因此我们的`Address.fromJson`方法将仍然具有一个`Map<String, dynamic>`参数。

```
factory Address.fromJson(Map<String, dynamic> parsedJson) {
  
  return new Address(
      city: parsedJson['city'],
      streets: parsedJson['streets'],
  );
}
```

现在，`address_services.dart`通过添加我们上面提到的3个片段来构建。*必须记住要放置正确的文件名和方法名。示例项目已经*`*address_services.dart*`*为您构建。*

现在，当您运行此命令时，您将得到一个不错的小错误。：/

```
type 'List<dynamic>' is not a subtype of type 'List<String>'
```

我告诉你，这些错误几乎都发生在我使用Dart开发的每个步骤中。而且您也将拥有它们。因此，让我解释一下这意味着什么。我们正在请求一个，`List<String>`但是却收到一个，`List<dynamic>`因为我们的应用程序尚无法识别类型。

因此，我们必须将其明确转换为 `List<String>`

```
var streetsFromJson = parsedJson['streets'];
List<String> streetsList = new List<String>.from(streetsFromJson);
```

在这里，首先我们将变量映射`streetsFromJson`到`streets`实体。`streetsFromJson`还是一个`List<dynamic>`。现在，我们明确地创建一个新的`List<String> streetsList`包含所有元素**的**`streetsFromJson`。

在[此处](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/lib/model/address_model.dart)检查更新的方法。*现在注意return语句。
现在，您可以运行此程序，*`*address_services.dart*`*它将完美运行。*

# Json结构＃3：简单嵌套结构

现在，如果我们有一个来自[shape.json](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/assets/shape.json)的嵌套结构，该[怎么办](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/assets/shape.json)

```
{
  "shape_name":"rectangle",
  "property":{
    "width":5.0,
    "breadth":10.0
  }
}
```

在这里，`property`包含一个对象而不是基本的原始数据类型。
*那么PODO会是什么样子？*

好吧，让我们分解一下。In our `shape_model.dart` , let’s make a class for `Property` first.

```dart
class Property{
  double width;
  double breadth;

  Property({
    this.width,
    this.breadth
});
}
```

现在，我们为构建类`Shape`。*我将两个类都放在同一个Dart文件中。*

```dart
class Shape{
  String shapeName;
  Property property;

  Shape({
    this.shapeName,
    this.property
  });
}
```

请注意，第二个数据成员`property` is basically an object of是我 们上一类的对象 `Property`.

**规则3：对于嵌套结构，请首先创建类和构造函数，然后从底层添加工厂方法**

*所谓底层，是指首先征服*`*Property*`*阶级，然后再上一层*`*Shape*`*。

*By bottom level, we mean, first we conquer* `*Property*` *class, and then we go one level above to the* `*Shape*` *class. 这只是我的建议，而不是Flutter规则。*

```
factory Property.fromJson(Map<String, dynamic> json){
  return Property(
    width: json['width'],
    breadth: json['breadth']
  );
}
```

*这是一* simple map.*

但是对于我们 factory method at `Shape` class, 我们不能仅仅这样做

```
factory Shape.fromJson(Map<String, dynamic> parsedJson){
  return Shape(
    shapeName: parsedJson['shape_name'],
    property : parsedJson['property']
  );
}
```

`property : parsedJson['property']` 首先，这将引发类型不匹配错误-

```
type '_InternalLinkedHashMap<String, dynamic>' is not a subtype of type 'Property'
```

And second, *hey we just *制作了一个不错的 little class for Property, *我看不到它在任何地方都有用。*

Right.我们必须在此处映射我们的Property类。

```
factory Shape.fromJson(Map<String, dynamic> parsedJson){
  return Shape(
    shapeName: parsedJson['shape_name'],
    property: Property.fromJson(parsedJson['property'])
  );
}
```

因此，基本上，我们`Property.fromJson`从`Property`类中调用该方法，作为返回，we map it to the `property` entity. Simple! Check out the code [here](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/lib/model/shape_model.dart).

Run this with your `shape_services.dart` and you are good to go.

# JSON结构＃4：具有列表的嵌套结构

*让我们检查一下*[*product.json*](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/assets/product.json)

```
{
  "id":1,
  "name":"ProductName",
  "images":[
    {
      "id":11,
      "imageName":"xCh-rhy"
    },
    {
      "id":31,
      "imageName":"fjs-eun"
    }
  ]
}
```

好的，现在我们越来越深入。*我在内部某处看到对象列表。哇。* Woah.*

是的，因此此结构具有一个对象列表，但其本身仍是 a map. (请参阅**规则1**和**规则2**）。现在参考**规则3**，让我们构造我们的`product_model.dart`。

所以我们创建了两个新类`Product`和`Image`。
*Note:* `*Product*` *will *将具有一个数据成员，该成员是* List of* `*Image*`

```
class Product {
  final int id;
  final String name;
  final List<Image> images;

  Product({this.id, this.name, this.images});
}

class Image {
  final int imageId;
  final String imageName;

  Image({this.imageId, this.imageName});
}
```

The factory method for `Image` will be非常简单和基本。

```
factory Image.fromJson(Map<String, dynamic> parsedJson){
 return Image(
   imageId:parsedJson['id'],
   imageName:parsedJson['imageName']
 );
}
```

Now for the factory method for `Product`

```
factory Product.fromJson(Map<String, dynamic> parsedJson){

  return Product(
    id: parsedJson['id'],
    name: parsedJson['name'],
    images: parsedJson['images']
  );
}
```

这显然会引发运行时错误

```
type 'List<dynamic>' is not a subtype of type 'List<Image>'
```

如果我们这样做，

```
images: Image.fromJson(parsedJson['images'])
```

这也绝对是错误的，它会立即引发错误，因为您无法将`Image`对象分配给`List<Image>`

因此，我们必须创建一个`List<Image>`，然后将其分配给`images`

```
var list = parsedJson['images'] as List;
print(list.runtimeType); //returns List<dynamic>List<Image> imagesList = list.map((i) => Image.fromJson(i)).toList();
```

`list`这是一个List <dynamic>。现在，我们遍历列表，每个对象映射`list`到`Image`调用`Image.fromJson`，（and map each object in `list` to `Image` by calling `Image.fromJson` ）and then we把每个map object into 一个新的列表with `toList()` and 存储in `List<Image> imagesList`. Find the full code [here](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/lib/model/product_model.dart).

# JSON结构＃5: List of maps

现在让我们转到[photo.json](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/assets/photo.json)

```
[
  {
    "albumId": 1,
    "id": 1,
    "title": "accusamus beatae ad facilis cum similique qui sunt",
    "url": "http://placehold.it/600/92c952",
    "thumbnailUrl": "http://placehold.it/150/92c952"
  },
  {
    "albumId": 1,
    "id": 2,
    "title": "reprehenderit est deserunt velit ipsam",
    "url": "http://placehold.it/600/771796",
    "thumbnailUrl": "http://placehold.it/150/771796"
  },
  {
    "albumId": 1,
    "id": 3,
    "title": "officia porro iure quia iusto qui ipsa ut modi",
    "url": "http://placehold.it/600/24f355",
    "thumbnailUrl": "http://placehold.it/150/24f355"
  }
]
```

哦，哦。**规则1和规则2**告诉我这不可能是 a map 因为json字符串以方括号开头*这是对象列表吗？*是的 The object being here is `Photo` (或任何您想调用的对象).

```
class Photo{
  final String id;
  final String title;
  final String url;

  Photo({
    this.id,
    this.url,
    this.title
}) ;

  factory Photo.fromJson(Map<String, dynamic> json){
    return new Photo(
      id: json['id'].toString(),
      title: json['title'],
      url: json['json'],
    );
  }
}
```

*但是它的列表*`*Photo*`*，这是否意味着您必须构建一个包含的类*`*List<Photo>*`*？*

是的，我建议。

```
class PhotosList {
  final List<Photo> photos;

  PhotosList({
    this.photos,
  });
}
```

另请注意，此json字符串是List of maps. 。因此，在我们的工厂方法中，我们没有`Map<String, dynamic>`参数，因为它是一个列表。这就是为什么首先确定结构很重要的原因。因此，我们的新参数将为`List<dynamic>`。

```
factory PhotosList.fromJson(List<dynamic> parsedJson) {

    List<Photo> photos = new List<Photo>();

    return new PhotosList(
       photos: photos,
    );
  }
```

这会引发错误

```
Invalid value: Valid value range is empty: 0
```

嘿，因为我们永远无法使用该`Photo.fromJson`方法。
如果在列表初始化后添加此行代码怎么办？

```
photos = parsedJson.map((i)=>Photo.fromJson(i)).toList();
```

与之前的概念相同，我们不必将其映射到json字符串中的任何键，因为它是列表，而不是映射。Code [here](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/lib/model/photo_model.dart).

# JSON结构＃6：复杂的嵌套结构

Here is [page.json](https://github.com/PoojaB26/ParsingJSON-Flutter/blob/master/assets/page.json).

我将要求您解决此问题。它已经包含在示例项目中。您只需要为此构建模型和服务文件。但是在给您提示之前，我不会总结（如果需要，则需要任何提示）。

照常应用**规则1**和**规则2**。首先确定结构。这是一 a map. 因此，从1-5开始的所有json结构都会有所帮助。

**规则3**要求您首先创建类和构造函数，然后从最底层添加工厂方法。Just another tip. Also add the classes from the deep/bottom level. For e.g, for this json structure, make the class for `Image` first, then `Data` and `Author` and then the main class `Page`. 并按相同顺序添加工厂方法。

对于类`Image`，`Data`请参考**Json结构＃4**。
有关类，`Author`请参阅**Json结构＃3**

*初学者提示：在尝试任何新资产时，请记住在pubspec.yaml文件中进行声明。*

这篇Fluttery文章就是这样。本文可能不是那里最好的JSON解析文章，（因为我还在学习很多东西），但我希望它能帮助您入门。



如果您学到了一两件事，请拍拍手👏尽可能多地表示支持！这激励着我写更多的东西。

[参考](https://medium.com/flutter-community/parsing-complex-json-in-flutter-747c46655f51)