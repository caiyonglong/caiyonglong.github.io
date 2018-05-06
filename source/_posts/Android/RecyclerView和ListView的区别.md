---
title: RecyclerView和ListView的区别
date: 2016-10-20 16:21:17
tags: [基础]
toc: true 文章目录
categories: Android
---
# RecyclerView是什么?
>RecyclerView is a more advanced and flexible version of ListView. This 
widget is a container for large sets of views that can be recycled and 
scrolled very efficiently. Use the RecyclerView widget when you have 
lists with elements that change dynamically.

>RecyclerView是ListView的更高度定制版，当你需要高效的展示大量数据时候，动态改变元素的列表的时候，就用这个。


RecyclerView是support-v7包中新组件,官方介绍RecyclerView 是 ListView 的升级版本，更加先进和灵活,是一个强大的滑动组件，与传统的ListView相比较，他同样拥有item回收复用的功能，但是RecyclerView可以直接把ViewHolder的实现封装起来，用户只要实现自己的ViewHoler就可以了。
RecyclerView新特性有：ViewHolder，ItemDecorator，LayoutManager，SmothScroller以及增加或删除item时item动画。

<!-- MORE -->
# RecyclerView 用法
```java
mRecyclerView = findViewById(R.id.recyclerview);
//设置布局管理器
mRecyclerView.setLayoutManager(layout);
//设置adapter
mRecyclerView.setAdapter(adapter)
//设置Item增加、移除动画
mRecyclerView.setItemAnimator(new DefaultItemAnimator());
//添加分割线
mRecyclerView.addItemDecoration(new DividerItemDecoration(
                getActivity(), DividerItemDecoration.HORIZONTAL_LIST));
```

> 1、将 layout 抽象成了一个 LayoutManager，RecylerView 不负责子 View 的布局， 我们可以自定义 LayoutManager 来实现不同的布局效果， 目前只提供了LinearLayoutManager。 LinearLayoutManager 可以指定方向，默认是垂直， 可以指定水平等，能支持线性布局、网格布局、瀑布流布局三种，而且同时还能够控制横向还是纵向滚动。

> 2、标准化了 ViewHolder， 编写 Adapter 面向的是 ViewHoder 而不在是View 了， 复用的逻辑被封装了， 写起来更加简单。
> 3、局部刷新
> 4、动画效果：增加或删除item时item动画

# RecyclerView 相对于ListView的优缺点
## 优点
1、它封装了viewholder的回收复用。RecyclerView.ViewHolder
2、RecyclerView使用布局管理器管理子view的位置（目前尚只提供了LinearLayoutManager），也就是说你再不用拘泥于ListView的线性展示方式，如果之后提供其他customLayoutManager的支持，你能够使用复杂的布局来展示一个动态组件。
```java
StaggeredGridLayoutManager mStaggeredGridLayoutManager =
new StaggeredGridLayoutManager(2, StaggeredGridLayoutManager.VERTICAL);
//表示两列，并且是竖直方向的瀑布流
mRecyclerView.setLayoutManager(mStaggeredGridLayoutManager);
```
在布局上支持三种StaggeredGridLayoutManager， GridLayoutManager LinearLayoutManager。
3、自带了ItemAnimation，可以设置加载和移除时的动画，方便做出各种动态浏览的效果。
4、将ListView中的getView分成了onCreateViewHolder和onBindViewHolder两个方法，更利于代码的维护。
5、相对简单 
我们看下Listview他背后的继承关系
```java
public class ListView extends AbsListView 
public abstract class AbsListView extends AdapterView<ListAdapter>
public abstract class AdapterView<T extends Adapter> extends ViewGroup
```
三重继承，内容还挺多的，不是直接继承ViewGroup，而相反的RecycleView却是直接继承自ViewGroup的。
## 缺点
1、不能简单的加头和尾。
2、不能简单的设置子item的点击事件。
    解决方案：自定义接口实现点击事件。
## 其他
ListView可以设置选择模式，并添加MultiChoiceModeListener
```java
listView.setChoiceMode(ListView.CHOICE_MODE_MULTIPLE_MODAL);
listView.setMultiChoiceModeListener(new MultiChoiceModeListener() {
    public boolean onCreateActionMode(ActionMode mode, Menu menu) { ... }
    public void onItemCheckedStateChanged(ActionMode mode, int position,
long id, boolean checked) { ... }
    public boolean onActionItemClicked(ActionMode mode, MenuItem item) {
        switch (item.getItemId()) {
            case R.id.menu_item_delete_crime:
            CrimeAdapter adapter = (CrimeAdapter)getListAdapter();
            CrimeLab crimeLab = CrimeLab.get(getActivity());
            for (int i = adapter.getCount() - 1; i >= 0; i--) {
                if (getListView().isItemChecked(i)) {
                    crimeLab.deleteCrime(adapter.getItem(i));
                }
          }
        mode.finish();
        adapter.notifyDataSetChanged();
        return true;
        default:
            return false;
}
    public boolean onPrepareActionMode(ActionMode mode, Menu menu) { ... }
    public void onDestroyActionMode(ActionMode mode) { ... }
});
```

参考链接：http://blog.csdn.net/sanjay_f/article/details/48830311