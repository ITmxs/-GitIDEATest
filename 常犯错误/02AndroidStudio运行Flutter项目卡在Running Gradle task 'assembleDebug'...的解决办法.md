# AndroidStudio运行Flutter项目卡在Running Gradle task 'assembleDebug'...的解决办法

#### 问题

> ```
> AndroidStudio`运行`Flutter`项目卡在`Running Gradle task 'assembleDebug'...
> ```

![image-20201108223127279](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201108223127279.png)

#### 问题出现时的软件+硬件:

> ```
> Android Studio 3.6.0` 、 `windows`、`OPPO A5
> ```

在百度上谷歌了好大一阵儿时间，各种修改配置和地址，一直没有解决。最后自己根据之前的搜索结果摸索出一点，问题出在`Gradle`，那就升级一下它吧。
 在`android` -- `wrapper` -- `gradle-wrapper.properties`下修改`distributionUrl`的值，将`gadle`修改为最新版本，如下

> ###### distributionUrl=https\://services.gradle.org/distributions/gradle-5.6.2-all.zip

接着在运行`build.gradle`文件，等待`gradle`更新完成。最后就一切OK ，很快完美运行起来了。



