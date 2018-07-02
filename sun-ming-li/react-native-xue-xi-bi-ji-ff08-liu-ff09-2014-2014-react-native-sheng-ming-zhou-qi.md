上一篇文章介绍了 “ React Native的ES5、ES6写法对照”，使用ES5编写的RN代码的生命周期和使用ES6编写的代码的生命周期，稍有不同。在这篇文章中[https://www.race604.com/react-native-component-lifecycle/](https://www.race604.com/react-native-component-lifecycle/)介绍了使用ES5语法编写的RN代码的生命周期。本文介绍一下使用ES6编写的代码的生命周期。

#### 一、 Mounting 只调用一次

1、定义一个组件的默认属性 static defaultProps

2、组件初始化constructor\(props\)

3、组件渲染前 componentWillMount

4、组件渲染 render

5、组件渲染完成 componentDidMount

#### 二、运行中 \(updating\) 多次调用

1、属性\(props\)改变 componentWillReceiveProps -&gt; shouldComponentUpdate

2、是否渲染组件 shouldComponentUpdate -&gt;

true -&gt; componentWillUpdate-&gt; render -&gt; componentDidUpdate

false -&gt; 运行中

3、状态\(state\)改变 -&gt; shouldComponentUpdate

#### 三、卸载\(Unmount\) 只调用一次

componentWillUnmount

#### 四、在各个生命周期建议做的事

constructor\(\)方法里初始化state  
componentDidMount\(\)方法里跑网/耗时操作  
componentWillMount\(\)可在方法里对state进行最后的修改

注意，不要在 constructor 或者 render 里 setState\(\)，這是因为 constructor 已含 this.state={} ，而 render 里 setState 会造成setState -&gt; render -&gt; setState -&gt; render  
能做的setState，只要是render前，就放在componentWillMount，render后，就放在 componentDidMount。這两个 function 是 react lifecycle 中，最常使用的两个。

![](/assets/20180611071653943-2.png)



参考文章：

[https://blog.csdn.net/weixin\_38883944/article/details/80644988](https://blog.csdn.net/weixin_38883944/article/details/80644988)

[https://blog.csdn.net/ddwhan0123/article/details/78490884](https://blog.csdn.net/ddwhan0123/article/details/78490884)

