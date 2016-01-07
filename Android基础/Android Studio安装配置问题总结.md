  之前一直都是使用Eclipse写Android的，但一直都有听说Android Studio是一个更好的用于开发Android的软件，之前其实也有保存过一些别人写的安装和配置Android Studio的一些教程，在这里也列出来：
[Android Studio 入门指南](http://www.jianshu.com/p/36cfa1614d23)；
[Eclipse，到了说再见的时候了——Android Studio最全解析](http://android.jobbole.com/77635/)
[Android Studio系列教程一--下载与安装](http://stormzhang.com/devtools/2014/11/25/android-studio-tutorial1/)
另外，还有一个比较详细的介绍配置的博文系列：
[第一次使用Android Studio时你应该知道的一切配置](http://www.cnblogs.com/smyhvae/p/4390905.html)
[第一次使用Android Studio时你应该知道的一切配置（二）：新建一个属于自己的工程并安装Genymotion模拟器](http://www.cnblogs.com/smyhvae/p/4392611.html)
[第一次使用Android Studio时你应该知道的一切配置（三）：gradle项目构建](http://www.cnblogs.com/smyhvae/p/4456420.html)

在这里就总结下我安装的过程中遇到的一些问题吧，首先我是从[SDK Tools - AndroidDevTools](https://github.com/inferjay/AndroidDevTools#sdk-tools)
这里下载的，毕竟尚未使用过翻墙软件，而且这个Github里面也分享了很多有关Android开发过程中使用到的一些工具，软件，还有一些电子书，资料非常齐全。

安装完成后，遇到几个问题，分别如下：
 1. 首先第一个问题是弹出一个对话框，提示the environment variable JAVA_HOME(with the value of "F:\Java\JDK\" ) does not point to a valid JVM installation. 
“F:\Java\JDK\”这是我安装jdk的文件路径
这个问题上网搜索了好久，有的说需要在JDK\后面加“；”，有的说去掉“\”，反正试了这几个都不行，我干脆就直接去掉“JDK\",发现这个问题居然解决了....

 2. 在解决了第一个问题后，马上就出现第二个问题了，又弹出一个对话框显示以下这段文字：
‘ tools.jar' seems to be not in Android Studio classpath.Please ensure JAVA_HOME points to JDK rather than JRE.”

百度了这个问题，根据['tools.jar'seems to be not in Android Studio](http://jingyan.baidu.com/article/ce4366491d06343773afd3cc.html)
这里给出的一些建议解决了问题，我主要是将jdk和jre分开了，并且没有设置SDK的环境变量，将这两个问题解决完后，就可以使用Android Studio了！

2015-12-24 补充
  因为换了新电脑，并且使用了win10的系统，所以需要重新安装java jdk，android sdk以及android studio，所以就在这再补充下前面安装配置jdk和sdk中遇到的问题。

首先是jdk，按照这个[如何在WIN10搭建Java开发环境](http://jingyan.baidu.com/article/fea4511a12b158f7bb9125b9.html)，在官网上下载jdk，安装，然后配置环境变量，但是在验证是否安装配置成功的时候，发现输入“java”是成功的，但是输入“javac”命令时，却提示了不是内部环境变量，但是在jdk的bin文件夹中是存在javac.exe这个文件，所以应该是配置环境变量中path的出问题了，当然我是按照上面教程中输入路径的，但是win10提供了浏览这个选项，通过这个找到要配置的文件路径，发现就成功解决这个问题了。
![这里写图片描述](http://img.blog.csdn.net/20151224205331635)



