
##集成融云
在“打卡助手”APP中集成了即时通讯功能，采用了来自融云的第三方技术。
>>集成融云即时通讯的步骤：
   首先从融云官网注册账号，注册你的APP并且获取一个APP key和APP Secret。然后下载融云的sdk，并依赖到项目里来。融云的sdk，有4个主要的module: IMKit, IMLib ,CallKit, CallLib。在本项目中我们主要使用了前两个module，暂时可以满足当前的需求。此外融云还提供了一些so文件，小米推送jar包，高德地图的jar包。为了能准确接到消息推送请务必兴建asset目录，并在asset下放入push_daemon（推送守护）中的相关文件。以上准备工作在照着官网步骤即可完成，在这里主要说说曾经遇到的问题。曾经没有添加高德地图jar包，在聊天的底部面板就没有出现发送定位的功能，添加上后默认出现发送定位。其次配置Manifest文件和相关的Activity，Receiver。在application里面初始化融云。这一步照着官网来。但是这里我遇到了跟原来的fileProvider冲突的问题。自己原来的android:resource="@xml/rc_file_path"和通云IMKit里的文件冲突。
   采取的办法： xml文件夹下放上融云的file_path 和 自己原来的，在AndroidManifest.xml里做如下配置
    <provider
            android:name=".ui.provider.DakaFileProvider"
            android:authorities="${applicationId}.fileprovider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths"/>
        </provider>
        <!-- 融云 -->
        <provider
            android:name="android.support.v4.content.FileProvider"
            android:authorities="${applicationId}.FileProvider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/rc_file_path"/>
        </provider>
   
   
   
接下来开始相关代码的编写。首先进行融云的连接，RongIM.connect(token, new RongIMClient.ConnectCallback(){}); 

然后给融云提供通讯录里的联系人。融云提供了2种方式，这里我们介绍第一种方式

```java
List<Friend> userList = null;
private void initUserInfo() {
    userList = new ArrayList<>();
    userList.add(new Friend("639952043", "gui1", ""));
    userList.add(new Friend("9916271905", "gui2", ""));
    userList.add(new Friend("123456", "gui3", ""));
    userList.add(new Friend("1234567", "gui4", ""));
}
```


Friend是自己写的bean类，只有3个成员变量 userId,username,图像的url。
这里是以之为例初始化了几个自己写的用户。在给融云提供用户的时候应当这样写：

```java
RongIM.setUserInfoProvider(new RongIM.UserInfoProvider() {
    @Override
    public UserInfo getUserInfo(String s) {
        for (Friend i : userList) {
            if (i.getUserId().equals(s)) {
                Log.e(TAG, i.getName());
                return new UserInfo(i.getUserId(), i.getName(), Uri.parse(i.getImageUrl()));
            }
        }
        return null;
    }
},true);
```
这样融云才能区分出自己和对方。该方法在融云里只会被调用一次，希望在初始化会话列表Fragment之前被调用。


如果联系人列表发生更新，采用以下方式刷新给融云提供的联系人列表
```java
for (Friend i : userList) {
    if (i.getUserId().equals(s)) {
        Log.e(TAG, i.getName());
       RongIM.getInstance().refreshUserInfoCache(new UserInfo(i.getUserId(),i.getName(),Uri.parse(i.getImageUrl())));
    }
}
```
另外单独修改自己的名称和头像也用这个方法来刷新。
```java
  RongIM.getInstance().refreshUserInfoCache(new UserInfo(i.getUserId(),i.getName(),Uri.parse(i.getImageUrl()))); 
```
 



