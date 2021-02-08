```

where should i add?
```



```
import 'package:flutter/material.dart';
import 'package:flutter_svg/flutter_svg.dart';
import 'package:map_launcher/map_launcher.dart';

class MapsSheet {
static show({
@required BuildContext context,
@required Function(AvailableMap map) onMapTap,
}) async {
final availableMaps = await MapLauncher.installedMaps;

showModalBottomSheet(
  context: context,
  builder: (BuildContext context) {
    return SafeArea(
      child: Column(
        children: <Widget>[
          Expanded(
            child: SingleChildScrollView(
              child: Container(
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
}
```

使用：

```
 DirectionsMode directionsMode = DirectionsMode.driving;
                List<Coords> waypoints = [
                  Coords(_local.lat, _local.lng),
                ];
                MapsSheet.show(
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
```

