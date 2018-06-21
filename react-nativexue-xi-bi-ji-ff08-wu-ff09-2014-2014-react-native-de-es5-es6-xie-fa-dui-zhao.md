React Native内置了对ES2015标准的支持，在React Native的中文文档https://reactnative.cn/docs/0.51/getting-started.html 以及英文文档https://facebook.github.io/react-native/docs/getting-started.html 中建议初学者学习ES6，使用ES6进行开发。但是网上搜到的很多教程和例子都是ES5版本的，对于初学者造成困惑。本篇文章整理了一些React Native的ES5、ES6写法对照，希望大家在使用ES6语法开发的同时，也能读懂ES5的代码。

\#\#\#1. JSX、JavaScript、ES5和ES6的关系

之前介绍过React Native的语法是JSX（JavaScript eXtension），实质是JavaScript+XML，JSX就是Javascript和XML结合的一种格式。

JavaScript由三部分组成： 

ECMAScript（核心）+DOM（文档对象模型）+BOM（浏览器对象模型） 

ECMAScript作为核心，规定了语言的组成部分：语法、类型、语句、关键字、保留字、操作符、对象 

ES5是ECMAScript的第五个版本、 ES6是ECMAScript的第六个版本。

\#\#\#2. React Native的ES5、ES6写法对照

\#\#\#\#（1）模块的引用

a. 在ES5中，引入React、React Native包基本通过require进行，代码：

        

    //ES5

    var React = require\("react"\);

    var {

        Component,

        PropTypes

    } = React;  //引用React抽象组件



    var ReactNative = require\("react-native"\);

    var {

        Image,

        Text,

     } = ReactNative;  //引用具体的React Native组件



b. 在ES6里，使用import引入，代码：

    

    //ES6

    import React, { 

        Component,

        PropTypes,

    } from 'react';

    import {

        Image,

        Text

    } from 'react-native'



\#\#\#\#（2）导出单个类

a. 在ES5中，一般通过module.exports导出一个类给别的模块用

    

    //ES5

    var MyComponent = React.createClass\({

        ...

    }\);

    module.exports = MyComponent;

引用时：



    //ES5

    var MyComponent = require\('./MyComponent'\);

b. 在ES6中，通常使用export default 来实现相同的功能：

   

     //ES6

    export default class MyComponent extends Component{

        ...

    }

引用时：



    //ES6

    import MyComponent from './MyComponent';

\#\#\#\#\#注意： 导入和导出的写法必须配套，不能混用！



\#\#\#\#（3）定义组件类

a. 在ES5中使用React.createClass来定义一个组件类，代码：



    //ES5

    var Photo = React.createClass\({

       ...

    }\); 

b. 在ES6里，我们通过定义一个继承自React.Component的class来定义一个组件类，代码：



    //ES6

    class Photo extends React.Component {

         ...

    } 

\#\#\#\#\(4\) 给组件定义方法

a. ES5中给组件定义的方法格式为  方法名: function\(\)，两个方法之间需要用“，”隔开。代码：



    //ES5 

    var Photo = React.createClass\({

        componentWillMount: function\(\){



        },

        render: function\(\) {

            return \(

                &lt;Image source={this.props.source} /&gt;

            \);

        },

    }\);

b. ES6中给组件定义的方法格式为  方法名\(\)，两个方法之间不再需要用“，”隔开。代码：



    //ES6

    class Photo extends React.Component {

        componentWillMount\(\) {



        }

        render\(\) {

            return \(

                &lt;Image source={this.props.source} /&gt;

            \);

        }

    }

\#\#\#\#（5）定义组件的属性类型propTypes和默认属性Props

a. 在ES5里，属性类型和默认属性分别通过propTypes成员和getDefaultProps方法来实现，代码：



    //ES5 

    var Video = React.createClass\({

        getDefaultProps: function\(\) {

            return {

                autoPlay: false,

                maxLoops: 10,

            };

        },

        propTypes: {

            autoPlay: React.PropTypes.bool.isRequired,

            maxLoops: React.PropTypes.number.isRequired,

            posterFrameSrc: React.PropTypes.string.isRequired,

            videoSrc: React.PropTypes.string.isRequired,

        },

        render: function\(\) {

            return \(

                &lt;View /&gt;

            \);

        },

    }\);

b. 在ES6里，可以统一使用static成员来实现，代码：



    //ES6

    class Video extends React.Component {

        static defaultProps = {

            autoPlay: false,

            maxLoops: 10,

        };  // 注意这里有分号

        static propTypes = {

            autoPlay: React.PropTypes.bool.isRequired,

            maxLoops: React.PropTypes.number.isRequired,

            posterFrameSrc: React.PropTypes.string.isRequired,

            videoSrc: React.PropTypes.string.isRequired,

        };  // 注意这里有分号

        render\(\) {

            return \(

                &lt;View /&gt;

            \);

        } // 注意这里既没有分号也没有逗号

    }