然后初始化聊天列表,聊天列表是个Fragment，我们使用融云官方的Fragment。这里我们这样来写:
```java
private Fragment initConversationList() {

    if (mConversationListFragment == null) {
        ConversationListFragment listFragment = new ConversationListFragment();
        Uri uri = Uri.parse("rong://" + getApplicationInfo().packageName).buildUpon()
                .appendPath("conversationlist")
                .appendQueryParameter(Conversation.ConversationType.PRIVATE.getName(), "false") //设置私聊会话是否聚合显示
                .appendQueryParameter(Conversation.ConversationType.GROUP.getName(), "false")//群组
                .appendQueryParameter(Conversation.ConversationType.PUBLIC_SERVICE.getName(), "false")//公共服务号
                .appendQueryParameter(Conversation.ConversationType.APP_PUBLIC_SERVICE.getName(), "false")//订阅号
                //    .appendQueryParameter(Conversation.ConversationType.SYSTEM.getName(), "false")//系统 注销掉之后不会接收来自系统的推送
                .build();
        mConversationsTypes = new Conversation.ConversationType[]{Conversation.ConversationType.PRIVATE,
                Conversation.ConversationType.GROUP,
                Conversation.ConversationType.PUBLIC_SERVICE,
                Conversation.ConversationType.APP_PUBLIC_SERVICE,
                //    Conversation.ConversationType.SYSTEM   //注销掉之后不会接收来自系统的推送
        };
        
        listFragment.setUri(uri);
        mConversationListFragment = listFragment;
        return listFragment;
    } else {
        return mConversationListFragment;
    }
}
````
这里mConversationsTypes 其实可以不写，决定当前app接收哪几种会话的是.appendQueryParameter(Conversation.ConversationType.APP_PUBLIC_SERVICE.getName(), "false")//订阅号
参数1：会话类型;参数2：是否聚合显示，项目里我填的都是false。

在通讯录里找到联系人调用
RongIM.getInstance().startPrivateChat(context, targetId, targetName);
会话列表里就会显示出你们之间的会话条目。

自定义ConversationActivity。这个会话activity 中一些布局，参数你可能跟融云的不太一样需要自己去自定义一些，在本项目里我改了标题和背景颜色，控件直接在activity_conversation.xml里修改就可以。参数通过
title = intent.getData().getQueryParameter("title");
来获取。
在IMKit的资源目录下有个rc_config.xml。
      res>values>rc_config.xml，
有很多关于对话页面的配置，可以在这里进行修改。
例如：
<!-- 设置是否支持消息撤回-->
<bool name="rc_enable_message_recall">true</bool>
<!-- 设置已读回执，目前仅支持单聊、群聊、讨论组 -->
<bool name="rc_read_receipt">true</bool>


>>增加底部面板的扩展功能：
 
以增加请假按钮为例，融云官网已经说的很清楚了，这里贴上代码：
```java
@MessageTag(value = "app:QingJia", flag = MessageTag.ISCOUNTED | MessageTag.ISPERSISTED)
public class QingJiaMessage extends MessageContent {
    //自定义的属性
    private String name;
    private String type;
    private String startDay;
    private String endDay;

    public QingJiaMessage() {}
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getType() {
        return type;
    }
    public void setType(String type) {
        this.type = type;
    }
    public String getStartDay() {
        return startDay;
    }
    public void setStartDay(String startDay) {
        this.startDay = startDay;
    }
    public String getEndDay() {
        return endDay;
    }
    public void setEndDay(String endDay) {
        this.endDay = endDay;
    }

    private Parcel mParcel;

    public QingJiaMessage(Parcel parcel) {
        mParcel = parcel;
        setName(ParcelUtils.readFromParcel(parcel));
        setType(ParcelUtils.readFromParcel(parcel));
        setStartDay(ParcelUtils.readFromParcel(parcel));
        setEndDay(ParcelUtils.readFromParcel(parcel));
    }

    public static QingJiaMessage obtain(String name, String type, String startDay, String endDay) {
        QingJiaMessage qingJiaMessage = new QingJiaMessage();
        qingJiaMessage.name = name;
        qingJiaMessage.type = type;
        qingJiaMessage.startDay = startDay;
        qingJiaMessage.endDay = endDay;
        return qingJiaMessage;
    }

    public QingJiaMessage(byte[] data) {
        String jsonStr = null;
        try {
            jsonStr = new String(data, "UTF-8");
        } catch (UnsupportedEncodingException e1) {
        }

        try {
            JSONObject jsonObj = new JSONObject(jsonStr);
            if (jsonObj.has("name")) {
                name = jsonObj.optString("name");
            }
            if (jsonObj.has("type")) {
                type = jsonObj.optString("type");
            }
            if (jsonObj.has("startDay")) {
                startDay = jsonObj.optString("startDay");
            }
            if (jsonObj.has("endDay")) {
                endDay = jsonObj.optString("endDay");
            }
        } catch (JSONException e) {
            Log.e("TAG", e.getMessage());
        }
    }

    public static final Creator<QingJiaMessage> CREATOR = new Creator<QingJiaMessage>() {
        @Override
        public QingJiaMessage createFromParcel(Parcel source) {
            return new QingJiaMessage(source);
        }
        @Override
        public QingJiaMessage[] newArray(int size) {
            return new QingJiaMessage[size];
        }
    };

    @Override
    public byte[] encode() {
        JSONObject jsonObject = new JSONObject();
        try {
            jsonObject.put("name", this.getName());
            jsonObject.put("type", this.getType());
            jsonObject.put("startDay", this.getStartDay());
            jsonObject.put("endDay", this.getEndDay());
            return jsonObject.toString().getBytes("UTF-8");
        } catch (Exception e) {
            Log.e("JSONException", e.getMessage());
        }
        return new byte[0];
    }

    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel parcel, int flags) {
        //  ParcelUtils.writeToParcel(parcel, getContent());//该类为工具类，对消息中属性进行序列化
        ParcelUtils.writeToParcel(parcel, getName());
        ParcelUtils.writeToParcel(parcel, getType());
        ParcelUtils.writeToParcel(parcel, getStartDay());
        ParcelUtils.writeToParcel(parcel, getEndDay());
    }
}



@ProviderTag(messageContent = QingJiaMessage.class,//（这里是你自定义的消息实体）
        showReadState = true)
