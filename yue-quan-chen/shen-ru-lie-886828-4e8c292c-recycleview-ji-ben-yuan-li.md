## 整体结构

![](/assets/RecyclerView.png)

内部类

- RecyclerView.LayoutManager 负责Item视图的布局的显示管理
- RecyclerView.ItemDecoration 给每一项Item视图添加修饰的View,
- RecyclerView.Adapter 为每一项Item创建视图
- RecyclerView.ViewHolder 承载Item视图的子布局
- RecyclerView.ItemAnimator 负责处理数据添加或者删除时候的动画效果
- RecyclerView.Cache Recycler/RecycledViewPool/ViewCacheExtension

最后一个Cache是我加上去的，实际上并没有这个类，主要是为了说明RecyclerView对Item强大的缓存

## Observer&Observable

Recycler没有采用系统的Observer，因为自己需要接收通知的方法过多，所以自己设计了一个观察者AdapterDataObserver

![](/assets/AdapterDataObserver.png)

## Recycler

- 在Recycler中实际上缓存VieHolder的有2类集合，一类是可见的ViewHolder数组，一类是不可见的ViewHolder数组，其中可见的数组中又分为数据改变跟没有改变的
- 跟ListView的RecycleBin一样，Recycler也是RecyclerView设计的一个专门用于回收ViewHolder的类

## 缓存

#### 1.缓存与复用的原理

RecyclerView 的内部维护了一个二级缓存，滑出界面的 ViewHolder 会暂时放到 cache 结构中，而从 cache 结构中移除的 ViewHolder，则会放到一个叫做 RecycledViewPool 的循环缓存池中。

RecycledView 的性能并不比 ListView 要好多少，它最大的优势在于其扩展性。但是有一点，在 RecycledView 内部的这个第二级缓存池 RecycledViewPool 是可以被多个 RecyclerView 共用的，这一点比起直接缓存 View 的 ListView 就要高明了很多，但也正是因为需要被多个 RecyclerView 公用，所以我们的 ViewHolder 必须继承自同一个基类(即RecyclerView.ViewHolder)。

在 ViewPool 里的 ViewHolder 都是全新的 ViewHolder ，只要 type 一样，有找到，就可以拿出来复用，但是需要触发 onBindViewHolder() 重新绑定下数据。mCachedViews 中的 ViewHolder 则可以直接拿来用

默认的情况下，cache 缓存 2 个 holder，RecycledViewPool 缓存 5 个 holder。对于二级缓存池中的 holder 对象，会根据 viewType 进行分类，不同类型的 viewType 之间互不影响。

#### 2.RecyclerView 缓存(如果算上创建的那一次，应该是四级了)

第一级缓存：
就是上面的一系列 mCachedViews。如果仍依赖于 RecyclerView （比如已经滑动出可视范围，但还没有被移除掉），但已经被标记移除的 ItemView 集合会被添加到 mAttachedScrap 中。然后如果 mAttachedScrap 中不再依赖时会被加入到 mCachedViews 中。 mChangedScrap 则是存储 notifXXX 方法时需要改变的 ViewHolder 。

第二级缓存：
ViewCacheExtension 是一个抽象静态类，用于充当附加的缓存池，当 RecyclerView 从第一级缓存找不到需要的 View 时，将会从 ViewCacheExtension 中找。不过这个缓存是由开发者维护的，如果没有设置它，则不会启用。通常我们也不会去设置他，系统已经预先提供了两级缓存了，除非有特殊需求，比如要在调用系统的缓存池之前，返回一个特定的视图，才会用到他。

第三级缓存：
最强大的缓存器。之前讲了，与 ListView 直接缓存 ItemView 不同，从上面代码里我们也能看到，RecyclerView 缓存的是 ViewHolder。而 ViewHolder 里面包含了一个 View 这也就是为什么在写 Adapter 的时候 必须继承一个固定的 ViewHolder 的原因。首先来看一下 RecycledViewPool，是通过一个默认为 5 大小的 ArrayList 实现的,然后每一个 ArrayList 又都是放在一个 Map 里面的，SparseArray 这个类我们在讲性能优化的时候已经多次提到了，就是两个数组，用来替代 Map 的。把所有的 ArrayList 放在一个 Map 里面，这也是 RecyclerView 最大的亮点，这样根据 itemType 来取不同的缓存 Holder，每一个 Holder 都有对应的缓存，而只需要为这些不同 RecyclerView 设置同一个 Pool 就可以了。

#### 3.总结

RecyclerView 滑动场景下的回收复用涉及到的结构体两个：mCachedViews 和 RecyclerViewPool。mCachedViews 优先级高于 RecyclerViewPool，回收时，最新的 ViewHolder 都是往 mCachedViews 里放，如果它满了，那就移出一个扔到 ViewPool 里好空出位置来缓存最新的 ViewHolder。复用时，也是先到 mCachedViews 里找 ViewHolder，但需要各种匹配条件，概括一下就是只有原来位置的卡位可以复用存在 mCachedViews 里的 ViewHolder，如果 mCachedViews 里没有，那么才去 ViewPool 里找。

在 ViewPool 里的 ViewHolder 都是全新的 ViewHolder ，只要 type 一样，有找到，就可以拿出来复用，但是需要触发 onBindViewHolder() 重新绑定下数据。mCachedViews 中的 ViewHolder 则可以直接拿来用

默认的情况下，cache 缓存 2 个 holder，RecycledViewPool 缓存 5 个 holder。

与 ListView 直接缓存 ItemView 不同，RecyclerView 缓存的是 ViewHolder。

ListView处理回收复用的类是RecycleBin一样，RecyclerView 用于回收ViewHolder的类是Recycler