用react-native完成一个listview

```js


import React, { Component, } from 'react';
import {
    Text,
    View,
    ListView,
    Image,
    ActivityIndicator,
    TouchableNativeFeedback,
    ProgressBarAndroid,
    Modal,
} from 'react-native';
//React-Native虽然也有Dialog，但是并不好用，所幸有Modal这个组件
//Modal组件可以用来覆盖包含React Native根视图的原生视图（如UIViewController，Activity）。

//在嵌入React Native的混合应用中可以使用Modal。Modal可以使你应用中RN编写的那部分内容覆盖在原生视图上显示。

//import {Navigator} from 'react-native-deprecated-custom-components'
import{ Navigator } from 'react-native'

import { StackNavigator } from 'react-navigation';

import styles from '../Styles/Main';
import MovieDetail from './MovieDetail';

var REQUEST_URL = 'https://api.douban.com/v2/movie/top250';

class MovieList extends Component {
    // 构造
    constructor(props) {
        super(props);
        // 初始状态
        this.state = {
            movies: new ListView.DataSource({
                rowHasChanged: ((row1, row2)=>row1 !== row2)
            }),
            load: false
        }
        //
    }

    static navigationOptions = {
        title: '推荐电影',
    };


    render() {
        //  const { navigate } = this.props.navigation;
        if (!this.state.load) {
            //return (<View style={styles.loading}>
            //            <ActivityIndicator size="large"></ActivityIndicator>
            // <ProgressBarAndroid  color="blue" styleAttr='Inverse'  justifyContent:'center' />
            //</View>);
            return ( <Modal animationType={'fade'} transparent={true} onRequestClose={()=> this.onRequestClose()}>
                <View style={styles.loadingBox}>
                    <ProgressBarAndroid styleAttr='Inverse' color='blue'/>
                    <Text>正在加载数据...</Text>
                </View>
            </Modal>);
        }


        return (
            <ListView
                dataSource={this.state.movies}
                renderRow={this.renderMovieList.bind(this)}
            ></ListView>

        );
    }

    /**
     * 跳转到电影详情的页面
     * @param movie
     */
    showMovieDetail(movie) {
        //this.props.navigator.push({
        //   // title: movie.title,
        //   // component: MovieDetail,
        //    scene:MovieDetail,
        //    params:{id:movie.id}
        //});
        let _this = this;
        const { navigator } = this.props;
        if (navigator) {
            navigator.push({
               // id: movie.id,
                params:{movie},
                component: MovieDetail,
            })
        }
    }


    renderMovieList(movie) {
        return (
            <TouchableNativeFeedback underlayColor="rgba(97,125,138,0.5)"
                //这里用了个this绑定当前的对象
                                     onPress={()=>this.showMovieDetail(movie)
                                   //{ console.log( `《${movie.title}》`);}
                                    //    ()=>navigate('Chat')
                                    }
            >
                <View style={styles.item}>
                    <View style={styles.itemImage}>
                        <Image source={{ uri: movie.images.large }}
                               style={{ width: 100, height: 100, marginTop: 5 }}>
                        </Image>
                    </View>
                    <View style={styles.itemContent}>
                        <Text style={styles.itemHeader}>{movie.title}</Text>
                        <Text style={styles.itemMeta}>{movie.original_title}({movie.year})</Text>
                        <Text style={styles.redText}>{movie.rating.average}</Text>
                    </View>
                </View>
            </TouchableNativeFeedback>
        );
    }

    /**
     * 加载耗时的网络操作
     */
    componentDidMount() {
        this.fetchData();
    }

    fetchData() {
        fetch(REQUEST_URL
            , {
                method: 'GET'
            }
        )//请求地址
            .then((response) => response.json())//取数据
            .then((responseText) => {//处理数据
                //通过setState()方法重新渲染界面

                this.setState({
                    //设置数据源刷新界面
                    movies: this.state.movies.cloneWithRows(responseText.subjects),
                    //改变加载ListView
                    load: this.state.load = true,
                })

            })
            .catch((error) => {
                console.warn(error);
            }).done();

    }
}

const SimpleApp = StackNavigator({
    Home: {screen: MovieList},
    Chat: {screen: MovieDetail},
});
export {MovieList as default};
```