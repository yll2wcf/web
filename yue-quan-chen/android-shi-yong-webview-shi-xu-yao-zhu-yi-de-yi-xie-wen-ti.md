#### 内核版本

1. 在Android 4.4以下(不包含4.4)系统WebView底层实现是采用WebKit内核
2. 在Android 4.4及其以上Google 采用了chromium作为系统WebView的底层内核支持。

#### 注意点

1. Android SDK 16 以及之前的版本存在远程代码执行安全漏洞,该漏洞源于程序没有正
确限制使用 WebView. addJavascriptInterface方法,远程攻击者可通过使用 Java Reflection
API利用该漏洞执行任意Java对象的方法。Cordova利用onJsPrompt() 替代了addJavaScriptInterface()。
2. WebViewClient.onPageFinished()：Android4.4的手机上onPageFinished()回调会多调用一次；当前正在加载的网页产生跳转的时候这个方法可能会被多次调用。可以使用WebChromeClient.onProgressChanged方法。
3. Webview在Android 4.0上已经默认开启硬件加速，硬件加速导致页面渲染问题，需要关闭硬件加速。
4. 内存泄漏。

#### 内存泄露原因
	
1. Android5.1 以上系统原因，onDetachedFromWindow增加了isDestroyed判断，如果执行了destroy()就会导致无法反注册。5.1以下没有判断，因此没有问题。
2. 布局文件中直接使用：在XML文件中创建WebView,会把Activity作为Context传给WebView，而不是Application Context。所以在finishingActivity的时候，WebView任然持有Activity引用，导致Activity无法被回收。

#### 内存泄露解决方案1：

使用动态床架WebView，不使用布局文件声明方式

步骤：
1. 代码动态创建WebView，弱引用传入上下文
2. 在布局文件中用容器类布局，比如FrameLayout作为WebView的容器，在Activity中主动把WebView添加到容器中。
3. 在OnDestory()中移除、销毁WebView。

``` java

@Override
protected void onDestroy() {
   super.onDestroy();
   if (mWebView != null) {
      ViewParent parent = mWebView.getParent();
      if (parent != null) {
         ((ViewGroup) parent).removeView(mWebView);
      }
      mWebView.removeAllViews();
      mWebView.destroy();
      mWebView = null;
   }
}

```

#### 内存泄露解决方案2：

为加载WebView的界面开启新进程，在该页面退出之后关闭这个进程,使用System.exit(0);直接退出虚拟机（Android为每一个进程创建一个虚拟机的）。需要考虑多进程通信问题，通过AIDL与主进程进行通信。QQ貌似使用的这种方式。

步骤：
1. 在manifest文件中给webView所在的activity加上android:process属性
2. 在webView所在的activity的onDestroy()方法中System.exit(0);