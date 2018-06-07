融云推送：

在Application的onCreate里
注册华为和小米推送
```java
            RongPushClient.registerHWPush(this);
            RongPushClient.registerMiPush(this, "12321321312321", "23123123123");
```
写在初始化融云之前。

在配置清单里配置：
```
 <!-- 第三方的推送 权限 广播 和服务 融云 -->
        <!-- 小米 配置开始 < -->
        <service
            android:name="com.xiaomi.push.service.XMPushService"
            android:enabled="true"/>
        <service
            android:name="com.xiaomi.mipush.sdk.PushMessageHandler"
            android:enabled="true"
            android:exported="true"/>
        <service
            android:name="com.xiaomi.mipush.sdk.MessageHandleService"
            android:enabled="true"/>
        <!-- 注：此service必须在2.2.5版本以后（包括2.2.5版本）加入 -->

        <service
            android:name="com.xiaomi.push.service.XMJobService"
            android:enabled="true"
            android:exported="false"
            android:permission="android.permission.BIND_JOB_SERVICE"/>
        <!-- 注：此service必须在3.0.1版本以后（包括3.0.1版本）加入 -->

        <receiver
            android:name="com.xiaomi.push.service.receivers.NetworkStatusReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE"/>

                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
        </receiver>
        <receiver
            android:name="com.xiaomi.push.service.receivers.PingReceiver"
            android:exported="false">
            <intent-filter>
                <action android:name="com.xiaomi.push.PING_TIMER"/>
            </intent-filter>
        </receiver>
        <receiver
            android:name="io.rong.push.platform.MiMessageReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="com.xiaomi.mipush.RECEIVE_MESSAGE"/>
            </intent-filter>
            <intent-filter>
                <action android:name="com.xiaomi.mipush.MESSAGE_ARRIVED"/>
            </intent-filter>
            <intent-filter>
                <action android:name="com.xiaomi.mipush.ERROR"/>
            </intent-filter>
        </receiver>
        <!-- 小米 配置结束 < -->
        
        
         <!-- HMS 配置开始 -->
        <!-- 华为HMS推送静态注册 -->
        <meta-data
            android:name="com.huawei.hms.client.appid"
            android:value="10338843"/>

        <!-- BridgeActivity定义了HMS-SDK中一些跳转所需要的透明页面 -->
        <activity
            android:name="com.huawei.hms.activity.BridgeActivity"
            android:configChanges="orientation|locale|screenSize|layoutDirection|fontScale"
            android:excludeFromRecents="true"
            android:exported="false"
            android:hardwareAccelerated="true"
            android:theme="@android:style/Theme.Translucent">
            <meta-data
                android:name="hwc-theme"
                android:value="androidhwext:style/Theme.Emui.Translucent"/>
        </activity>
        <!-- 解决华为移动服务升级问题的透明界面（必须声明） -->
        <activity
            android:name="com.huawei.android.hms.agent.common.HMSAgentActivity"
            android:configChanges="orientation|locale|screenSize|layoutDirection|fontScale"
            android:excludeFromRecents="true"
            android:exported="false"
            android:hardwareAccelerated="true"
            android:theme="@android:style/Theme.Translucent">
            <meta-data
                android:name="hwc-theme"
                android:value="androidhwext:style/Theme.Emui.Translucent"/>
        </activity>

        <provider
            android:name="com.huawei.hms.update.provider.UpdateProvider"
            android:authorities="com.zl.daka.hms.update.provider"
            android:exported="false"
            android:grantUriPermissions="true">
        </provider>

        <!-- 第三方相关 :接收Push消息（注册、Push消息、Push连接状态）广播 -->
        <receiver android:name="io.rong.push.platform.HMSReceiver">
            <intent-filter>

                <!-- 必须,用于接收token -->
                <action android:name="com.huawei.android.push.intent.REGISTRATION"/>
                <!-- 必须，用于接收消息 -->
                <action android:name="com.huawei.android.push.intent.RECEIVE"/>
                <!-- 可选，用于点击通知栏或通知栏上的按钮后触发onEvent回调 -->
                <action android:name="com.huawei.android.push.intent.CLICK"/>
                <!-- 可选，查看push通道是否连接，不查看则不需要 -->
                <action android:name="com.huawei.intent.action.PUSH_STATE"/>
            </intent-filter>
        </receiver>
        <receiver android:name="com.huawei.hms.support.api.push.PushEventReceiver">
            <intent-filter>

                <!-- 接收通道发来的通知栏消息，兼容老版本Push -->
                <action android:name="com.huawei.intent.action.PUSH"/>
            </intent-filter>
        </receiver>
        <!-- HMS 配置结束 -->
        
        
        
          <!--
      应用处于后台运行或者和融云服务器 disconnect() 的时候，如果收到消息，
      融云 SDK 会以通知形式提醒您。所以您还需要自定义一个继承融云 PushMessageReceiver 的广播接收器，用来接收提醒通知
        -->
        <receiver
            android:name=".receiver.RongYunNotificationReceiver"
            android:exported="true">
            <intent-filter>
                <action android:name="io.rong.push.intent.MESSAGE_ARRIVED"/>
                <action android:name="io.rong.push.intent.MI_MESSAGE_ARRIVED"/>
                <action android:name="io.rong.push.intent.MESSAGE_CLICKED"/>
                <action android:name="io.rong.push.intent.MI_MESSAGE_CLICKED"/>
                <action android:name="io.rong.push.intent.THIRD_PARTY_PUSH_STATE"/>
            </intent-filter>
        </receiver>
```      
```java      
public class RongYunNotificationReceiver extends PushMessageReceiver {
    @Override
    public boolean onNotificationMessageArrived(Context context, PushNotificationMessage message) {
       return false; // 返回 false, 会弹出融云 SDK 默认通知; 返回 true, 融云 SDK 不会弹通知, 通知需要由您自定义。
}

    @Override
    public boolean onNotificationMessageClicked(Context context, PushNotificationMessage message) {
        return false; // 返回 false, 会走融云 SDK 默认处理逻辑, 即点击该通知会打开会话列表或会话界面; 返回 true, 则由您自定义处理逻辑。
    }
}
```

```
<activity
   android:name="B"
   android:launchMode="singleTask"
   android:screenOrientation="portrait"
   android:windowSoftInputMode="stateHidden|adjustResize">

   <intent-filter>
       <action android:name="android.intent.action.VIEW" />
       <category android:name="android.intent.category.DEFAULT" />
       <data
           android:host="你的包名"
           android:path="/conversationlist"
           android:scheme="rong" />
   </intent-filter>
</activity>

把这个data配置在哪个activity下面，推送过来的时候就会打开哪个activity
<data
           android:host="你的包名"
           android:path="/conversationlist"
           android:scheme="rong" />


````

        
        
        
        
        
        
        
        
        
        
        
        
        
        
