https://pub.dev/packages/flutter_screenutil

```
import'package:flutter/material.dart';
classScreenUtil{
staticScreenUtilinstance=newScreenUtil();
//设计稿的设备尺寸修改doublewidth;
doubleheight;
boolallowFontScaling;
staticMediaQueryData_mediaQueryData;staticdouble_screenWidth;
staticdouble_screenHeight;staticdouble_pixelRatio;staticdouble_statusBarHeight;
staticdouble_bottomBarHeight;
staticdouble_textScaleFactor;
ScreenUtil({this.width=1080,this.height=1920,
this.allowFontScaling=false,});
staticScreenUtilgetInstance(){returninstance;
}
voidinit(BuildContextcontext){
MediaQueryDatamediaQuery=MediaQuery.of(context);_mediaQueryData=mediaQuery;
_pixelRatio=mediaQuery.devicePixelRatio;_screenWidth=mediaQuery.size.width;_screenHeight=mediaQuery.size.height;
_statusBarHeight=mediaQuery.padding.top;_bottomBarHeight=_mediaQueryData.padding.bottom;_textScaleFactor=mediaQuery.textScaleFactor;
}
staticMediaQueryDatagetmediaQueryData=>_mediaQueryData;
///每个逻辑像素的字体像素数，字体的缩放比例staticdoublegettextScaleFactory=>_textScaleFactor;
///设备的像素密度
staticdoublegetpixelRatio=>_pixelRatio;
///当前设备宽度dp
staticdoublegetscreenWidthDp=>_screenWidth;
///当前设备高度dp
staticdoublegetscreenHeightDp=>_screenHeight;
///当前设备宽度px
staticdoublegetscreenWidth=>_screenWidth*_pixelRatio;
///当前设备高度px
staticdoublegetscreenHeight=>_screenHeight*_pixelRatio;
///状态栏高度dp刘海屏会更高
staticdoublegetstatusBarHeight=>_statusBarHeight;
///底部安全区距离dp
staticdoublegetbottomBarHeight=>_bottomBarHeight;
///实际的dp与设计稿px的比例getscaleWidth=>_screenWidth/instance.width;
getscaleHeight=>_screenHeight/instance.height;
///根据设计稿的设备宽度适配
///高度也根据这个来做适配可以保证不变形setWidth(doublewidth)=>width*scaleWidth;
///根据设计稿的设备高度适配
///当发现设计稿中的一屏显示的与当前样式效果不符合时,///或者形状有差异时,高度适配建议使用此方法
///高度适配主要针对想根据设计稿的一屏展示一样的效果setHeight(doubleheight)=>height*scaleHeight;
///字体大小适配方法
///@paramfontSize传入设计稿上字体的px,
///@paramallowFontScaling控制字体是否要根据系统的“字体大小”辅助选项来进行缩放。默认值为false。
///@paramallowFontScalingSpecifieswhetherfontsshouldscaletorespectTextSize accessibilitysettings.Thedefaultisfalse.
setSp(doublefontSize)=>allowFontScaling?setWidth(fontSize):setWidth(fontSize)/_textScaleFactor;
} 
```