public class QingJiaMessageProvider extends IContainerItemProvider.MessageProvider<QingJiaMessage> {

    @Override
    public void bindView(View view, int i, QingJiaMessage qingJiaMessage, UIMessage uiMessage) {
        ViewHolder holder = (ViewHolder) view.getTag();
        holder.tvName.setText(qingJiaMessage.getName() + "");
        holder.tvType.setText(qingJiaMessage.getType() + "");
        holder.tvEnd.setText(qingJiaMessage.getEndDay() + "");
        holder.tvStart.setText(qingJiaMessage.getStartDay() + "");
    }

    @Override
    public Spannable getContentSummary(QingJiaMessage qingJiaMessage) {
        return new SpannableString(qingJiaMessage.getName() + "");
    }
    @Override
    public void onItemClick(View view, int position, QingJiaMessage qingJiaMessage, UIMessage uiMessage) {
}
    @Override
    public View newView(Context context, ViewGroup viewGroup) {
        View view = LayoutInflater.from(context).inflate(R.layout.item_qingjia, null);
        ViewHolder holder = new ViewHolder();
        holder.tvName = (TextView) view.findViewById(R.id.tv_name);
        holder.tvType = (TextView) view.findViewById(R.id.tv_qingjialeixing);
        holder.tvStart = (TextView) view.findViewById(R.id.tv_start);
        holder.tvEnd = (TextView) view.findViewById(R.id.tv_end_day);
        view.setTag(holder);
        return view;
    }

    private static class ViewHolder {
        public TextView tvName, tvType, tvStart, tvEnd;
    }

}

public class QingJiaPlugin implements IPluginModule {
    @Override
    public Drawable obtainDrawable(Context context) {
        return context.getResources().getDrawable(R.mipmap.ic_launcher);
    }

    @Override
    public String obtainTitle(Context context) {
        return "请假";
    }

    @Override
    public void onClick(Fragment fragment, RongExtension rongExtension) {
        QingJiaMessage qingJiaMessage = QingJiaMessage.obtain("张三", "年假", "2018412", "2018413");
        //发送消息
        RongIM.getInstance().sendMessage(Conversation.ConversationType.PRIVATE, rongExtension.getTargetId(), qingJiaMessage, "请假条", "99999999999999", new RongIMClient.SendMessageCallback() {
            @Override
            public void onError(Integer integer, RongIMClient.ErrorCode errorCode) {
                Log.e("tagError",errorCode.getMessage()+"");
            }
            @Override
            public void onSuccess(Integer integer) {
                Log.e("tagSu",integer+"");
            }
        });
    }

    @Override
    public void onActivityResult(int i, int i1, Intent intent) {
        // 获取回调的信息，然后用sendMessage发送出去
    }

}

这个是扩展后的底部面板代码：
public class MyExtensionModule extends DefaultExtensionModule {
    @Override
    public List<IPluginModule> getPluginModules(Conversation.ConversationType conversationType) {
        List<IPluginModule> pluginModules =  super.getPluginModules(conversationType);
        pluginModules.add(new QingJiaPlugin());
        return pluginModules;
    }
}

最后在application里面进行注册面板，把下面这个写在初始化融云之后
private void rongYunPlugin() {
    List<IExtensionModule> moduleList = RongExtensionManager.getInstance().getExtensionModules();
    IExtensionModule defaultModule = null;
    if (moduleList != null) {
        for (IExtensionModule module : moduleList) {
            if (module instanceof DefaultExtensionModule) {
                defaultModule = module;
                break;
            }
        }
        if (defaultModule != null) {
            RongExtensionManager.getInstance().unregisterExtensionModule(defaultModule);
            RongExtensionManager.getInstance().registerExtensionModule(new MyExtensionModule());
        }
    }
}
```
>>未读消息监听：
//设置未读消息数变化监听器。
```java
mTagUnread = new IUnReadMessageObserver() {
    @Override
    public void onCountChanged(int i) {
        //聊天的tab显示未读消息的数量   
    }
};
RongIM.getInstance().addUnReadMessageCountChangedObserver(mTagUnread, Conversation.ConversationType.PRIVATE,
        Conversation.ConversationType.GROUP,
        Conversation.ConversationType.PUBLIC_SERVICE,
        Conversation.ConversationType.APP_PUBLIC_SERVICE);
        
    
```
>>会话界面操作监听：
```java


  RongIM.setConversationListBehaviorListener(new RongIM.ConversationListBehaviorListener() {
          
            
                @Override
            public boolean onConversationPortraitClick(Context context,  Conversation.ConversationType conversationType, String s) {
                return false;
            }

            @Override
            public boolean onConversationPortraitLongClick(Context context, Conversation.ConversationType conversationType, String s) {
                return false;
            }

            @Override
            public boolean onConversationLongClick(Context context, View view, UIConversation uiConversation) {
                return false;
            }

            @Override
            public boolean onConversationClick(Context context, View view, UIConversation uiConversation) {
                return false;
            }
        });
```








