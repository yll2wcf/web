1实现一个简单的react-native 页面跳转

引用第三方的react-navigation:
官网地址：https://reactnavigation.org/docs/en/getting-started.html
github 地址:https://github.com/react-navigation/react-navigation

在项目的根目录下执行：
#yarn add react-navigation
# or with npm
# npm install --save react-navigation

#APP.js
```js
import React from 'react';
//导入 react-navigation 组件
import {
    createStackNavigator,
    createBottomTabNavigator,
    createMaterialTopTabNavigator
} from 'react-navigation';

import welcome from './src/page/WelcomePage'
import index from './src/page/IndexPage'
import second from './src/page/SecondPage'
//页面切换动画插入器
import CardStackStyleInterpolator from 'react-navigation/src/views/StackView/StackViewStyleInterpolator';

//headerTitle: '标题' ===> 导航栏的标题
//header: null ===> 隐藏导航栏
//headerTintColor: 'white' ===> 返回按钮的颜色
//headerTitleStyle: {} ===> 导航栏文字的样式
//headerStyle: {} ===> 导航栏的样式
//headerRight: (< View/>) ===> 设置顶部导航栏友边的视图 和 解决当有返回箭头时，文字不居中
//headerLeft: (< View/>) ===> 设置导航栏左边的视图 和 去除返回箭头
//headerBackTitle: null ===> 设置跳转页面左侧返回箭头后面的文字，默认是上一个页面的标题
//gestureResponseDistance: {horizontal: 300} ===> //设置滑动返回的距离
const App = createStackNavigator({
      //这里index和second的先后顺序会决定在APP上显示的先后顺序
        index: {
            screen: index,
            navigationOptions: {
                gesturesEnabled: true,
                header: null,

            }
        },


        second: {
            screen: second,
            navigationOptions: {
                gesturesEnabled: true,
                header: null,
                tabBarLabel: '我是第二页',
            }
        }


    }
)


export default App
```

#IndexPage.js
```js
import React, {Component} from 'react';
import {Text ,
    View,
    TouchableOpacity,
    TextInput,
    StyleSheet
} from 'react-native'

export default class IndexPage extends Component {

    // 构造
    constructor(props) {
        super(props);
        // 初始状态
        this.state = {};

    }


    render() {
        return (
            <View style={styles.container}>
                <Text style={styles.instructions}>
                    我是第一页
                </Text>


                <View style={styles.touchView}>
                    <TouchableOpacity style={styles.touchStyle} onPress={()=>{
                    this._jumpToSecond();}}>
                        <Text style={styles.touchText}>
                            跳到第二页(不带参数)
                        </Text>
                    </TouchableOpacity>
                </View>


                <View style={styles.touchView}>
                    <TouchableOpacity style={styles.touchStyle} onPress={()=>{
                    this._jumpToSecondWithParam();}}>
                        <Text style={styles.touchText}>
                            跳到第二页(带参数)
                        </Text>
                    </TouchableOpacity>
                </View>


            </View>
        );
    }

    _jumpToSecond() {
        this.props.navigation.navigate('second');
    }

    _jumpToSecondWithParam() {
        this.props.navigation.navigate('second', {
        //设置一些写死的参数
            title: '图片详情',
            url: "一条url地址",
        });
    }
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
    },
    instructions: {
        textAlign: 'center',
        color: '#333333',
        marginBottom: 5,
    },
    touchView: {
        margin: 10,
        width:200,
        height: 100,


    },
    touchText1: {
        padding: 0,
        fontSize: 30,
        color: '#C62F37',
        justifyContent: 'space-between',
        alignItems: 'center',

    },

    touchText: {
        padding: 0,
        fontSize: 20,
        color: '#C62F37',
        justifyContent: 'space-around',
        alignItems: 'center',

    },
    touchStyle: {
        backgroundColor: '#4792FA',
        alignItems: 'center',
    },
    headerStyle: {
        backgroundColor: '#4398ff',
    },
    headerTitleStyle: {
        //标题的文字颜色
        color: 'white',
        //设置标题的大小
        fontSize: 18,
        //居中显示
        alignSelf: 'center',
    },
});
```
#SecondPage
```js
import React, {Component} from 'react';
import {Text ,
    View,
    TouchableOpacity,
    TextInput,
    StyleSheet
} from 'react-native';
import HTTPUtil from '../utils/HttpUtils';
type
Props = {};
export default class SecondPage extends Component <Props> {





    //构造
    constructor(props) {
        super(props);

        const {navigation} = this.props;
        let param1 = this.props.navigation.getParam('title'); // 获取第一个参数
        let param2 = this.props.navigation.getParam('url');
        // 初始状态
        this.state = {
            title: param1,
            url: param2,
        };

    }


    render() {
      

        return (
            <View style={styles.container}>
                <Text>第二{this.state.title}</Text>
                <View>
                    <TouchableOpacity style={styles.touchStyle} onPress={()=>{
                    this._jumpToIndex();} }>
                        <Text style={styles.touchText}>
                            跳回第一页
                        </Text>

                    </TouchableOpacity>
                </View>

              

            </View>
        )
    }

    _jumpToIndex() {
        this.props.navigation.navigate('index');
    }

}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'flex-start',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
    },
    touchView: {
        width: 300,
        height: 100,

    },
    touchText: {
        color: '#C62F37',
    },

    touchStyle: {
        backgroundColor: '#E9F8FD',
        alignItems: 'center',
    },
    headerStyle: {
        backgroundColor: '#4398ff',
    },
    headerTitleStyle: {
        color: 'white',
        //设置标题的大小
        fontSize: 18,
        //居中显示
        alignSelf: 'center',
    },
});

```









