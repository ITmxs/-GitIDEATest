

[TOC]

Google于2018年12月4日发布了Flutter 1.0，它是一款功能强大的工具，可让您在iOS和Android上创建漂亮的应用程序。与Firebase等其他平台一起，移动应用程序开发现在比以往更加轻松。目前版本1.22.6

用Flutter设计漂亮的应用

在本教程中，我将向您展示如何在Mac计算机上使用Flutter创建您的第一个Hello World应用，我们将在Xcode iOS模拟器上对其进行测试。



# 1.安装Flutter

要将Flutter安装到我们的计算机上，首先我们需要下载[Flutter SDK](https://flutter.cn/docs/get-started/install/macos)。我们还应该创建并重新定位到自定义工作目录，并将下载的*flutter_macos_v1.22.6-stable.zip*文件移动到该目录中。

之后，我们可以解压缩flutter工具并使用在控制台中键入的以下代码来设置环境。

```
unzip flutter_macos_v1.22.6-stable.zip
export PATH="$PATH:`pwd`/flutter/bin"
```

如果一切正常，我们应该可以通过在控制台中输入以下代码来检查依赖关系。

```
flutter doctor
```

![Image for post](https://miro.medium.com/freeze/max/60/1*Uk0daYs9fCgRZwKT5fq9GA.gif?q=20)

![Image for post](https://miro.medium.com/max/618/1*Uk0daYs9fCgRZwKT5fq9GA.gif)

在进行下一步之前，我们还需要配置bash配置文件。bash概要文件可以在名为*.bash_profile*的主目录中找到，使用文本编辑器打开文件并添加以下行。

```
export PATH="$PATH:[PATH_TO_FLUTTER_GIT_DIRECTORY]/flutter/bin"
```

Where *[PATH_TO_FLUTTER_GIT_DIRECTORY]* should be replaced by the name of the working directory we have just created for Flutter.配置完概要文件后，我们可以在主目录中运行以下命令来更新路径。

```
source ~/.bash_profile
```

I如果找不到*.profile*，只需创建一个名称为*.profile*的空文件并将其保存到根目录，然后重试。

要验证路径是否已成功更新，我们可以在控制台中键入以下命令。

```
echo $PATH
```

并且我们应该在return参数中看到包含以下内容的内容。其中*[PATH_TO_FLUTTER_GIT_DIRECTORY]*是我们的工作目录。

```
[PATH_TO_FLUTTER_GIT_DIRECTORY]/flutter/bin
```

# 2.设置iOS模拟器

为了安装了iOS模拟器首先我们要安装的Xcode到我们的计算机，它都可以[在网上下载](https://developer.apple.com/xcode/)或[在app store](https://itunes.apple.com/us/app/xcode/id497799835)。之后，我们可以配置Xcode命令行工具以使用新安装的版本，并在控制台中键入以下命令。

```
sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

然后，我们可以使用以下命令打开iOS模拟器。

```
open -a Simulator
```

![Image for post](https://miro.medium.com/freeze/max/52/1*Q34htDxYW43DeC6d6W7mNg.gif?q=20)

![Image for post](https://miro.medium.com/max/320/1*Q34htDxYW43DeC6d6W7mNg.gif)

这可能需要花费一些时间来加载，但是一旦完成，我们就可以继续部署第一个Flutter应用程序。

还有两件事要注意：

1.通过检查模拟器的“**硬件”>“设备”**菜单中的设置，确保模拟器使用的是64位设备（iPhone 5s或更高版本）。

2.根据开发计算机的屏幕大小，模拟的高屏幕密度iOS设备可能会溢出屏幕。在模拟器的“**窗口”>“比例”**菜单下设置设备比例。

# 3.在Flutter中创建一个Hello World应用

轻而易举地创建应用很简单，只需键入以下命令即可。您可以随意将*hello_world*更改为您喜欢的任何其他名称。Flutter将打印*“全部完成！”* 该过程完成后，请在控制台中输入。

```
flutter create hello_world
```

![Image for post](https://miro.medium.com/freeze/max/60/1*vKN91jLJP0lv3r5EKozdBg.gif?q=20)

![Image for post](https://miro.medium.com/max/618/1*vKN91jLJP0lv3r5EKozdBg.gif)

现在，我们可以使用以下命令转到应用程序目录：

```
cd hello_world
```

然后使用以下命令运行该应用程序：

```
flutter run
```

![Image for post](https://miro.medium.com/freeze/max/60/1*Mep2rENF-UvsQ45IEMRf0A.gif?q=20)

![Image for post](https://miro.medium.com/max/640/1*Mep2rENF-UvsQ45IEMRf0A.gif)

FFlutter将生成一个默认应用程序，如演示中所示，该默认应用程序允许我们在跟踪点击次数的同时单击一个按钮。该应用程序的主要组件由*lib*文件夹中的*main.dart*文件定义。尝试使用以下代码替换*main.dart*文件中的内容。

```
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```

完成后，我们可以通过在控制台中键入“ r”来更新模拟器。而且我们应该看到模拟器已更新为显示“ Hello World”文本，如下所示。

![Image for post](https://miro.medium.com/freeze/max/60/1*uF6VVBKcEsiw3WDbjbBxMQ.gif?q=20)

![Image for post](https://miro.medium.com/max/640/1*uF6VVBKcEsiw3WDbjbBxMQ.gif)

恭喜你！现在，您已经创建了第一个Flutter应用程序，有关Flutter的更多信息，您可以在[官方网站](https://flutter.io/)上找到它们 Flutter primarily utilizes dart and you can find more on its [official website](https://www.dartlang.org/) too.

Have fun 构建移动应用程序

- 将来我可能会写关于Flutter的更多教程。我相信该工具在移动应用程序开发领域具有广阔的前景。以下是我编写的与Flutter和iOS开发相关的指南，我将继续更新列表。
  - [**如何在MacOS上将iOS Flutter应用程序与Firebase集成**](https://medium.freecodecamp.org/how-to-integrate-your-ios-flutter-app-with-firebase-on-macos-6ad08e2714f0)
  - [**如何在iOS设备上测试Flutter iOS应用**](https://medium.com/@shenhuang_21425/how-to-test-your-flutter-ios-app-on-your-ios-device-75924bfd75a8)
- 参考：https://medium.com/@shenhuang_21425/how-to-test-your-flutter-ios-app-on-your-ios-device-75924bfd75a8)

https://poojabhaumik.medium.com/