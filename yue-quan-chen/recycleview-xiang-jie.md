#### 与 ListView 对比

对比项 | AbsListView | RecyclerView
--------|----------|--------
定向刷新 | 不支持 | 支持
局部刷新 | 不支持 | 支持
刷新动画 | 不支持 | 支持
Item点击 | 支持 | 不支持
分隔线 | 样式单一 | 自定义样式
布局方式 | 列表/网格 | 自定义样式
头尾添加 | 支持 | 不支持

#### ListView实现局部刷新

通过ListView的getChildAt()来获得需要更新的View，然后通过getTag()获得ViewHolder，从而实现更新。

![](/assets/listview_local_refresh.jpeg)

#### RecyclerView

RecyclerView将ListView中getView()的功能拆分成了onCreateViewHolder()和onBindViewHolder()。