同一个个recyclerview加载不同分割线：

    在recyclerview上可以显示不同种类的数据，我们可以轻易的将它区分开。如果要做成同一种数据用同种分割线，不同种类的数据用更粗一点的分割线分开，如果做到？
    这里我们需要复写RecyclerView.ItemDecoration；
```java
    public class DifferentDataDividerItemDecoration extends RecyclerView.ItemDecoration {


    public final int[] ATTRS = new int[]{android.R.attr.listDivider}; // 线性列表 方向
    public static final int HORIZONTAL_LIST = LinearLayoutManager.HORIZONTAL;
    public static final int VERTICAL_LIST = LinearLayoutManager.VERTICAL;
    private Drawable mDivider;
    private int mOrientation;
    private Paint mPaint;
    private int mDividerHeight = 2;


    /**
     * 设置不同数据区域的分割线 会额外变宽，在这里设置要变宽的位置
     */
    private int needHeighterPosition = -1;

    public int getNeedHeighterPosition() {
        return needHeighterPosition;
    }

    public void setNeedHeighterPosition(int needHeighterPosition) {
        this.needHeighterPosition = needHeighterPosition;
    }

    /**
     * 默认样式分割线 * 宽度为2 颜色为灰色 * * @param context * @param orientation
     */
    public DifferentDataDividerItemDecoration(Context context, int orientation) {
        final TypedArray a = context.obtainStyledAttributes(ATTRS);
        mDivider = a.getDrawable(0);
        a.recycle();
        setOrientation(orientation);
    }

    /**
     * 自定义分割线 * * @param context * @param orientation 列表方向 * @param drawableId 分割线图片
     */
    public DifferentDataDividerItemDecoration(Context context, int orientation, int drawableId) {
        this(context, orientation);
        mDivider = ContextCompat.getDrawable(context, drawableId);
        mDividerHeight = mDivider.getIntrinsicHeight();
    }

    /**
     * 自定义分割线 * * @param context * @param orientation 列表方向 * @param dividerHeight 分割线高度 * @param dividerColor 分割线颜色
     */
    public DifferentDataDividerItemDecoration(Context context, int orientation, int dividerHeight, int dividerColor) {
        this(context, orientation);
        mDividerHeight = dividerHeight;
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(context.getResources().getColor(dividerColor));
        mPaint.setStyle(Paint.Style.FILL);
    }

    public void setOrientation(int orientation) {
        if (orientation != HORIZONTAL_LIST && orientation != VERTICAL_LIST) {
            throw new IllegalArgumentException("invalid orientation");
        }
        mOrientation = orientation;
    }

    @Override
    public void onDraw(Canvas c, RecyclerView parent) {

        if (mOrientation == VERTICAL_LIST) {
            drawVertical(c, parent);
        } else {
            drawHorizontal(c, parent);
        }
    }

    @Override
    public void onDraw(Canvas c, RecyclerView parent, RecyclerView.State state) {
        super.onDraw(c, parent, state);
    } // 绘制垂直排列的分割线

    public void drawVertical(Canvas c, RecyclerView parent) {
        final int left = parent.getPaddingLeft();
        final int right = parent.getWidth() - parent.getPaddingRight();
        final int childCount = parent.getChildCount();
        for (int i = 0; i < childCount; i++) {
            final View child = parent.getChildAt(i);
            RecyclerView v = new RecyclerView(parent.getContext());
            final RecyclerView.LayoutParams params = (RecyclerView.LayoutParams) child.getLayoutParams();
            final int top = child.getBottom() + params.bottomMargin;
            final int bottom = top + mDividerHeight;
            if (mDivider != null) {
                mDivider.setBounds(left, top, right, bottom);
                mDivider.draw(c);
            }
            if (mPaint != null) {
                c.drawRect(left, top, right, bottom, mPaint);
            }
        }
    }

    // 绘制水平排列的分割线
    public void drawHorizontal(Canvas c, RecyclerView parent) {
        final int top = parent.getPaddingTop();
        final int bottom = parent.getHeight() - parent.getPaddingBottom();
        final int childCount = parent.getChildCount();
        for (int i = 0; i < childCount; i++) {
            final View child = parent.getChildAt(i);
            final RecyclerView.LayoutParams params = (RecyclerView.LayoutParams) child.getLayoutParams();
            final int left = child.getRight() + params.rightMargin;
            final int right = left + mDividerHeight;
            if (mDivider != null) {
                mDivider.setBounds(left, top, right, bottom);
                mDivider.draw(c);
            }
            if (mPaint != null) {
                c.drawRect(left, top, right, bottom, mPaint);
            }
        }
    }

    @Override
    public void getItemOffsets(Rect outRect, int itemPosition, RecyclerView parent) {
        if (mOrientation == VERTICAL_LIST) {

            if(needHeighterPosition!=-1){
                if (itemPosition == needHeighterPosition) {//设置在什么位置用更粗的分割线分开
                    outRect.set(0, 0, 0, 30);//这个位置的分割线会变粗
                } else {
                    outRect.set(0, 0, 0, mDividerHeight);
                }

            }else {
                outRect.set(0, 0, 0, mDividerHeight);
            }

        } else {
            outRect.set(0, 0, mDividerHeight, 0);
        }
    }


}
```

使用的时候

```abz
 mDecoration = new DifferentDataDividerItemDecoration(getActivity(), DividerItemDecoration.VERTICAL_LIST);
    mDecoration.setNeedHeighterPosition(2);//如果这里写2，第三个条目的分割线会啊变粗，可以根据需求定制位置
    
                mRv.addItemDecoration(mDecoration);
 
```