也有人这么写，虽然不推荐：



    //ES6

    class Video extends React.Component {

        render\(\) {

            return \(

                &lt;View /&gt;

            \);

        }

    }

    Video.defaultProps = {

        autoPlay: false,

        maxLoops: 10,

    };

    Video.propTypes = {

        autoPlay: React.PropTypes.bool.isRequired,

        maxLoops: React.PropTypes.number.isRequired,

        posterFrameSrc: React.PropTypes.string.isRequired,

        videoSrc: React.PropTypes.string.isRequired,

    };

\#\#\#\#（6）初始化State

a. 在ES5中使用getInitialState初始化State，代码：



    //ES5 

    var Video = React.createClass\({

        getInitialState: function\(\) {

            return {

                loopsRemaining: this.props.maxLoops,

            };

        },

    }\)

b. ES6下，有两种写法：



    //ES6

    class Video extends React.Component {

        state = {

            loopsRemaining: this.props.maxLoops,

        }

    }

不过我们推荐更易理解的在构造函数中初始化（这样你还可以根据需要做一些计算）：



    //ES6

    class Video extends React.Component {

        constructor\(props\){

            super\(props\);

            this.state = {

                loopsRemaining: this.props.maxLoops,

            };

        }

    }



\#\#\#\(7\)把方法作为回调提供

a. 在ES5下，React.createClass会把所有的方法都bind一遍，这样可以提交到任意的地方作为回调函数，而this不会变化。但官方现在逐步认为这反而是不标准、不易理解的。



    //ES5

    var PostInfo = React.createClass\({

        handleOptionsButtonClick: function\(e\) {

            // Here, 'this' refers to the component instance.

            this.setState\({showOptionsModal: true}\);

        },

        render: function\(\){

            return \(

                &lt;TouchableHighlight onPress={this.handleOptionsButtonClick}&gt;

                    &lt;Text&gt;{this.props.label}&lt;/Text&gt;

                &lt;/TouchableHighlight&gt;

            \)

         },

    }\);

b. 在ES6下，你需要通过bind来绑定this引用，或者使用箭头函数（它会绑定当前scope的this引用）来调用



      / /ES6

    class PostInfo extends React.Component

    {

        handleOptionsButtonClick\(e\){

            this.setState\({showOptionsModal: true}\);

        }

         render\(\){

            return \(

                 &lt;TouchableHighlight 

                     onPress={this.handleOptionsButtonClick.bind\(this\)}

                     onPress={e=&gt;this.handleOptionsButtonClick\(e\)}

                    &gt;

                     &lt;Text&gt;{this.props.label}&lt;/Text&gt;

                 &lt;/TouchableHighlight&gt;

             \)

         },

    }

