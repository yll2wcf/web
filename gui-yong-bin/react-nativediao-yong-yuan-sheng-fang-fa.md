react-native 调用原生

首先在原生里实现代码
```java
ublic class NativeInformationModule extends ReactContextBaseJavaModule implements PreferenceManager.OnActivityResultListener {

    public List<LocalMedia> selectList = new ArrayList<>();
    private static final String REACT_CLASS = "NativeInformationModule";
    private ReactContext mReactContext;
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
     * Promise方式
     *
     * @param name
     * @param promise
     */
    @ReactMethod
    public void sendPromiseTime(String name, Promise promise) {
        WritableMap writableMap = new WritableNativeMap();
        writableMap.putString("age", "20");
        //     writableMap.putString("time",getTimeMillis());
        promise.resolve(writableMap);

    }

    //这里是希望
    @Override
    public boolean onActivityResult(int requestCode, int resultCode, Intent data) {
        if (resultCode == Activity.RESULT_OK && requestCode == CHOOSE_REQUEST) {
            switch (requestCode) {
                case PictureConfig.CHOOSE_REQUEST:

                    List<LocalMedia> selectList = PictureSelector.obtainMultipleResult(data);
                    if (selectList != null && selectList.size() > 0) {
                        LocalMedia localMedia = selectList.get(0);
                        String path = localMedia.getPath();



//                        WritableMap writableMap=new WritableNativeMap();
//                        writableMap.putString("path",path);

                        callback.invoke(path);
                    }
                    break;
            }
        }


        return false;
    }


}


```