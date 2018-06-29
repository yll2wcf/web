##react-native的生命周期
 

生命周期的三个阶段 

>第一阶段：是组件第一次绘制阶段，如图中的上面虚线框内，在这里完成了组件的加载和初始化； 
第二阶段：是组件在运行和交互阶段，如图中左下角虚线框，这个阶段组件可以处理用户 
交互，或者接收事件更新界面； 
第三阶段：是组件卸载消亡的阶段，如图中右下角的虚线框中，这里做一些组件的清理工作 

生命周期中一些重要方法 
>1、getInitialState 
该函数用于对组件的一些状态进行初始化，该状态是随时变化的（也就是说该函数会被多次调用），比如ListView的datasource,rowData等等，同样的，可以通过this.state.XXX取该属性值，同时可以对该值进行修改，通过this.setState修改，es6中将属性的初始化放到了构造函数中。 
2、render 
该函数组件必有的，通过返回JSX或其他组件来构成DOM,换言之，就是组件的核心渲染过程。 
3、componentDidMount 
在组件第一次绘制之后，会调用componentDidMount，通知组件已经加载完成。 
4、shouldComponentUpdate 
当组件接收到新的属性和状态改变的话，都会触发调用shouldComponentUpdate
