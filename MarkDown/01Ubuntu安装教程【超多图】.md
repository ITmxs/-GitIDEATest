#### 01前言

作为一名学习编程的人来说，或多或少会接触到Linux系统，想要获得Linux环境，一个办法就是将电脑系统直接换成Linux系统，但我们平常用惯了Windows系统，直接切换为Linux系统或多或少会有很多不方便的地方。另一个比较土豪的办法是，再买一台电脑，然后将系统换成Linux系统。但这种方法就比较伤钱包了

一个比较折中的方案是，在自己的电脑上安装一个Linux虚拟机。所谓虚拟机，就是在你已有的电脑里再虚拟出一个或多个电脑，可以理解为电脑中的电脑。

比如说，你可以在虚拟机里安装一个Window电脑，或者安装一个Linux电脑，都是可以的。虚拟机的作用就是帮你虚拟出运行一台真正的电脑所需要的各种资源，然后就可以在上面跑其它的操作系统。

常用的虚拟机有**Wmware，VirtualBox**这两种。这两种虚拟机用起来差不多，但WMware功能更全面，因此Luckly更喜欢用VMware，你们可以根据自己的喜好来选择。

#### 02虚拟机的安装

![image-20201109153533227](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109153533227.png)

首先要下载虚拟机的安装包，当前最新版本是VMware 15.0。下载途径有三个：

1. [VMware官网](https://www.vmware.com/)；

2.**找Luckly** -->  公众号内回复：**虚拟机**，获取下载链接。 公众号 《萌小肆聊编程》

虚拟机安装包下载完毕之后，将它安装到电脑里。这个安装过程很简单，一路下一步就好了。

#### 03Ubuntu镜像的下载

虚拟机安装完之后，这时候才完成第一步。这就像买回来了一台电脑，但还没安装操作系统。Linux的发行版有很多版本可以选择，比如：Ubuntu，Fedora，Centos，OpenSUSE，等等。其中，对于入门者来说，使用**Ubuntu**比较适合，因为它各种库什么的都已经集成好了，无需再繁琐的安装了。

获取Ubuntu20.0.4 64位操作系统的镜像方法有三个：

1. [Ubuntu官网](https://ubuntu.com/download)； 

2. **找Luckly** -->  公众号内回复：**虚拟机**，获取下载链接。 公众号 《萌小肆聊编程》

下载完之后把镜像随便放在一个地方（比如桌面），你只要能找到就好，安装完之后就可以删除掉它。

#### 04虚拟机硬件配置

在正式安装虚拟机之前，要先配置一下电脑，比如给它分配多大内存，CPU几核的，网络类型是怎样的，等等，就跟我们攒机一样。

##### 1.虚拟机安装完毕之后，界面如下图所示：

<img src="https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109154826763.png" alt="image-20201109154826763" style="zoom:80%;" />



点击图中红圈图标，开始创建一个新的虚拟机；

或者点击文件--》新建虚拟机

![image-20201109153620232](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109153620232.png)

##### 2.在弹出的对话框中选择自定义，然后点击下一步：



![image-20201109153705072](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109153705072.png)

##### 3.在 「虚拟机硬件兼容性」 里选择默认的即可，直接下一步：

![image-20201109153721834](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109153721834.png)



##### 4.在 「安装客户机操作系统」 里选择 「稍后安装操作系统」 ，然后点击下一步：

![image-20201109153743772](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109153743772.png)

##### 5.依然选择默认的，直接下一步：

![image-20201109153806169](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109153806169.png)

##### 6.在这一步 「命名虚拟机」 里，给自己的虚拟机命个名称，比如Ubuntu_LX，再选择要安装的位置。虚拟机所产生的文件比较大，所以选择位置所在的磁盘最好剩余空间大一些。



![image-20201109153836415](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109153836415.png)

##### 7.虚拟机处理器数量及内核都选择2，对于开发来说够用了。即使不够用的话，这个参数也是可以修改的。

![image-20201109153853793](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109153853793.png)

##### 8.虚拟机内存选择2048M，也就是2G，最好选择1G，2G，4G，8G，不要选择3G这样的。这个参数后期也可以修改。



![image-20201109153909464](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109153909464.png)

##### 9.后面这几步都可以直接「下一步即可」 ，磁盘空间20G不够的话可以选择40G，这个是动态的，也就是不是一下子就占用了你磁盘40G，而是用多少占多少。



<img src="https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109153927478.png" alt="img" style="zoom: 67%;" />



<img src="https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109154050330.png" alt="img" style="zoom:67%;" />

<img src="https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109154121980.png" alt="image-20201109154121980" style="zoom:67%;" />



<img src="https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109154225597.png" alt="image-20201109154225597" style="zoom:67%;" />



<img src="https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109154250119.png" alt="image-20201109154250119" style="zoom:67%;" />

<img src="https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109154318035.png" alt="image-20201109154318035" style="zoom:67%;" />

##### 10.上面几步完成之后，虚拟机长这个样：



![image-20201109154346178](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109154346178.png)





#### 05Ubuntu镜像安装

虚拟机硬件配置好之后，接下来正式安装Ubuntu操作系统。

##### 1.点击上图圈出来的 「编辑虚拟机设置」

![image-20201109152442109](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109152442109.png)

##### 2.在弹出的菜单里，从左边选择 「CD/DVD(SATA)」 ，然后在右边选择「使用ISO镜像文件」，再点击浏览，找到Ubuntu镜像。

![image-20201109152515030](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109152515030.png)

##### 3.之后点击确定，再点击「开启虚拟机」 。

##### 4.虚拟机开启之后，选择 「Install Ubuntu」 。左边的语言选择，是指系统语言。我们做开发的，建议语言什么的都选择英语。

![img](https://mmbiz.qpic.cn/mmbiz_png/YeUmRxGrEBb22Ds5JPqFeic2uCO0KNasYViaBu2zPk4EvwHueWepiabJrORC8lv3YI5stkadibIo5SX9piawb3dOObg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

##### 5.接下来这步选择的是键盘布局。我们这边所使用的布局是美国标准的，所以都选择English(US)。



![img](https://mmbiz.qpic.cn/mmbiz_png/YeUmRxGrEBb22Ds5JPqFeic2uCO0KNasY80NKbPjiaPf8yEg7VdJC5701OZbKFjL7mHXmAUUU7WsvGE3VRSZQg6w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

##### 6.接下来这一步直接默认：

![image-20201109151329595](C:/Users/Luckly/AppData/Roaming/Typora/typora-user-images/image-20201109151329595.png)

##### 7.在 「Installation Type」 里也是默认即可，直接点击 「Install Now」，之后的弹出窗口里点击 「continue」：

![image-20201109151553732](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109151553732.png)

##### 8.点击continue

![image-20201109151726427](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109151726427.png)

##### 9.在 「Where Are You?」 地图里点击一下大中国，然后点击 「continue」：

![image-20201109152043446](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109152043446.png)

##### 10.在 「Who Are You?」 填入个人基本信息，然后点击 「continue」，接下来就进入了下载安装的过程，整个过程大概需要20分钟。

![image-20201109152234984](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109152234984.png)

##### 11.看到此界面，表示正在安装

![image-20201109152323877](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109152323877.png)

##### 12.安装完毕之后选择 「restart now」，重启虚拟机。至此，虚拟机及Linux系统均已经安装完成。

![image-20201109155949479](https://mxszs.oss-cn-beijing.aliyuncs.com/img/image-20201109155949479.png)