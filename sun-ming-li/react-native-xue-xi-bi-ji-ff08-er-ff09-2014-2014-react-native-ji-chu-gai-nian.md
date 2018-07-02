上
一篇学习笔记介绍了React Native的环境安装以及创建第一个RN应用。

这一篇学习笔记主要介绍了React Native的基础概念：语法（JSX）、组件（components）、状态（state）、属性（props）。

### 1.语法（JSX）

React的代码其实就是将html和JS结合起来使用，它所使用的语法的名字就叫JSX。

来看下React Native的代码

上篇笔记中提到过，JSX通过{}大括号来使用JS变量。

如果是iOS开发工程师可能对JS不太熟悉，学习资料\[ECMAScript6入门\]\(http://es6.ruanyifeng.com/\)

```
const instructions = Platform.select({
ios: 'Press Cmd+R to reload,\n' +
'Cmd+D or shake for dev menu',
android: 'Double tap R on your keyboard to reload,\n' +
'Shake or press menu button for dev menu',
});

type Props = {};
export default class App extends Component<Props> {
render() {
return (
<View style={styles.container}>
<Text style={styles.instructions}>
{instructions}
</Text>
</View>
);
}
}
```

###  2.组件\(componets\)

a. React Native跟React不同，React直接使用的是 web组件，React Native是使用的是iOS或者Andriod的原生组件。在\[http://facebook.github.io/react-native/docs/getting-started.html\]\(http://facebook.github.io/react-native/docs/getting-started.html\)中的Components章节，我们可以查询到React native提供的组件，例如Button中就包含了如下内容：

（1）首先介绍了UIButton适用的平台：Button适用于ios和安卓平台

A basic button component that should render nicely on any platform. 

有一些控件只适用于iOS平台或者安卓平台，如DatePickerIOS、DrawerLayoutAndroid。

（2）接着介绍了UIButton提供的几种样式：Simple Button、Adjusted color、Fit to textlayout、Disabled Button如何使用，以及分别在ios和Android平台上呈现的样式。

（3）接着介绍了如果RN提供的Button组件不满足需要可以使用TouchableOpacity h或者TouchlableNativedFeedback自定义Button。

（4）接着介绍了使用Button的代码、属性、以及属性的赋值类型。

```
import { Button } from 'react-native';
...

<Button
onPress={onPressLearnMore}
title="Learn More"
color="#841584"
accessibilityLabel="Learn more about this purple button"
/>
```

b. 我们还可以自定义组件，例如下面代码就定义了一个 HelloWorld组件，我们可以将HelloWorld组件放置到任何需要显示Hello world！文本的地方。

```
import React, { Component } from 'react';
import {
Text
} from 'react-native';

export default class HelloWord extends Component{
render(){
return(
<Text>Hello world!</Text>
)
}
}
```

### 3.状态\(state\)

在RN中有两种数据可以控制一个控件显示不同的nr：props 和state。

props是由伏组件给自组件赋的值，在组件的生命周期中是固定的。对于变化的数据我们就要使用State了。

通常我们在构造方法（constructor）中初始化一个statte，调用setState方法来改变state的值。

假如我们需要制作一段不停闪烁的文字。文字内容本身在组件创建时就已经指定好了，所以文字内容应该是一个prop。而文字的显示或隐藏的状态（快速的显隐切换就产生了闪烁的效果）则是随着时间变化的，因此这一状态应该写到state中。代码如下：

```
import React, { Component } from 'react';
import { AppRegistry, Text, View } from 'react-native';

class Blink extends Component {
constructor(props) {
super(props);
this.state = {isShowingText: true};

// Toggle the state every second
setInterval(() => {
this.setState(previousState => {
return { isShowingText: !previousState.isShowingText };
});
}, 1000);
}

render() {
let display = this.state.isShowingText ? this.props.text : ' ';
return (
<Text>{display}</Text>
);
}
}

export default class BlinkApp extends Component {
render() {
return (
<View>
<Blink text='I love to blink' />
<Blink text='Yes blinking is so great' />
<Blink text='Why did they ever take this out of HTML' />
<Blink text='Look at me look at me look at me' />
</View>
);
}
}

// skip this line if using Create React Native App
AppRegistry.registerComponent('AwesomeProject', () => BlinkApp);

```

### 4.属性\(props\)

大多数组件在创建时可以传入不同的参数，定制为不同的显示样式。创建时可以进行赋值的参数称为属性。

a. 例如RN的基础组件Image，当我们创建一个image组件时，我们可以使用source属性控制显示那张图片

```
import React, { Component } from 'react';
import { AppRegistry, Image } from 'react-native';

export default class Bananas extends Component {
render() {
let pic = {
uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
};
return (
<Image source={pic} style={{width: 193, height: 110}}/>
);
}
}

// skip this line if using Create React Native App
AppRegistry.registerComponent('AwesomeProject', () => Bananas);
```

因为pic是js语法，所有要放到{}中显示.  

style={{width:193, height:110}}两个{}含义是里面的{width:193, height:110}定义了一个省略名称的style，外面的{}使用了这个省略名称的style。

b. 我们也可以为自定义的组件添加属性，例如上面的HelloWorldApp组件，只是固定显示Hello World！如果我们给这个组件添加一个属性 name，当我们在父组件中为HelloWorldApp组件的name属性赋值为“小明”时，就可以显示成Hello 小明！

代码如下，使用和赋值一个属性就相当于定义了以props，就是这么简单，但是请注意props不可以被修改，是不可变变量。可变的变量就要使用state了。

```
import React, { Component } from 'react';
import { AppRegistry, Text, View } from 'react-native';

class Greeting extends Component {
render() {
return (
<Text>Hello {this.props.name}!</Text>
);
}
}

export default class LotsOfGreetings extends Component {
render() {
return (
<View style={{alignItems: 'center'}}>
<Greeting name='Rexxar' />
<Greeting name='Jaina' />
<Greeting name='Valeera' />
</View>
);
}
}

// skip this line if using Create React Native App
AppRegistry.registerComponent('AwesomeProject', () => LotsOfGreetings);

```




```
const instructions = Platform.select({
  ios: 'Press Cmd+R to reload,\n' +
    'Cmd+D or shake for dev menu',
  android: 'Double tap R on your keyboard to reload,\n' +
    'Shake or press menu button for dev menu',
});

type Props = {};
export default class App extends Component<Props> {
  render() {
    return (
      <View style={styles.container}
        <Text style={styles.instructions}>
          {instructions}
        </Text>
      </View>
    );
  }
}
```

### 2.组件\(componets\)

a. React Native跟React不同，React直接使用的是 web组件，React Native是使用的是iOS或者Andriod的原生组件。在[http://facebook.github.io/react-native/docs/getting-started.html](http://facebook.github.io/react-native/docs/getting-started.html)中的Components章节，我们可以查询到React native提供的组件，例如Button中就包含了如下内容：

（1）首先介绍了UIButton适用的平台：Button适用于ios和安卓平台

A basic button component that should render nicely on any platform. 

有一些控件只适用于iOS平台或者安卓平台，如DatePickerIOS、DrawerLayoutAndroid。

（2）接着介绍了UIButton提供的几种样式：Simple Button、Adjusted color、Fit to textlayout、Disabled Button如何使用，以及分别在ios和Android平台上呈现的样式。

（3）接着介绍了如果RN提供的Button组件不满足需要可以使用TouchableOpacity h或者TouchlableNativedFeedback自定义Button。

（4）接着介绍了使用Button的代码、属性、以及属性的赋值类型。

```
import { Button } from 'react-native';
...

<Button
  onPress={onPressLearnMore}
  title="Learn More"
  color="#841584"
  accessibilityLabel="Learn more about this purple button"
/>
```

b. 我们还可以自定义组件，例如下面代码就定义了一个 HelloWorld组件，我们可以将HelloWorld组件放置到任何需要显示Hello world！文本的地方。

```
import React, { Component } from 'react';
import {
    Text
} from 'react-native';

export default class HelloWord extends Component{
    render(){
        return(
            <Text>Hello world!</Text>
        )
    }
}
```

### 3.状态\(state\)

在RN中有两种数据可以控制一个控件显示不同的nr：props 和state。

props是由伏组件给自组件赋的值，在组件的生命周期中是固定的。对于变化的数据我们就要使用State了。

通常我们在构造方法（constructor）中初始化一个statte，调用setState方法来改变state的值。

假如我们需要制作一段不停闪烁的文字。文字内容本身在组件创建时就已经指定好了，所以文字内容应该是一个prop。而文字的显示或隐藏的状态（快速的显隐切换就产生了闪烁的效果）则是随着时间变化的，因此这一状态应该写到state中。代码如下：

```
import React, { Component } from 'react';
import { AppRegistry, Text, View } from 'react-native';

class Blink extends Component {
  constructor(props) {
    super(props);
    this.state = {isShowingText: true};

    // Toggle the state every second
    setInterval(() => {
      this.setState(previousState => {
        return { isShowingText: !previousState.isShowingText };
      });
    }, 1000);
  }

  render() {
    let display = this.state.isShowingText ? this.props.text : ' ';
    return (
      <Text>{display}</Text>
    );
  }
}

export default class BlinkApp extends Component {
  render() {
    return (
      <View>
        <Blink text='I love to blink' />
        <Blink text='Yes blinking is so great' />
        <Blink text='Why did they ever take this out of HTML' />
        <Blink text='Look at me look at me look at me' />
      </View>
    );
  }
}

// skip this line if using Create React Native App
AppRegistry.registerComponent('AwesomeProject', () => BlinkApp);
```

### 4.属性\(props\)

大多数组件在创建时可以传入不同的参数，定制为不同的显示样式。创建时可以进行赋值的参数称为属性。

a. 例如RN的基础组件Image，当我们创建一个image组件时，我们可以使用source属性控制显示那张图片

```
import React, { Component } from 'react';
import { AppRegistry, Image } from 'react-native';

export default class Bananas extends Component {
  render() {
    let pic = {
      uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'
    };
    return (
      <Image source={pic} style={{width: 193, height: 110}}/>
    );
  }
}

// skip this line if using Create React Native App
AppRegistry.registerComponent('AwesomeProject', () => Bananas);
```

因为pic是js语法，所有要放到{}中显示.  

style={{width:193, height:110}}两个{}含义是里面的{width:193, height:110}定义了一个省略名称的style，外面的{}使用了这个省略名称的style。

b. 我们也可以为自定义的组件添加属性，例如上面的HelloWorldApp组件，只是固定显示Hello World！如果我们给这个组件添加一个属性 name，当我们在父组件中为HelloWorldApp组件的name属性赋值为“小明”时，就可以显示成Hello 小明！

代码如下，使用和赋值一个属性就相当于定义了以props，就是这么简单，但是请注意props不可以被修改，是不可变变量。可变的变量就要使用state了。

```
import React, { Component } from 'react';
import { AppRegistry, Text, View } from 'react-native';

class Greeting extends Component {
  render() {
    return (
      <Text>Hello {this.props.name}!</Text>
    );
  }
}

export default class LotsOfGreetings extends Component {
  render() {
    return (
      <View style={{alignItems: 'center'}}>
        <Greeting name='Rexxar' />
        <Greeting name='Jaina' />
        <Greeting name='Valeera' />
      </View>
    );
  }
}

// skip this line if using Create React Native App
AppRegistry.registerComponent('AwesomeProject', () => LotsOfGreetings);

```



