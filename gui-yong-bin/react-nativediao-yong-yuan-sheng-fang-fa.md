react-native 调用原生

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











