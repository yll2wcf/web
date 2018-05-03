## 常用命令

- 创建工程：react-native init 项目名称
- 查看RN本地版本：react-native --version
- 更新RN本地版本：npm update -g react-native-cli
- 查询RN的npm包最新版本：npm info react-native
- 升级或者降级npm包的版本：npm install --save react-native@0.18
- 更新项目templates文件：react-native upgrade

## 常用控件的常用属性

#### View

- Flexbox 弹性布局
- Transforms  动画属性
- backfaceVisibility enum('visible', 'hidden')    定义界面翻转的时候是否可见
- opacity number 设置透明度，取值从0-1；
- overflow enum('visible', 'hidden')  设置内容超出容器部分是显示还是隐藏；
- elevation number 高度   设置Z轴，可产生立体效果。

#### Flexbox 容器属性

- flexDirection 该属性决定主轴的方向（即项目的排列方向）。
	- row：主轴为水平方向，起点在左端。
    - row-reverse：主轴为水平方向，起点在右端。
    - column(默认值)：主轴为垂直方向，起点在上沿。
    - column-reverse：主轴为垂直方向，起点在下沿。
- justifyContent 定义了伸缩项目在主轴线的对齐方式
	- flex-start(默认值)：伸缩项目向一行的起始位置靠齐。
	- flex-end：伸缩项目向一行的结束位置靠齐。
	- center：伸缩项目向一行的中间位置靠齐。
	- space-between：两端对齐，项目之间的间隔都相等。
	- space-around：伸缩项目会平均地分布在行里，两端保留一半的空间。
- alignItems 定义项目在交叉轴上如何对齐，可以把其想像成侧轴（垂直于主轴）的“对齐方式”
	- flex-start：交叉轴的起点对齐。
    - flex-end：交叉轴的终点对齐 。
    - center：交叉轴的中点对齐。
    - baseline：项目的第一行文字的基线对齐。
    - stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
- flexWrap 换行
	- nowrap(默认值)：不换行
	- wrap：换行，第一行在上方
	- wrap-reverse：换行，第一行在下方。（和wrap相反）

#### Flexbox 元素属性

- flex 宽度权重
- alignSelf 允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。
	- auto 
	- flex-start
	- flex-end
	- center 
	- baseline 
	- stretch