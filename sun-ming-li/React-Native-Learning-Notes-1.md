RN官方网站：

中文网：[https://reactnative.cn/](https://reactnative.cn/)

英文网站：https://facebook.github.io/react-native/docs/getting-started.html

## 1.React native介绍

（1）概念：React Native简称RN是Facebook2015年发布的跨平台开发框架，它的设计理念是：使用React Native开发，既拥有Native的良好人机交互体验，又保留了React的开发效率。

（2）RN优点：

开发效率高，成本低。写一套代码可以在两个平台运行；

快速热更新，可以实现增量开发。

支持混合开发，原生代码可以和RN实现双向通讯。

## 2.搭建开发环境

https://facebook.github.io/react-native/docs/getting-started.html

参照此链接安装mac os 下ios的RN开发环境，选择Building Projects with Native Code

需要安装：

（1）[Homebrew](http://brew.sh/), Mac系统的包管理器，用于安装NodeJS和一些其他必需的工具软件。

本人安装Homebrew没有遇到问题。

 (2) Node.js

React Native目前需要NodeJS 5.0或更高版本，用来执行npm命令，安装RN依赖包。

(3)React Native的命令行工具（react-native-cli）

(4)xcode

(5)Watchman [Watchman](https://facebook.github.io/watchman/docs/install.html)是由Facebook提供的监视文件系统变更的工具。安装此工具可以提高开发时的性能（packager可以快速捕捉文件的变化从而实现实时刷新）。译注：此工具官方虽然是推荐安装，但在实践中，我们认为此工具是必须安装，否则可能无法正常开发。

(6)安装[WebStorm](https://www.jetbrains.com/webstorm/)来编写React Native应用

andriod开发需要安装JDK8、Android Studio

## 3\. 创建项目

（1）打开命令行执行react-native init TShop -version 0.55 就创建了一个RN项目，名称是Tshop，版本号可以不加，默认是最新版本。

（2）执行react-native run-android命令运行Android程序

         执行react-native run-ios命令运行ios程序

   (3)项目中Anroid文件夹下是Android原生代码。ios文件夹下是ios的原生代码。node_modules 是自动生成的项目依赖文件，不要动SVN和Git不要提交。

（4）index.js 程序的入口文件。

对其中的代码分析如下：import App from ‘./App’ 此句话的含义是项目一启动就加载了App.js文件中名为的App组件。

import {AppRegistry} from 'react-native';

import App from './App'; // 引入了 App.js

// 注册程序

AppRegistry.registerComponent('TShop', () => App);

（5）实质是用JSX的语言映射为了iOS和Android两个平台的原生代码  

JSX实质就是JS添加了标签X以使用控件进行显示。

控件中再回到js语法要加{}

![image](http://upload-images.jianshu.io/upload_images/12092998-e948ddfde6b9cdc3?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 注意：文字显示要放在<Text>控件里

const instructions = Platform.select({

   ios: 'Press Cmd+R to reload,\n' +

     'Cmd+D or shake for dev menu',

  android: 'Double tap R on your keyboard to reload,\n' +

     'Shake or press menu button for dev menu',

 });

export default class App extends Component {

  render() {

    return (

![image](http://upload-images.jianshu.io/upload_images/12092998-6d7fcd2d6eb4103f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  );

  }

const styles = StyleSheet.create({

  container: {

    flex: 1,  //填充整个布局

    justifyContent: 'center',  //主轴居中

    alignItems: 'center',       //水平居中

    backgroundColor: '#F5FCFF',

  },

  welcome: {

    fontSize: 20,

    textAlign: 'center',

    margin: 10,

  },

  instructions: {

    textAlign: 'center',

    color: '#333333',

    marginBottom: 5,

  },

};