箭头函数实际上是在这里定义了一个临时的函数，箭头函数的箭头\`=&gt;\`之前是一个空括号、单个的参数名、或用括号括起的多个参数名，而箭头之后可以是一个表达式（作为函数的返回值），或者是用花括号括起的函数体（需要自行通过return来返回值，否则返回的是undefined）。



\`\`\`

// 箭头函数的例子

\(\)=&gt;1

v=&gt;v+1

\(a,b\)=&gt;a+b

\(\)=&gt;{

    alert\("foo"\);

}

e=&gt;{

    if \(e == 0\){

        return 0;

    }

    return 1000/e;

}



\`\`\`



需要注意的是，不论是bind还是箭头函数，每次被执行都返回的是一个新的函数引用，因此如果你还需要函数的引用去做一些别的事情（譬如卸载监听器），那么你必须自己保存这个引用



\`\`\`

// 错误的做法

class PauseMenu extends React.Component{

    componentWillMount\(\){

        AppStateIOS.addEventListener\('change', this.onAppPaused.bind\(this\)\);

    }

    componentDidUnmount\(\){

        AppStateIOS.removeEventListener\('change', this.onAppPaused.bind\(this\)\);

    }

    onAppPaused\(event\){

    }

}



\`\`\`



\`\`\`

// 正确的做法

class PauseMenu extends React.Component{

    constructor\(props\){

        super\(props\);

        this.\_onAppPaused = this.onAppPaused.bind\(this\);

    }

    componentWillMount\(\){

        AppStateIOS.addEventListener\('change', this.\_onAppPaused\);

    }

    componentDidUnmount\(\){

        AppStateIOS.removeEventListener\('change', this.\_onAppPaused\);

    }

    onAppPaused\(event\){

    }

}



\`\`\`



从\[这个帖子\]\(http://www.tuicool.com/articles/Rj6RFnm\)中我们还学习到一种新的做法：



\`\`\`

// 正确的做法

class PauseMenu extends React.Component{

    componentWillMount\(\){

        AppStateIOS.addEventListener\('change', this.onAppPaused\);

    }

    componentDidUnmount\(\){

        AppStateIOS.removeEventListener\('change', this.onAppPaused\);

    }

    onAppPaused = \(event\) =&gt; {

        //把方法直接作为一个arrow function的属性来定义，初始化的时候就绑定好了this指针

    }

}

\`\`\`

\#\#\#\(8\) Mixins



在ES5下，我们经常使用mixin来为我们的类添加一些新的方法，譬如PureRenderMixin



\`\`\`

var PureRenderMixin = require\('react-addons-pure-render-mixin'\);

React.createClass\({

  mixins: \[PureRenderMixin\],



  render: function\(\) {

    return &lt;div className={this.props.className}&gt;foo&lt;/div&gt;;

  }

}\);



\`\`\`



然而现在官方已经不再打算在ES6里继续推行Mixin，他们说：\[Mixins Are Dead. Long Live Composition\]\(https://medium.com/@dan\_abramov/mixins-are-dead-long-live-higher-order-components-94a0d2f9e750\)。



尽管如果要继续使用mixin，还是有一些第三方的方案可以用，譬如\[这个方案\]\(https://github.com/brigand/react-mixin\)



不过官方推荐，对于库编写者而言，应当尽快放弃Mixin的编写方式，上文中提到\[Sebastian Markbåge\]\(https://gist.github.com/sebmarkbage/ef0bf1f338a7182b6775\)的一段代码推荐了一种新的编码方式：



\`\`\`

//Enhance.js

import { Component } from "React";



export var Enhance = ComposedComponent =&gt; class extends Component {

    constructor\(\) {

        this.state = { data: null };

    }

    componentDidMount\(\) {

        this.setState\({ data: 'Hello' }\);

    }

    render\(\) {

        return &lt;ComposedComponent {...this.props} data={this.state.data} /&gt;;

    }

};



\`\`\`



\`\`\`

//HigherOrderComponent.js

import { Enhance } from "./Enhance";



class MyComponent {

    render\(\) {

        if \(!this.data\) return &lt;div&gt;Waiting...&lt;/div&gt;;

        return &lt;div&gt;{this.data}&lt;/div&gt;;

    }

}



export default Enhance\(MyComponent\); // Enhanced component



\`\`\`



用一个“增强函数”，来某个类增加一些方法，并且返回一个新类，这无疑能实现mixin所实现的大部分需求。



\#\#\#\(9\) ES6+带来的其它好处



\#\#\#\# 解构&属性延展



结合使用ES6+的解构和属性延展，我们给孩子传递一批属性更为方便了。这个例子把className以外的所有属性传递给div标签：



\`\`\`

class AutoloadingPostsGrid extends React.Component {

    render\(\) {

        var {

            className,

            ...others,  // contains all properties of this.props except for className

        } = this.props;

        return \(

            &lt;div className={className}&gt;

                &lt;PostsGrid {...others} /&gt;

                &lt;button onClick={this.handleLoadMoreClick}&gt;Load more&lt;/button&gt;

            &lt;/div&gt;

        \);

    }

}



\`\`\`



下面这种写法，则是传递所有属性的同时，用覆盖新的className值：



\`\`\`

&lt;div {...this.props} className="override"&gt;

    …

&lt;/div&gt;



\`\`\`



这个例子则相反，如果属性中没有包含className，则提供默认的值，而如果属性中已经包含了，则使用属性中的值



\`\`\`

&lt;div className="base" {...this.props}&gt;

    …

&lt;/div&gt;

\`\`\`

\#\#\#参考资料

\[https://blog.csdn.net/qtwwyl/article/details/76285270\]\(https://blog.csdn.net/qtwwyl/article/details/76285270\)



\[https://blog.csdn.net/wbiokr/article/details/73027398?utm\_source=itdadao&utm\_medium=referral\]\(https://blog.csdn.net/wbiokr/article/details/73027398?utm\_source=itdadao&utm\_medium=referral\)



\[http://bbs.reactnative.cn/topic/15/react-react-native-%E7%9A%84es5-es6%E5%86%99%E6%B3%95%E5%AF%B9%E7%85%A7%E8%A1%A8/2\]\(http://bbs.reactnative.cn/topic/15/react-react-native-%E7%9A%84es5-es6%E5%86%99%E6%B3%95%E5%AF%B9%E7%85%A7%E8%A1%A8/2\)







