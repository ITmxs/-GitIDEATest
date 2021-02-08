



```
//Box装饰器

  Widget boxDecoration() {
    return Center(
      child: Container(
        width: 100.0,
        height: 100.0,
        //添加装饰
        child: DecoratedBox(
          
          //装饰定位 DecorationPosition.background背景模式 DecorationPosition.foreground前景模式
          position: DecorationPosition.background,
          decoration: BoxDecoration(
            
            //背景色
            color: Colors.grey,
            // //设置背景图片

            // image: DecorationImage(
            //   //图片填充方式
            //   fit: BoxFit.cover,
            //   image: ExactAssetImage('assets/view.jpg' ),
            // ),
            //边框弧度
            //borderRadius: BorderRadius.circular(10.0),
            border: Border.all(
              //边框颜色
              color: Colors.black,
              //边框粗细
              width: 1.0,
            ),
            //边框样式
            shape: BoxShape.rectangle,
            
          ),
          child: IconButton(icon: Icon(Icons.add), onPressed: _modelBottomCameraSheet,
          
        ),
      ),
    ));
  }
  
```

