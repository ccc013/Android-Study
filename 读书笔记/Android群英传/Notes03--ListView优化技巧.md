##第4章 ListView使用技巧
###1. ListView常用优化技巧
#####(1) 使用ViewHolder模式提高效率
* ViewHolder模式是充分利用了ListView的视图缓存机制，避免了每次在调用getView()的时候都去通过findViewById()实例化控件。
* 使用ViewHolder模式来优化ListView，只需要在自定义Adapter中定义一个内部类ViewHolder，并将布局中的控件作为成员变量。

代码如下所示:

    @Override
    public View getView(int position, View contentView, ViewGroup parent){
        ViewHolder holder == null;
        // 判断是非缓存
        if(contentView == null){
            holder = new ViewHolder();
            // 通过LayoutInflater实例化布局
            contentView = mInflater.inflate(R.layout.viewholder_item, null);
            holder.img = (ImageView) contentView.findViewById(R.id.imageView);
            holder.title = (TextView) contentView.findViewById(R.id.textView);
            contentView.setTag(holder);
        }else{
            // 通过tag找到缓存的布局
            holder = (ViewHolder)contentView.getTag();
        }
        // 设置布局中控件要显示的视图
        holder.img.setBackgroundResource(R.drawable.ic_laucher);
        holder.title.setText(mData.get(position));
        return contentView;
    }

    public final class ViewHolder{
        public ImageView img;
        public TextView title;
    }

#####(2) 设置项目间分割线
ListView的各个项目之间可以通过设置分割线来进行区分，系统提供了divider和dividerHeight两个属性来帮助我们实现这个功能。使用代码如下:

    android:divider="@android:color/darker_gray";
    android:dividerHeight="10dp";
    // 这样设置，分割线就是透明的
    android:divider="@null";

#####(3) 隐藏滚动条
默认的ListView在滚动时，在右边会显示滚动条，指示当前滑动的位置，可以通过设置scrollbars属性来控制滚动条状态。

    // 如此设置就会隐藏滚动条
    android:scrollbars="none";

#####(4) 取消ListView的Item点击效果
    // 第一种方法
    android:listSelector="#00000000";
    // 第二种方法
    android:listSelector="@android:color/transparent";

#####(5) 设置ListView需要显示在第几项
    // 设置显示在第n个item
    listView.setSelection(n);
上述方法类似于scrollTo，是瞬间完成的移动，除此之外，还可以使用如下代码实现平滑移动:

    mListView.smoothScrollBy(distance, duration);
    mListView.smoothScrollByOffset(offset);
    mListView.smoothScrollToPosition(index);

#####(6) 动态修改ListView
    // 代码例子
    mData.add("new");
    mAdapter.notifyDataSetChanged();
在修改了数据后，调用adapter的notifyDataSetChanged()方法即可，但需注意的是保证传进Adapter的数据List是同一个List而不能是其他对象，否则将无法实现这个效果。

#####(7) 遍历ListView中的所有Item
使用ListView的getChildAt()方法来获取第i个子View

#####(8) 处理空ListView
当列表中无任何数据时，ListView提供了一个方法——setEmptyView()，通过这个方法可以设置一个在空数据下显示的默认提示，而在有数据时就不会显示。

#####(9) ListView滑动监听
这里介绍两种监听ListView滑动事件的方法，一个是通过OnTouchListener,一个是使用OnScrollListener来实现监听。

* OnTouchListener是View中的监听事件，通过监听ACTION_DOWN、ACTION_MOVE、ACTION_UP这三个事件发生时的坐标，就可以根据坐标判断用户滑动的方向，并在不同的事件中进行相应的逻辑处理

* OnScrollListener是AbsListView中的监听事件，它有两个回调方法--onScrollStateChanged()和onScroll()，这里的第一个方法onScrollStateChanged(AbsListView view, int scrollState)是根据它的参数scrollState来决定它回调的次数，scrollState有以下三种模式：

  * OnScrollListener.SCROLL_STATE_IDLE：滚动停止时
  * OnScrollListener.SCROLL_STATE_TOUCH_SCROLL：正在滚动时
  * OnScrollListener.SCROLL_STATE_FLING：手指抛动时，即手指用力滑动，在离开后ListView由于惯性继续滑动的状态

当用户没有做手指抛动时，该方法只会回调2次，否则是3次。

第二个方法onScroll(AbsListView view, int firstVisibleItem, int visibleItemCount, int totalItemCount)在ListView滚动时一直回调，后面三个int参数可以非常精确地显示了当前ListView滚动的状态，这三个参数如下所示：

* firstVisibleItem：当前能看见的第一个Item的ID（从0开始）
* visibleItemCount：当前能看见的Item总数(包括只是显示了一小半的Item)
* totalItemCount：整个ListView的Item总数

###2. ListView 常用拓展
常用的拓展有以下几种：

* 具有弹性的ListView
* 自动显示、隐藏布局的ListView
* 聊天ListView
* 动态改变ListView布局，两种方法，一是将两种布局写在一起，通过控制布局的显示、隐藏，来达到切换布局的效果；另一种是在getView()的时候，通过判断来选择加载不同的布局



