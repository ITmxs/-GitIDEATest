

[TOC]

[Flutter](https://flutter.io/)是Google提供的开源[移动应用开发](https://appitventures.com/services/mobile-app-development/)SDK，用于通过单个代码库构建漂亮的Native Android和iOS应用。Dart是用于开发Flutter应用程序的语言。

*Flutter现在已退出测试版，*[*Flutter 1.0*](https://developers.googleblog.com/2018/12/flutter-10-googles-portable-ui-toolkit.html)*已于12月4日发布。*

本文介绍了如何安装Flutter和开发环境，还分享了针对Flutter应用程序开发中最常见的要求和问题的解决方案。对于想要尝试Flutter应用程序开发的Android开发人员而言，以下过程更有用。

# #1 — 设置

## 用于开发Flutter应用程序的IDE

- [Android Studio](https://developer.android.com/studio/?gclid=EAIaIQobChMIsrH185KL3wIVxAorCh0t2wCsEAAYASAAEgJjHvD_BwE)
- [Visual Studio Code](https://code.visualstudio.com/download)
- [IntelliJ](https://www.jetbrains.com/idea/download/#section=mac)

##  Flutter安装

- 下载[Flutter Zip](https://flutter.io/docs/get-started/install/macos)文件。
- 解压缩Zip文件。
- 将Flutter工具添加到终端的路径。→当前工作目录。这是一个**临时路径**设置，因此当您重新启动系统时，必须再次设置flutter工具路径

> *注意：要**在MacOS中****永久\****设置****Flutter路径\****并避免在每次重新启动时进行设置，请在MacOS终端中执行以下操作。*

- `touch .bash_profile`
- `open .bash_profile`
- *它会打开一个文件，您需要在下面的命令中复制并粘贴该文件。之后，保存并重新启动系统。*
  *`*export PATH="$PATH:***PWD***/flutter/bin"*`*
  *`***“PWD\*”**` → Present Working Directory.

## 验证Flutter的安装和版本

- `*flutter doctor -v*`

# #2 — Android Studio配置

## 安装Android Studio

要在Android模拟器中运行该应用，您必须安装Android Studio才能获取Android SDK和模拟器。

- 下载[Android Studio](https://developer.android.com/studio/?gclid=EAIaIQobChMIsrH185KL3wIVxAorCh0t2wCsEAAYASAAEgJjHvD_BwE)并运行`.dmg`文件。它将自动安装最新的Android SDK。

## **将Dart插件添加到Android Studio**

如果您希望将Android Studio用作开发Flutter的主要IDE，则必须设置`dart`对Android Studio的语言支持，如下所示。从这里开始，我们将重点关注Android Studio的设置，但您也可以使用  Visual Studio Code or IntelliJ (类似于Android Studio).

- Preferences → Plugins → Browse Repository → type **Dart** in search bar → Install and Restart android studio.
- 首选项→插件→浏览存储库→在搜索栏中键入**Dart** →安装并重新启动android studio。

## **将Flutter插件添加到Android Studio**

- 偏好设置→插件→浏览存储库→在搜索栏中键入**Flutter** →安装并重新启动android studio。

- Preferences → Plugins → Browse Repository → type **Flutter** in search bar → Install and Restart android studio.

## **创建Android模拟器**

中文：

- 工具→AVD管理器→打开一个窗口
- 选择→创建虚拟设备
- 电话→Nexus 5X 5'2“→Oreo x86→Android 8.0→AVD名称（Nexus 5X API 26）→完成
- 选择**Nexus 5X API 26**仿真器→单击启动AVD（开始）按钮

英文：

- Tools → AVD Manager → Opens a window
- Select → Create Virtual Device
- Phone → Nexus 5X 5'2" → Oreo x86 → Android 8.0 → AVD Name(Nexus 5X API 26) → Finish
- Select **Nexus 5X API 26** emulator → click on Launch AVD(start) button

![Image for post](https://miro.medium.com/freeze/max/60/1*Nz0C7NC0wR73z7KcPhUVrg.gif?q=20)

![Image for post](https://miro.medium.com/max/1152/1*Nz0C7NC0wR73z7KcPhUVrg.gif)

创建Android模拟器

# #3 — 3-iOS模拟器设置

## Xcode的安装

- 要在iOS模拟器中执行该应用，我们必须安装Xcode。从App Store获取Xcode并安装。

**从Android Studio启动iOS模拟器**

- 您可以在Android Studio中的工具栏下方看到**Flutter Device Selection**按钮。
- 如果您已经安装了Xcode，则单击**Flutter Device Selection**按钮将打开iOS模拟器。

## **有用的iOS模拟器命令**

- **Return to home →** `Cmd`**+**`Shift`**+**`h`
- **See recent apps** →`Cmd`+`Shift`+`h`+`h`
- **Quit Simulator**→`Cmd`+`q`

# #4 — 使用终端在仿真器或模拟器上运行

## 从终端执行Flutter应用

- **Run From Terminal →** `flutter run`
- **Run in all devices →** `flutter run -d all lib/welcome.dart`

如果您的系统连接了多个设备，**请**使用以下命令**检查连接的设备** → `flutter devices`

> *运行上面的命令将列出如下设备*
>
> SM G890UU • **4299a0c86788f678** • android-arm • Android 7.0 (API 24) (emulator)

**在所选设备中运行** →** `flutter run -d 4299a0c86788f678`

检查选定设备中的**特定设计/运行特定的dart文件** → `flutter run -d 4299a0c86788f678 lib/main.dart`

# #5 — Flutter项目结构

在本文中，我将在Android Studio中使用Flutter。

![Image for post](https://miro.medium.com/max/60/1*2O2l1NtUVPZ_we23GHnUMQ.png?q=20)

![Image for post](https://miro.medium.com/max/484/1*2O2l1NtUVPZ_we23GHnUMQ.png)

- 在“项目”部分中，以上屏幕截图显示了flutter应用程序的结构。
  - **Android：**具有所有与Android相关的文件。
  - **iOS：**具有所有与iOS相关的文件。
  - **lib：**具有所有的dart文件。这是主文件夹，我们可以在其中编写所有应用程序代码。
  - **测试：**具有所有测试代码。
  - **images：**我创建了这个文件夹，其中包含了我们在应用程序中使用的所有图像。
  - **pubspec.yaml：**它具有所有第三方依赖性以及我们在应用程序中使用的资产。
- **yaml** —另一个多列布局

![Image for post](https://miro.medium.com/max/60/1*KXH3GqoQdoeGlZTrrXgLoQ.png?q=20)

![Image for post](https://miro.medium.com/max/674/1*KXH3GqoQdoeGlZTrrXgLoQ.png)

> `*flutter_rating*`是第三方依赖性。
> 我们必须在`pubspec.yaml`指定。我们可以使用文件夹名称和文件名或仅使用文件夹名称来指定文件路径。
>
> `images/apple.jpeg`→我们只能使用apple.jpeg图像。
> `images/`→我们可以使用图像文件夹中的所有图像。
>
> > 确保依赖项**与空格正确对齐**。否则，图像将不会显示。如果您是Flutter或YAML的新手，则很难找到此特定问题。

# **#6 — **如何更改应用名称**

## **在Android中**

Android文件夹→app→src→main→Open Manifest→更改Application标签中的标签。

Android folder → app → src → main → Open Manifest → change the label in Application tag.

```dart
<application
android：name =” io.flutter.app.FlutterApplication”
android：label =” flutter_android_app ” />
```



## **在iOS中**

iOS文件夹→Runner→info.plist→编辑下面的字符串`CFBundleName`

> <key>CFBundleName</key>
> <string>**flutter_ios_app**</string>

# #7 — 如何更改应用程序图标

## **在Android中**

展开Android文件夹→应用程序→src→主菜单→res→在mipmap文件夹中添加应用程序图标。

Expand the Android folder → app → src → main → res → add app icons in mipmap folders.

展开Android文件夹→app→src→main→打开Manifest→更改Application标签中的图标。

Expand the Android folder → app → src → main → open Manifest → change the icon in Application tag.

> <application
> android:name=”io.flutter.app.FlutterApplication”
> android:label=”flutter_android_app”
> android:icon=”**@mipmap/ic_launcher**”>

**在iOS中，
**展开iOS文件夹→Runner→Assets.xcassets→AppIcon.appiconset
在该文件夹中，添加所有应用程序图标。

Expand iOS folder → Runner → Assets.xcassets → AppIcon.appiconset
In that folder add all app icons.

展开iOS文件夹→Runner→Assets.xcassets→AppIcon.appiconset→更新`Contents.json`文件。

Expand iOS folder → Runner → Assets.xcassets → AppIcon.appiconset → update the `Contents.json` file.

# #8 — 如何更改启动画面

## **在Android中**

Expand Android folder → res → drawable → add image
Expand Android folder → res → drawable → in Launch_background.xml, add bitmap tag.

> <item>
> <bitmap
> android:gravity=”center”
> android:src=”**@drawable/ios_android**” />
> </item>

## **在iOS中**

Expand iOS folder → Runner → Assets.xcassets → LaunchImage.imageset → add all sizes of splash images in the folder

Expand iOS folder → Runner → Assets.xcassets → LaunchImage.imageset → → open `Contents.json `→ change file name

我希望上述**设置说明**和有关如何设置一些**基本要求的**建议对Flutter应用程序开发的初学者有用。由于**Flutter现已退出测试版**，因此我希望看到越来越多的开发人员使用Flutter通过单个代码库开发Android和iOS应用程序。出于**动机，**请[访问](https://itsallwidgets.com/)Flutter社区构建的[Widgets库网站](https://itsallwidgets.com/)。

如果您喜欢这篇文章，并且对您有所帮助，请给我一些鼓掌！

谢谢😄

参考链接：https://medium.com/mobile-app-developers/how-to-setup-flutter-f70a6b21997f