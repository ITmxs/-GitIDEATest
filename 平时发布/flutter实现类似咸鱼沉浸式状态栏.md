

### 类似咸鱼沉浸式状态栏

#### 第一步 导包

```
import 'dart:io';
import 'package:flutter/services.dart';
12
```

#### 第二步 添加代码

<img src="https://luckly007.oss-cn-beijing.aliyuncs.com/images/20210208150230.png" alt="image-20210208150229995" style="zoom: 80%;" />





```dart
if (Platform.isAndroid) {
      SystemUiOverlayStyle systemUiOverlayStyle =
          SystemUiOverlayStyle(statusBarColor: Colors.transparent);
      SystemChrome.setSystemUIOverlayStyle(systemUiOverlayStyle);
    }

```

