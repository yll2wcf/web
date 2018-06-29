##flex布局

   flex是flexible box的简称，即“弹性布局”，比传统的盒状布局更灵活。当一个元素指定为flex布局，其内元素的布局就可以遵循flex布局的规则。flex布局主要思想是：让容器有能力改变其子项目宽度、高度，以最佳的方式填充可用空间。
  
   容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置叫main start，结束位置叫main end；交叉轴的开始位置叫cross start，结束位置叫cross end。
 

1、flexDirection容器属性（方向）

>该属性决定主轴的方向（子项目的排列方向）。
row：主轴为水平方向，起点在左端
row-reverse：主轴为水平方向，起点在右端
column：主轴为垂直方向，起点在上方
column-reverse：主轴为垂直方向，起点在下方
 

2、flexWrap容器属性（换行）

>默认情况下，容器的子项目都排在一条轴线上，可是如果一条轴线排不下，需要换行呢？
nowrap（默认值）：不换行
wrap：换行，第一行在上方
wrap-reverse：换行，但第一行在下方
 

3、justifyContent属性（主轴的对齐方式）

>flex-start：左对齐
flex-end：右对齐
center：水平居中
space-between：两边分别靠左右两边，子项目之间间隔相等
 

4、alignItems属性（交叉轴的对齐方式）

>flex-start：顶部对齐
flex-end：底部对齐
center：垂直居中
stretch：若子项目为设置高度或设为auto，子项目占满整个容器
baseline：文字基线对齐
 

