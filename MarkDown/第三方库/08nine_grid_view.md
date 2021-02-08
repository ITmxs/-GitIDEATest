### NineGridView

类似微博动态，微信朋友圈，展示图片的九宫格控件。支持单张大图预览。
同时也支持微信群组，钉钉群组，QQ讨论组头像效果。

### DragSortView

类似微博/微信发布动态选图九宫格。支持按压放大效果，拖拽排序，拖拽到指定位置删除。

https://github.com/flutterchina/nine_grid_view/blob/master/README-ZH.md







```
DefaultTabController(length: 3, child:Row(
  children: [
     TabBar(tabs: [
  Tab(text: "全部"),
            Tab(text: "关注的人"),
            Tab(text: "我的群"),
])
  ],
)),
TabBarView(children: [
          Text("这是全部内容"),
          Text("这是我的关注"),
          Text("这是我的群"),
          

        ])
```

