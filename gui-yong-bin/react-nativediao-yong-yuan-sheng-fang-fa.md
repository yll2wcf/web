react-native 调用原生



#注意
 如果你修改了原生代码，我的处理方式是卸载APP重新安装，reload命令只是重新安装的js代码，
 
首先在原生里实现代码

```java
public class NativeInformationModule extends ReactContextBaseJavaModule {

    private static final String REACT_CLASS = "TransMissonMoudle";
    private ReactContext mReactContext;

    public TransMissonMoudle(ReactApplicationContext reactContext) {
        super(reactContext);
        this.mReactContext = reactContext;
    }

    @Override
    public String getName() {
        return REACT_CLASS;
    }

    @ReactMethod
    public void nativeToast() {
        Log.e("tagMsg", "调用原生吐司");
        Toast.makeText(mReactContext, "调用原生吐司", Toast.LENGTH_SHORT).show();

    }
   

}
```

```java

public class TransMissonPackage implements ReactPackage{

    @Override
    public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {
        List<NativeModule> modules = new ArrayList<>();
        modules.add(new NativeInformationModule(reactContext));//摇一摇
        return  modules;
    }




    @Override
    public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
        List<ViewManager> viewManagerList=new ArrayList<>();

       // return viewManagerList;

        return Collections.emptyList();
    }
}
````
#在MainApplication里
    添加上增加的package
```java

     @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
          new MainReactPackage(),
              new TransMissonPackage()
      );
    }


```

```js
#在js里如何调用原生代码

import React, {Component} from 'react';
import {
    AppRegistry,
    Text ,
    NativeModules,
    View,
    TouchableOpacity,
    TextInput,
    StyleSheet,
    ToastAndroid,
    DeviceEventEmitter,
    Image,
} from 'react-native';


import HTTPUtil from '../utils/HttpUtils';
type
Props = {};
export default class ThirdPage extends Component <Props> {




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
                    <TouchableOpacity style={styles.touchStyle} onPress={()=>{this._jumpToIndex();} }>
                        <Text style={styles.touchText}>
                            跳回第二页
                        </Text>

                    </TouchableOpacity>
                </View>
                <View>
                    <TouchableOpacity style={styles.touchStyle}
                                      onPress={()=>{this.nativeToast1();} }>
                        <Text style={styles.touchText}>
                            原生吐司
                        </Text>

                    </TouchableOpacity>
                </View>
           
            </View>
        )
    }


    _jumpToIndex() {
        this.props.navigation.navigate('second');
    }

   

    nativeToast1() {
        //  ToastAndroid.show("非原生" , ToastAndroid.SHORT)

        NativeModules.NativeInformationModule.nativeToast();
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

    viewImageStyle: {
        backgroundColor: '#567ECB',
        width: 300,
        height: 300
    }
});
```













