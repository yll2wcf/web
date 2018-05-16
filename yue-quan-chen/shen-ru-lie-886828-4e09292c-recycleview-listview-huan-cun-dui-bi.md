## 缓存机制

#### 1.层级不同

ListView(两级缓存)：

![](/assets/listview_cache.png)

RecyclerView(四级缓存)：

![](/assets/recycleview_cache.jpg)

ListView和RecyclerView缓存机制基本一致：

1). mActiveViews和mAttachedScrap功能相似，意义在于快速重用屏幕上可见的列表项ItemView，而不需要重新createView和bindView；

2). mScrapView和mCachedViews + mReyclerViewPool功能相似，意义在于缓存离开屏幕的ItemView，目的是让即将进入屏幕的ItemView重用.

3). RecyclerView的优势在于a.mCacheViews的使用，可以做到屏幕外的列表项ItemView进入屏幕内时也无须bindView快速重用；b.mRecyclerPool可以供多个RecyclerView共同使用，在特定场景下，如viewpaper+多个列表页下有优势.客观来说，RecyclerView在特定场景下对ListView的缓存机制做了补强和完善。

#### 2. 缓存不同

1). RecyclerView缓存RecyclerView.ViewHolder，抽象可理解为：
View + ViewHolder(避免每次createView时调用findViewById) + flag(标识状态)；

2). ListView缓存View。

缓存不同，二者在缓存的使用上也略有差别，具体来说：
ListView获取缓存的流程：

![](/assets/listview_get_cache.png)

RecyclerView获取缓存的流程：

![](/assets/recycleview_get_cache.png)

1). RecyclerView中mCacheViews(屏幕外)获取缓存时，是通过匹配pos获取目标位置的缓存，这样做的好处是，当数据源数据不变的情况下，无须重新bindView.而同样是离屏缓存，ListView从mScrapViews根据pos获取相应的缓存，但是并没有直接使用，而是重新getView（即必定会重新bindView）

2). ListView中通过pos获取的是view，即pos-->view；
RecyclerView中通过pos获取的是viewholder，即pos --> (view，viewHolder，flag)；
从流程图中可以看出，标志flag的作用是判断view是否需要重新bindView，这也是RecyclerView实现局部刷新的一个核心。

## 总结

RecyclerView比ListView多两级缓存，支持多个离ItemView缓存，支持开发者自定义缓存处理逻辑，支持所有RecyclerView共用同一个RecyclerViewPool(缓存池)。

ListView和RecyclerView最大的区别在于数据源改变时的缓存的处理逻辑，ListView是"一锅端"，将所有的mActiveViews都移入了二级缓存mScrapViews，而RecyclerView则是更加灵活地对每个View修改标志位，区分是否重新bindView。

#### ListView回收机制

ListView为了保证Item View的复用，实现了一套回收机制，该回收机制的实现类是RecycleBin，他实现了两级缓存：

- View[] mActiveViews: 缓存屏幕上的View，在该缓存里的View不需要调用getView()。
- ArrayList<View>[] mScrapViews;: 每个Item Type对应一个列表作为回收站，缓存由于滚动而消失的View，此处的View如果被复用，会以参数的形式传给getView()。

#### RecyclerView回收机制

RecyclerView和ListView的回收机制非常相似，但是ListView是以View作为单位进行回收，RecyclerView是以ViewHolder作为单位进行回收。Recycler是RecyclerView回收机制的实现类，他实现了四级缓存：

- mAttachedScrap: 缓存在屏幕上的ViewHolder。
- mCachedViews: 缓存屏幕外的ViewHolder，默认为2个。ListView对于屏幕外的缓存都会调用getView()。
- mViewCacheExtensions: 需要用户定制，默认不实现。
- mRecyclerPool: 缓存池，多个RecyclerView共用。

