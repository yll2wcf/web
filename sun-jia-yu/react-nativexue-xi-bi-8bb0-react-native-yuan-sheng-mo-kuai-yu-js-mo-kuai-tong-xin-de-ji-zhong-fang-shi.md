##React Native与JS通信的三种方式

方式一：通过Callbacks的方式

>说起Callbacks大家都不陌生，它是最常用的设计模式之一。无论是Java，Object-c，C#，还是JavaScript等都会看到Callbacks的身影。
原生模块支持Callbacks类型的参数，该Callbacks对应JS中的function。
在原生模块中：
public class RNTestModule extends ReactContextBaseJavaModule{
    public RNTestModule(ReactApplicationContext reactContext) {
        super(reactContext);
    }
    @Override
    public String getName() {
        return "RNTest";
    }

 > @ReactMethod
  public void measureLayout(
      int tag,
      int ancestorTag,
      Callback errorCallback,
      Callback successCallback) {
    try {
      measureLayout(tag, ancestorTag, mMeasureBuffer);
      map.putDouble("relativeX",1);
      map.putDouble("relativeY", 1);
      map.putDouble("width", 2);
      map.putDouble("height",3);
      successCallback.invoke(relativeX, relativeY, width, height);
    } catch (IllegalViewOperationException e) {
      errorCallback.invoke(e.getMessage());      
    }
}

在上述代码中，measureLayout方法的参数中后两个就是Callbacks，当原生模块处理成功时通过successCallback回调来告知JS处理成功的结果，当原生模块发生异常时，则通过errorCallback回调来JS处理异常。
在JS模块中：

>RNTest.measureLayout(
  100,
  100,
  (msg) => {
    console.log(msg);
  },
  (x, y, width, height) => {
    console.log(x + ':' + y + ':' + width + ':' + height);
  }
);

上述代码，是在JS模块中调用原生模块的方法measureLayout，同时向它传递了四个参数，后两个是function类型的参数用于接收原生模块的处理结果。
通过上述的方式，JS调用原生模块的measureLayout方法，原生模块则通过errorCallback与successCallbackCallbacks来将处理结果传递到JS。

方式二：通过Promises的方式
Promises是ES6的一个新的特性，在React Native中会看到Promises的大量使用。 
在原生模块中：

>public class RNTestModule extends ReactContextBaseJavaModule{
    public RNTestModule(ReactApplicationContext reactContext) {
        super(reactContext);
    }
    @Override
    public String getName() {
        return "RNTest";
    }
    @ReactMethod
    public void measureLayout(
            int tag,
            int ancestorTag,
            Promise promise) {
        try {
            WritableMap map = Arguments.createMap();
            map.putDouble("relativeX",1);
            map.putDouble("relativeY", 1);
            map.putDouble("width", 2);
            map.putDouble("height",3);

            promise.resolve(map);
        } catch (IllegalViewOperationException e) {
            promise.reject(e);
        }
    }
}

上述代码中，measureLayout方法接收的最后一个为Promise，当相应的处理结果出来之后原生模块通过调用Promise的相应方法来向JS模块传递处理成功，或处理失败的数据。
提示：在原生模块中Promise类型的参数要放在最后一位，这样JS调用的时候才能返回一个Promise。
在JS模块中：
>async test() {
  try {
    var {
        relativeX,
        relativeY,
        width,
        height,
    } = await RNTest.measureLayout(100, 100);
    console.log(relativeX + ':' + relativeY + ':' + width + ':' + height);  
  } catch (e) {
    console.error(e);
  }
}

在上述代码中，通过ES7的新特性async/await来修饰了test方法，来以同步方式调用原生模块的measureLayout方法，如果原生模块处理成功， 
那么JS中relativeX,relativeY,width,height会获得相应的值，如果原生模块处理失败，则会抛出异常。
如果，不希望以同步的形式调用，可以这样写：

>test2(){
  RNTest.measureLayout(100,100).then(e=>{
    console.log(e.relativeX + ':' + e.relativeY + ':' + e.width + ':' + e.height);
    this.setState({
      relativeX:e.relativeX,
      relativeY:e.relativeY,
      width:e.width,
      height:e.height,
    })
  }).catch(error=>{
    console.log(error);
  });
}

方式三：通过发送事件的方式

原生模块支持另外一种向JS模块传递数据的方式，通过发送事件的方式。
在原生模块中：

>@Overridepublic void onHandleResult(String barcodeData) {
    WritableMap params = Arguments.createMap();
    params.putString("result", barcodeData);
    sendEvent(getReactApplicationContext(), "onScanningResult", params);
}
private void sendEvent(ReactContext reactContext,String eventName, @Nullable WritableMap params) {
    reactContext.getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)
            .emit(eventName, params);
}

上述代码向JS模块发送了一个名为“onScanningResult”的事件，并携带了“params”作为参数。
在JS模块中：
下面是在JS代码中进行监听原生模块发出的名为“onScanningResult”的事件。

>componentDidMount() {
    //注册扫描监听
    DeviceEventEmitter.addListener('onScanningResult',this.onScanningResult);
}
onScanningResult = (e)=> {
    this.setState({
        scanningResult: e.result,
    });
    // DeviceEventEmitter.removeListener('onScanningResult',this.onScanningResult);//移除扫描监听
}

在JS中通过DeviceEventEmitter注册监听了名为“onScanningResult”的事件，当原生模块发出名为“onScanningResult”的事件后，绑定在该事件上的onScanningResult = (e)会被回调。 
然后通过e.result就可获得事件所携带的数据。
心得：如果在JS中有多处注册了onScanningResult事件，那么当原生模块发出事件后，这几个地方会同时收到该事件。不过大家也可以通过DeviceEventEmitter.removeListener('onScanningResult',this.onScanningResult) 
来移除对名为“onScanningResult”事件的监听。

方式	缺点	优点
通过Callbacks的方式	只能传递一次	传递可控，JS模块调用一次，原生模块传递一次
通过Promises的方式	只能传递一次	传递可控，JS模块调用一次，原生模块传递一次
通过发送事件的方式	原生模块主动传递，JS模块被动接收	可多次传递
三种方式的优缺点

