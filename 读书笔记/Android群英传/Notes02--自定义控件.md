##第3章 Android 控件架构与自定义控件详解
###1. Android 控件架构
(1) Android中控件大致被分成两类，即**ViewGroup**控件和**View**控件。ViewGroup控件作为父控件可以包含多个View控件，并管理其包含的View控件。通过ViewGroup，整个界面上的控件形成了一个树形结构，也就是我们常说的控件树，上层控件负责下层控件的测量和绘制，并传递交互事件。每颗控件树的顶部都有一个ViewParent对象。View树结构图如下所示:

![View 树结构](https://raw.githubusercontent.com/ccc013/Android-Study/master/pictures/Android%20View%E6%A0%91%E7%BB%93%E6%9E%84%E5%9B%BE.png "View 树结构")

(2) Activity中使用的findViewById()方法就是在控件树中以树的深度优先遍历来查找对于元素的。

(3) 每个Activity都包含一个Window对象，Android中Window对象通常由PhoneWindow来实现，而PhoneWindow将一个DecorView设置为整个应用窗口的根View，而在DecorView中，显示上将屏幕分为两部分，一个是TitleView,另一个是ContentView,后者就是一个ID为content的Framelayout,activity_main.xml就是设置在这样一个Framelayout里。


###2. View的测量
* 对View的测量是在**onMeasure()**方法中进行

* Android系统提供了一个设计短小精悍却功能强大的类--MeasureSpec类，通过它可以帮助我们测量View。MeasureSpec是一个32位的int值，其中高2位是测量的模式，低30位是测量的大小，也就是可以获取View的测量模式和其想要绘制的大小。

* 测量的模式分为3种：

    (1) EXACTLY--精确值模式，当将控件的layout_width或layout_height属性指定为具体数值或者是match_parent时，就是使用这个模式；

    (2) AT_MOST--最大值模式，当控件的layout_width或layout_height属性指定为wrap_content时，控件大小一般随着控件的子控件或内容的变化而变化，此时控件的尺寸只要不超过父控件允许的最大尺寸即可；

    (3) UNSPECIFIED--它不指定其大小测量模式，View想多大就多大，通常情况下在绘制自定义View时才会使用。

* View类默认的onMeasure()方法只支持EXACTLY模式，所以自定义控件时需要支持wrap_content属性，就必须重写onMeasure()方法来指定wrap_content的大小

###3.View的绘制
* 重写onDraw()方法，并在Canvas对象上来绘制所需要的图形

* onDraw()中有个参数，就是`Canvas canvas`对象，在创建Canvas对象时，需要传进去一个bitmap对象，这个bitmap用来存储所有绘制在Canvas上的像素信息，然后调用所有的Canvas.drawXXXX方法都发生在这个bitmap上。

###4.ViewGroup的测量和绘制
* ViewGroup的大小为wrap_content时，它就需要对子View进行遍历，即调用它们的Measure方法来获得每个子View的测量结果，从而决定自己的大小；而在其他模式下则会通过具体的指定值来设置自身的大小
* 当子View测量完后，就需要将它们放到合适的位置，这个过程就是View的Layout过程，ViewGroup在执行Layout过程，也是使用遍历来调用子View的Layout方法，并指定其具体显示的位置，从而来决定其布局位置。
* 在自定义ViewGroup时，通常会重写**onLayout()**方法来控制其子View显示位置的逻辑

* ViewGroup通常情况不需要绘制，如果不是指定它的背景颜色，其onDraw()方法都不会被调用；但ViewGroup会使用dispatchDraw()方法来绘制其子View，其过程同样是通过遍历所有子View，并调用子View的绘制方法来完成绘制工作

###5.自定义View
* 通常情况，有三种方法来实现自定义的控件:

    a. 对现有控件进行拓展--一般在onDraw()方法中对原生控件行为进行扩展

    b. 通过组合来实现新的控件--通常需要继承一个合适的ViewGroup，再给它添加指定功能的控件，从而组成新的复合控件

    c. 重写View来实现全新的控件--通常需要继承View类，并重写它的onDraw()、onMeasure()等方法来实现绘制逻辑，同时通过重写onTouchEvent()等触控事件来实现交互逻辑

* View中通常有以下一些比较重要的回调方法：
   
   * onFinishInflate(): 从XML加载组件后回调
   * onSizeChanged(): 组件大小改变时回调
   * onMeasure(): 回调该方法来进行测量
   * onLayout(): 回调该方法来确定显示的位置
   * onTouchEvent(): 监听到触摸事件时回调

###6.自定义ViewGroup
自定义ViewGroup通常需要重写onMeasure()方法来对子View进行测量，重写onLayout()方法来确定子View的位置，重写onTouchEvent()方法增加响应事件。

###7.事件拦截机制分析
* 触摸事件就是捕获触摸屏幕后产生的事件，Android为触摸事件封装了一个类——MotionEvent

* 对于ViewGroup来说，需要重写三个方法：**dispatchTouchEvent(),onInterceptTouchEvent(),onTouchEvent()**;对于View，则是重写两个方法：**dispatchTouchEvent(),onTouchEvent()**

* 事件传递的时候，是从上而下，即从控件树的最顶端的一个ViewGroup，一直传递到最低端的View，分别先执行ViewGroup或View的**dispatchTouchEvent(),onInterceptTouchEvent()**方法，再往下传递
* 事件处理则是从下往上，执行onTouchEvent()方法

* 事件传递的返回值：True，就是拦截，不再往下传递；False，则是不拦截，继续传递。这个返回值是指onInterceptTouchEvent()方法的返回值
* 事件处理的返回值：True，处理了，不用审核，也就是不用往上汇报了；False，则是往上汇报，交给上级处理；
* 初始情况下，都是False


###总结
刚刚接触有关View，ViewGroup的测量、绘制，感觉第一次看，真的很难理解，难怪自定义View会让我们这些初学者觉得是很厉害。现在只是对自定义控件的过程，实现方式有个总体了解，要加深理解还是需要多用，也就是需要真正在需要实现自定义控件的时候，才能加深理解，毕竟实践出真知！
