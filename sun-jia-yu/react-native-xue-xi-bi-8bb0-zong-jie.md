##总结
1.Props（属性）

>大多数组件在创建的时候就可以使用各种参数来进行定制，用于定制的这些参数就叫做 props(属性);在使用时可以定制各种工程中需要用到的参数，通过以下方法获取相关参数的值：
this.props.参数名
注意：props是在父组件中指定，一经指定，在被指定的   组件的生命周期中就不能就改变了，简而言之，props的参数被指定了就不能在修改了

2.State(状态)

>定制一个组件的时候还可以使用 state,state中定制的参数的值是可以改变的，在ES5中使用：
<pre>
//申明属性
getInitialState{
return{
key_word: ''test"
}
}
//使用 this.state.参数名
//修改参数的值：
this.setState({
key_word: "texts"
})
</code>
中申明属性，使用的时候调用，修改属性值的时候调用
</pre>


3 .StyleSheet的使用

>在RN中任然使用JS来书写样式，所有核心组件都接受Style的属性，样式名采用的是 <strong>驼峰标识符</strong>,由于开发中实际情况比较复杂，使用<strong>StyleSheet.create</strong>来集体定义组件的样式：
<pre>
const styles = StyleSheet.create({
container: {
flex: 1,
justifyContent: 'center',
alignItems: 'center',
backgroundColor: '#F5FCFF',
},

>welcome: {
fontSize: 20,
textAlign: 'center',
margin: 10
},

>instructions: {
textAlign: 'center',
color: '#333333',
marginBottom: 5
}
});
</code>
</pre>


4.Flexbox布局

>在组件样式中使用Flex可以是组件在可利用的空间中动态的扩张或收缩，一般而言使用<em> flex:1 </em>来指定某个组件扩张来撑满剩余的空间（有点在之间组件中全屏的意思），如果有多个并列的子组件使用了flex:1，则这些子组件会平分父容器中剩余的空间。如果这些并列的子组件的flex值不一样，则谁的值更大，谁占据剩余空间的比例就更大（即占据剩余空间的比等于并列组件间flex值的比）。
Flexbox可以在不同屏幕尺寸上提供一致的布局结构，一般来说，使用flexDirection 、alignItems和justifyContent三个样式属性就已经能满足大多数布局需求。
<pre>
flexDirection：决定布局的主轴 
flexDirection: 'column' //主轴方向为竖直
flexDirection: 'row' //主轴方向为水平 <br />
alignItems: 决定侧轴的方向
alignItems：'flex-start' //从组件开始的位置开始排布
alignItems：'center' //居中
alignItems：'flex-end' //从组件结束的位置排布
alignItems：stretch' //如果组件为设置高度或为auto,将占满整个容器的高度,要使stretch选项生效的话，子元素在次轴方向上不能有固定的尺寸 <br />
justifyContent: 定义组件在主轴方向上的对齐方式
justifyContent: 'flex-start' //伸缩项目向一行的起始位 置靠齐
justifyContent: 'flex-end' //伸缩项目向一行的结束位置靠齐
justifyContent: 'center' //伸缩项目向一行的中间位置靠齐
justifyContent: 'space-between' //伸缩项目向两端对齐
justifyContent: 'space-around' //伸缩项目向平均分布在行里，两端保持一半的空间






