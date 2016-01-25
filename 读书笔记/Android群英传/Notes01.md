这是在阅读《Android群英传》时做的读书笔记。

这是前面两章的笔记，主要就是简单介绍Android以及Android Studio的介绍，ADB命令介绍。

#目录
[第1章 Android 体系与系统架构](#01)

[第2章 Android开发工具新接触](#02)

<h2 id="01">第1章 Android 体系与系统架构</h2>
###1. Android系统架构
Android的系统架构是分成4层的，分别是**Linux内核层、库和运行时、Framework层和应用层**。

**Dalvik与ART**
Dalvik包含了一整套的Android运行环境虚拟机，每个App都会分配Dalvik虚拟机来保证互相之间不受干扰，并保持独立，它的特点是运行时编译。

ART是在5.X版本使用的，取代了Dalvik,它的特点是安装时就进行编译，以后运行时就不用编译了。

###2. Android App 组件架构
**四大组件**
在应用层，Android的App组件架构，通常就是指四大组件，也就是**Activity、BroadCastReceiver、ContentProvider和Service。

**应用运行上下文对象**
Android系统的上下文对象，即在Context中，为我们封装了一个“语境”：也就是当前对象在程序中所处的一个环境，一个与系统交互的过程。Activity,Service,Application都是继承自Context.

**Android应用程序会在创建Applicaiton、Activity、Service时创建应用上下文Context。**

当应用程序第一次启动时，Android系统都会创建一个Application对象，同时创建Application Context，所有的组件都共同拥有这样一个Context对象，并且这个应用上下文对象贯穿整个应用进程的生命周期。可以通过`getApplicationContext()`方法来获取这个Context。

**查看源代码网站**：http://androidxref.com/

**Android与很多语言一样，引入了Makefile机制，Makefile最大的好处就是自动化编译，同时还可以做到可控制的编译。**


<h2 id="02">第2章 Android开发工具新接触</h2>
###1. Android Studio
在过去一段时间，Google都是基于Eclipse来作为Android开发的综合性IDE，但在2013年上，Google发布了Android Studio，到现在最新版本已经是达到2.0了，现在已经是官方指定的Android开发IDE了。下载地址当然是[官网](http://developer.android.com/sdk/installing/studio.html)，但是在国内，基本是登录不上去（如果不翻墙），所以这里介绍一个好用的镜像网站--[AndroidDevTools](http://www.androiddevtools.cn/)(当然我也是从这个网站下载AS的)，这个网站可是汇集了很多大家开发时需要用到的资源、工具。

这里我也列出当初安装Android Studio时参考的教程:

1. [Android Studio 入门指南](http://www.jianshu.com/p/36cfa1614d23)

2. [Eclipse，到了说再见的时候了——Android Studio最全解析](http://android.jobbole.com/77635/)

3. [Android Studio系列教程一--下载与安装](http://stormzhang.com/devtools/2014/11/25/android-studio-tutorial1/)

配置使用的教程:

1. [第一次使用Android Studio时你应该知道的一切配置](http://www.cnblogs.com/smyhvae/p/4390905.html)

2. [第一次使用Android Studio时你应该知道的一切配置（二）：新建一个属于自己的工程并安装Genymotion模拟器](http://www.cnblogs.com/smyhvae/p/4392611.html)

3. [第一次使用Android Studio时你应该知道的一切配置（三）：gradle项目构建](http://www.cnblogs.com/smyhvae/p/4456420.html)

###2. ADB(Android Debug Bridge)
####1. ADB基础
ADB工具位于SDK的**platform-tools**目录下，在命令行使用ADB需要进入到该目录中，或者将该路径添加到环境变量中。配置好后，在命令行中输入以下命令。

    C:\Users\Administrator>adb version
    # 显示下列内容，则是配置成功
    Android Debug Bridge version 1.0.32
在Windows系统上还需要下载对应的手机驱动，而Linux系统则不需要，一般可以下载各种手机助手，它们会自动识别手机并下载对应驱动，当然它们也是通过ADB来实现它们的功能的。

在命令行中输入`adb shell`就可以使用shell命令了，同时命令行中会显示`shell@hwH60:/ $ `,其中**@**后面跟着的应该就是你的手机品牌和型号。

####2. ADB 常用命令

    # 显示系统中全部Android平台
    C:\Users\Administrator>android list targets
    Available Android targets:
    ----------
    id: 1 or "android-15"
         Name: Android 4.0.3
         Type: Platform
         API level: 15
         Revision: 5
         Skins: HVGA, QVGA, WQVGA400, WQVGA432, WSVGA, WVGA800 (default), WVGA854, WXGA720, WXGA800
     Tag/ABIs : default/armeabi-v7a

     # 安装Apk程序之Install
     adb install-r 应用程序.apk
     # 安装Apk程序之Push(这个命令是写入手机中指定的位置，而非安装程序，还可以用来写入文件)
     adb push <local> <remote>
     # 从手机获取文件
     adb pull <remote> <local>
     eg. C:\Users\Administrator>adb pull /system/temp/ D:\file.txt

     # 查看Log
     C:\Users\Administrator>adb shell
     shell@hwH60:/ $ logcat | grep "abc"

     # 删除应用
     adb remount (重新挂载系统分区，使系统分区重新可写)
     adb shell
     cd system/app
     rm *.apk

     # 查看系统盘符
     adb shell df

     # 输出所有已经安装的应用
     adb shell pm list packages -f

     # 模拟按键输入
     adb shell input keyevent number(number就是要执行的Keyevent的Code)
     # 这里记录下作者书中给出的常用的部分，都可以从网上查到
     input keyevent 82 menu
     input keyevent 3 home
     input keyevent 4 back
     input keyevent 66 enter

     # 模拟滑动输入
     adb shell input touchscreen <x1> <y1> <x2> <y2>
     adb shell input touchscreen swipe 18 665 18 350

     # 查看运行状态
     adb shell dumpsys
     eg. C:\Users\Administrator>dumpsys activity activities | grep "tencent"

**Package管理信息**
详细，具体的命令可以查看API文档。

    # 列出所有的Package
    shell@hwH60:/ $ pm list packages -f

**AM管理信息**
依旧推荐查看API文档

    # 启动一个Activity
    adb shell am start -n 包名/包名+类名
    # 录制屏幕
    adb shell screenrecord/sdcard/demo.mp4
    # 重新启动
    adb reboot

**ADB命令来源**
ADB命令和Shell命令来源于这两个文件夹:**\system\core\toolbox 和 \framework\base\cmds**

**模拟器使用**
作者推荐使用的是[Genymotion](http://www.genymotion.net/)，我正在用的也是这个模拟器，毕竟Android官方的模拟器的确是不尽如人意，而这个模拟器的确是够快！非常好用！





