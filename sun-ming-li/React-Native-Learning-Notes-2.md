上一篇学习笔记介绍了React Native的环境安装以及创建第一个RN应用。

这一篇学习笔记主要介绍React Native的基础概念：语法\(JSX\)、组件\(componets\)、状态\(state\)、属性\(props\)。

### 1. 语法（JSX）

React的代码其实就是将html和JS结合起来使用，它所使用的语法的名字就叫JSX。

来看下React Native的代码

上篇笔记中就提到过，JSX通过{}大括号来使用JS变量。

如果是是iOS开发工程师可能对JS不太熟悉，学习资料[\[ECMAScript 6 入门\]](https://link.zhihu.com/?target=http%3A//es6.ruanyifeng.com/)。

![](/assets/1.png)

### 2.组件\(componets\)

a. React Native跟React不同，React直接使用的是 web组件，React Native是使用的是iOS或者Andriod的原生组件。在[http://facebook.github.io/react-native/docs/getting-started.html](https://link.jianshu.com/?t=http%3A%2F%2Ffacebook.github.io%2Freact-native%2Fdocs%2Fgetting-started.html)中的Components章节，我们可以查询到React native提供的组件，例如Button中就包含了如下内容：

（1）首先介绍了UIButton适用的平台：Button适用于ios和安卓平台

A basic button component that should render nicely on any platform. 

有一些控件只适用于iOS平台或者安卓平台，如DatePickerIOS、DrawerLayoutAndroid。

（2）接着介绍了UIButton提供的几种样式：Simple Button、Adjusted color、Fit to textlayout、Disabled Button如何使用，以及分别在ios和Android平台上呈现的样式。

（3）接着介绍了如果RN提供的Button组件不满足需要可以使用[TouchableOpacity](https://link.jianshu.com/?t=http%3A%2F%2Ffacebook.github.io%2Freact-native%2Fdocs%2Ftouchableopacity.html) or [TouchableNativeFeedback](https://link.jianshu.com/?t=http%3A%2F%2Ffacebook.github.io%2Freact-native%2Fdocs%2Ftouchablenativefeedback.html) 自定义Button。

（4）接着介绍了使用Button的代码、属性、以及属性的赋值类型。

![](/assets/2.png)



