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

（3）接着介绍了如果RN提供的Button组件不满足需要可以使用[TouchableOpacity](https://link.jianshu.com/?t=http%3A%2F%2Ffacebook.github.io%2Freact-native%2Fdocs%2Ftouchableopacity.html) or [TouchableNativeFeedback](https://link.jianshu.com/?t=http%3A%2F%2Ffacebook.github.io%2Freact-native%2Fdocs%2Ftouchablenativefeedback.html) 自定义Button。

（4）接着介绍了使用Button的代码、属性、以及属性的赋值类型。

![](/assets/2.png)b. 我们还可以自定义组件，例如下面代码就定义了一个 HelloWorldApp组件，我们可以将HelloWorldApp组件放置到任何需要显示Hello world！文本的地方。

![](/assets/3.png)

### 3.状态\(state\)

在RN中有两种数据可以控制一个控件显示不同的nr：props 和state。

props是由伏组件给自组件赋的值，在组件的生命周期中是固定的。对于变化的数据我们就要使用State了。

通常我们在构造方法（constructor）中初始化一个statte，调用setState方法来改变state的值。

假如我们需要制作一段不停闪烁的文字。文字内容本身在组件创建时就已经指定好了，所以文字内容应该是一个prop。而文字的显示或隐藏的状态（快速的显隐切换就产生了闪烁的效果）则是随着时间变化的，因此这一状态应该写到state中。代码如下：

![](/assets/4.png)  
啊
![](/assets/5.png)

### 4.属性\(props\)

大多数组件在创建时可以传入不同的参数，定制为不同的显示样式。创建时可以进行赋值的参数称为属性。

a. 例如RN的基础组件Image，当我们创建一个image组件时，我们可以使用source属性控制显示那张图片

![](/assets/6.png)

因为pic是js语法，所有要放到{}中显示.  

style={{width:193, height:110}}两个{}含义是里面的{width:193, height:110}定义了一个省略名称的style，外面的{}使用了这个省略名称的style。

b. 我们也可以为自定义的组件添加属性，例如上面的HelloWorldApp组件，只是固定显示Hello World！如果我们给这个组件添加一个属性 name，当我们在父组件中为HelloWorldApp组件的name属性赋值为“小明”时，就可以显示成Hello 小明！

代码如下，使用和赋值一个属性就相当于定义了以props，就是这么简单，但是请注意props不可以被修改，是不可变变量。可变的变量就要使用state了。

![](/assets/7.png)

