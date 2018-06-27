react-native 调用原生



#注意
 如果你修改了原生代码，我的处理方式是卸载APP重新安装，reload命令只是重新安装的js代码，
 
首先在原生里实现代码

```java
public class NativeInformationModule extends ReactContextBaseJavaModule  {

    public List<LocalMedia> selectList = new ArrayList<>();
    private static final String REACT_CLASS = "NativeInformationModule";
    private static ReactContext mReactContext;
    int CHOOSE_REQUEST = 188;

    @Override
    public String getName() {
        return REACT_CLASS;
    }


    public NativeInformationModule(ReactApplicationContext reactContext) {
        super(reactContext);
        this.mReactContext = reactContext;
    }


    @ReactMethod
    public void nativeToast() {
        Log.e("tagMsg", "调用原生吐司");
        Toast.makeText(mReactContext, "调用原生吐司", Toast.LENGTH_SHORT).show();

    }

    private Callback callback;
    /**
     * package com.facebook.react.bridge;   Callback 来自这个包
     */
    @ReactMethod
    public void callBackPic(String name, Callback callback) {
        this.callback = callback;
        RxPermissions rxPermissions = new RxPermissions(mReactContext.getCurrentActivity());

        rxPermissions.request(Manifest.permission.READ_EXTERNAL_STORAGE,
                Manifest.permission.WRITE_EXTERNAL_STORAGE, Manifest.permission.CAMERA).subscribe(new Observer<Boolean>() {
            @Override
            public void onSubscribe(Disposable d) {

            }

            @Override
            public void onNext(Boolean aBoolean) {
                if (aBoolean) {
                    PictureSelector.create(mReactContext.getCurrentActivity()).openGallery(PictureMimeType.ofImage())
                            .maxSelectNum(1)// 最大图片选择数量
                            .minSelectNum(0)
                            //       .selectionMode(MULTIPLE)//多选
                            .previewImage(true)//是否可以预览图片
                            .isCamera(true)// 是否显示拍照按钮 ture or false
                            .selectionMedia(selectList)
                            .forResult(CHOOSE_REQUEST);

                }
            }

            @Override
            public void onError(Throwable e) {

            }

            @Override
            public void onComplete() {

            }
        });




    }




    /**
     * 用来给原生的方法调用，给rn界面发送数据的
     * @param eventName
     * @param params
     */
    public static void sendTransMisson( String eventName, @javax.annotation.Nullable WritableMap params) {
        mReactContext.getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
                .emit(eventName, params);

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
```java

public class MainActivity extends ReactActivity {

    /**
     * Returns the name of the main component registered from JavaScript.
     * This is used to schedule rendering of the component.
     */
    @Override
    protected String getMainComponentName() {
        return "TestRN";
    }

    @Override
    public void onCreate(@Nullable Bundle savedInstanceState, @Nullable PersistableBundle persistentState) {
        super.onCreate(savedInstanceState, persistentState);

    }

//打开相册的结果回调
    @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (resultCode == Activity.RESULT_OK) {
            switch (requestCode) {
                case PictureConfig.CHOOSE_REQUEST:

                    List<LocalMedia> selectList = PictureSelector.obtainMultipleResult(data);
                    if (selectList != null && selectList.size() > 0) {
                        LocalMedia localMedia = selectList.get(0);
                        String path = localMedia.getPath();
                        WritableMap writableMap=new WritableNativeMap();
                        writableMap.putString("path",path);

                        Log.e("tagString1",path);
                        //将回调的照片路径传给rn页面
                        NativeInformationModule.sendTransMisson("EventGetPic",writableMap);

                    }
                    break;
            }
        }
    }
}

```




#在js里如何调用原生代码
```js

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

    componentWillMount() {
        DeviceEventEmitter.addListener('EventGetPic', this._OnPicSelected.bind(this));
    }

    //获取原生回调的结果
    _OnPicSelected(msg) {
        console.log(msg);
        if (msg && msg.path) {
            ToastAndroid.show("照片路径:" + "\n" + msg.path, ToastAndroid.LONG);
            this.setState({//更改状态要用setState方法
                title:'file:///'+ msg.path,//加载了一张本地相册的图片
            })
        }
    }


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
                <Text>第三{this.state.title}</Text>
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
                <View>
                    <TouchableOpacity style={styles.touchStyle}
                                      onPress={()=>{this._openGallery();} }>
                        <Text style={styles.touchText}>
                            打开相册
                        </Text>

                    </TouchableOpacity>
                </View>


                <Image style={styles.viewImageStyle}
                       source={{uri:this.state.title}}
                       resizeMode="cover"/>


            </View>
        )
    }

    //跳回第二页
    _jumpToIndex() {
        this.props.navigation.navigate('second');
    }

    //打开原生的相机
    _openGallery() {
        NativeModules.NativeInformationModule.callBackPic("Pic",
            (msg)=> {
                if (msg) {
                    ToastAndroid.show(msg.toString(), ToastAndroid.SHORT)
                }

            });
    }

    //调用原生吐司
    nativeToast1() {
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













