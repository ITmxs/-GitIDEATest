int -> string

```
age.toString()
```

string -> int 

```
int.parse('100');
```

String -> double

```
var onePointOne = ``double``.parse(``'1.1'``);
```

　

double->String

```
String piStr = 3.141592.toStringAsFixed(3); //结果为3.141
```

```
	var a = int.parse('1234');         //把字符串 1234 转换成 数值 1234
	print(a is int);                   //判断是否转换成功
	//输出 ture 
	
	var b = double.parse('1234.12');  //把字符串 1234.12 转换成 数值 1234.12
	print(b is double);               //判断是否转换成功
	//输出 ture 
	
	var str = 1234.toString();        //把数值 1234 转换成 字符串 1234
	print(str  is String);            //判断是否转换成功
	//输出 ture 
```

![刷](https://luckly007.oss-cn-beijing.aliyuncs.com/images/%E5%88%B7.jpg)