



[TOC]

[官方仓库

[](https://pub.dev/packages/map_launcher)



# Flutter 调用地图软件（高德、百度、腾讯、苹果）

# 一、说明

我们在应用开发中经常需要用到地图定位或导航的功能，而集成该功能的方式总的来说分为两类：

### 第 1 类：App 集成导航功能

这种方式的优点是可以进行深度地图定制，比如出行或外卖软件会有自己的定制，上面会有司机或骑手的小图标，但是集成开发成本也是比较高的。

### 第 2 类：跳转第三方地图软件

这种方式是比较简单的一种，把目的地传给第三方导航软件，比如百度地图，它会为你提供导航功能。这种方式开发成本低，可快速提供导航功能。

由于 Flutter 技术出来不久，普及率算不上很高，目前很多导航定位还未对其提供专门支持，如果想集成导航功能的话，开发成本非常高，所以我们会考虑用跳转三方地图软件的方式实现导航功能。

# 二、实现步骤

## 使用

### 1、添加插件：

```bash
dependencies:
  map_launcher: ^1.1.2
```

### 2、iOS 配置 info.plist

```bash
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>comgooglemaps</string>
    <string>baidumap</string>
    <string>iosamap</string>
    <string>waze</string>
    <string>yandexmaps</string>
    <string>yandexnavi</string>
    <string>citymapper</string>
    <string>mapswithme</string>
    <string>osmandmaps</string>
    <string>dgis</string>
    <string>qqmap</string>
</array>
```

### 3、编写工具类

```
import 'package:flutter/material.dart';
import 'package:flutter_svg/flutter_svg.dart';
import 'package:map_launcher/map_launcher.dart';

class MapsSheet {
  BuildContext context;
  Function(AvailableMap map) onMapTap;
  MapsSheet({this.context, this.onMapTap});

  void show() async {
    List<AvailableMap> availableMaps = await MapLauncher.installedMaps;
    for (var map in availableMaps) {
      map.mapName = getLocalName(amap: map);
    }

    showModalBottomSheet(
      // backgroundColor: Colors.transparent,
      context: context,

      builder: (BuildContext context) {
        return Container(
          padding: EdgeInsets.only(left: 10),
          decoration: BoxDecoration(
            color: Colors.white,
            borderRadius: BorderRadius.all(Radius.circular(20)),
          ),
          height: 400,
          child: Column(
            children: <Widget>[
              Expanded(
                child: SingleChildScrollView(
                  child: Container(
                    decoration: BoxDecoration(
                      color: Colors.white,
                      borderRadius: BorderRadius.all(Radius.circular(16)),
                    ),
                    child: Wrap(
                      children: <Widget>[
                        for (var map in availableMaps)
                          ListTile(
                            onTap: () => onMapTap(map),
                            title: Text(map.mapName),
                            leading: SvgPicture.asset(
                              map.icon,
                              height: 30.0,
                              width: 30.0,
                            ),
                          ),
                      ],
                    ),
                  ),
                ),
              ),
            ],
          ),
        );
      },
    );
  }

  String getLocalName({AvailableMap amap}) {
    switch (amap.mapType) {
      case MapType.amap:
        return '高德地图';

      case MapType.baidu:
        return '百度地图';
      case MapType.tencent:
        return '腾讯地图';
      case MapType.google:
        return '谷歌地图';

      default:
        return amap.mapName;
    }
  }
}

```

### 4.使用：

```
import 'package:map_launcher/map_launcher.dart';
                DirectionsMode directionsMode = DirectionsMode.driving;
                List<Coords> waypoints = [
                  Coords(_local.lat, _local.lng),
                ];
                MapsSheet mapsheet = MapsSheet(
                  context: context,
                  onMapTap: (map) {
                    map.showDirections(
                      destination: Coords(_local.lat, _local.lng),
                      destinationTitle: _local.addr,
                      origin: _local.lat == null || _local.lng == null
                          ? null
                          : Coords(_local.lat, _local.lng),
                      directionsMode: directionsMode,
                      waypoints: waypoints,
                    );
                  },
                );
                mapsheet.show();
```

