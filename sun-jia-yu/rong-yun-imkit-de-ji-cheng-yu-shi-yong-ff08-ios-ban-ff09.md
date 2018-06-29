##融云IMKit的基本使用
融云是一个比较强大的聊天三方框架。融云官方有两套API，IMLib和IMKit，其实IMLib需要我们自行聊天界面等，在不需要特别多的自定义界面推荐使用IMKit。
下面先讲解一下聊天的最基本用法
一、基本用法
>CocoaPods安装

•	融云 IM 通讯能力库 - RongCloudIM/IMLib
•	融云 IM 界面组件 - RongCloudIM/IMKit
•	融云音视频通讯能力库 - RongCloudIM/CallLib
•	融云音视频界面组件 - RongCloudIM/CallKit
•	融云红包 - RongCloudIM/RedPacket

开发人员只需要根据需求选择不同的SDK即可
例：`pod 'RongCloudIM/IMKit', '~> 2.8.3'` 

初始化
>在您需要使用融云 SDK 功能的类中，import 相关头文件
例：#import <RongIMKit/RongIMKit.h>
请使用您之前从融云开发者控制台注册得到的 App Key，通过 RCIM 的单例，传入 initWithAppKey:方法，初始化 SDK。
您在使用融云 SDK 所有功能（包括显示 SDK 中的 View 或者显示继承于 SDK 的 View ）之前，您必须先调用此方法初始化 SDK。 在 App 的整个生命周期中，您只需要将 SDK 初始化一次。
例：[[RCIM sharedRCIM] initWithAppKey:@"YourTestAppKey"];

连接服务器
>将您在上一步获取到的 Token，通过 RCIM 的单例，传入 -connectWithToken:success:error:tokenIncorrect: 方法，即可建立与服务器的连接。
例：

```
[[RCIM sharedRCIM] connectWithToken:@"YourTestUserToken"     success:^(NSString *userId) {
    NSLog(@"登陆成功。当前登录的用户ID：%@", userId);
} error:^(RCConnectErrorCode status) {
    NSLog(@"登陆的错误码为:%d", status);
} tokenIncorrect:^{
    //token过期或者不正确。
    //如果设置了token有效期并且token过期，请重新请求您的服务器获取新的token
    //如果没有设置token有效期却提示token错误，请检查您客户端和服务器的appkey是否匹配，还有检查您获取token的流程。
    NSLog(@"token错误");
}];
```

会话列表
>融云 IMKit 已经实现了一个默认的会话列表视图控制器，您直接使用或继承 RCConversationListViewController，即可快速启动和使用会话列表界面。    如下面的例子，新建一个 YourTestChatViewController Class，继承于 RCConversationListViewController。新建会话列表 View Controller新建一个继承于 YourTestChatViewController 的 Class，在 viewDidLoad 中设置需要显示的会话类型，需要将哪些类型的会话聚合显示。

```
例：//  YourTestChatListViewController.m
#import "YourTestChatListViewController.h"
@interface YourTestChatListViewController ()
@end

@implementation YourTestChatListViewController
- (void)viewDidLoad {
    //重写显示相关的接口，必须先调用super，否则会屏蔽SDK默认的处理
    [super viewDidLoad];
      //设置需要显示哪些类型的会话
      [self setDisplayConversationTypes:@[@(ConversationType_PRIVATE),
                                          @(ConversationType_DISCUSSION),
                                          @(ConversationType_CHATROOM),
                                          @(ConversationType_GROUP),
                                          @(ConversationType_APPSERVICE),
                                          @(ConversationType_SYSTEM)]];
      //设置需要将哪些类型的会话在会话列表中聚合显示
      [self setCollectionConversationType:@[@(ConversationType_DISCUSSION),
                                            @(ConversationType_GROUP)]];
}
@end
```

>初始化会话列表界面并显示
生成会话列表 View Controller 对象，并显示。

```
例：YourTestChatListViewController *chatList = [[YourTestChatListViewController alloc] init];
[self.navigationController pushViewController:chatList animated:YES];
```

>点击会话列表，进入聊天会话界面
在您的会话列表 View Controller 中加入以下代码，即可点击进入聊天会话界面。

>例：//重写RCConversationListViewController的onSelectedTableRow事件

```
- (void)onSelectedTableRow:(RCConversationModelType)conversationModelType
         conversationModel:(RCConversationModel *)model
               atIndexPath:(NSIndexPath *)indexPath {
    RCConversationViewController *conversationVC = [[RCConversationViewController alloc]init];
    conversationVC.conversationType = model.conversationType;
    conversationVC.targetId = model.targetId;
    conversationVC.title = @"想显示的会话标题";
    [self.navigationController pushViewController:conversationVC animated:YES];
}
```

会话页面
>融云 IMKit 中已经实现了完整的会话页面，包含发送、接收、更新等 UI，并覆盖常用的 IM 交互场景，您直接使用或继承 RCConversationViewController，即可快速启动和使用会话页面。
如下面的例子，创建一个 `RCConversationViewController` 对象并设置好会话类型、目标会话 ID，显示即可进行聊天。
//新建一个聊天会话`View Controller`对象,建议这样初始化

```
RCConversationViewController *chat = [RCConversationViewController alloc] initWithConversationType:conversationType
                    targetId:targetId];

```


>//设置会话的类型，如单聊、群聊、聊天室、客服、公众服务会话等

```
chat.conversationType = ConversationType_PRIVATE;
//设置会话的目标会话ID。（单聊、客服、公众服务会话为对方的ID，群聊、聊天室为会话的ID）
chat.targetId = @"targetIdYouWillChatIn";

//设置聊天会话界面要显示的标题
chat.title = @"想显示的会话标题";
//显示聊天会话界面
[self.navigationController pushViewController:chat animated:YES];

```


根据步骤做到这里就完成融云的基本聊天功能了，下面介绍一下我在集成融云的时候遇到的难点与解决方法，希望可以帮助到大家。
##二、可能出现的问题
1、	收不到远程推送
	>融云在app处于前台和处于后台并且未被销毁的时候使用长链接进行消息推送，而当app未运行或长时间处于后台状态的时候融云会进行远程消息推送，上线的时候切记要在融云网站上进行上线申请，并切换成融云生产Key。

2、	头像昵称问题
	>怎么替换头像和昵称，想要把app的头像和昵称替换，其实融云sdk提供了RCUserInfo类，只需要设置下相关信息就好，要根据不同的业务逻辑去实现相关的设置，实现了相关代码后没有实际的效果，查下下是否是kit和lib混用的结果所致，或者是实现的逻辑有问题。而我的项目中是在app登陆之后会自动发送请求登录融云的服务器，登录成功在发送通知去设置RCUserInfo。

3、	整体功能无法正常使用
	>RongCallKit和RongCallLib框架混用,直接混用的结果就是导致原本应该实现的方法无法显示相应的效果,查阅了很多资料和各种方式、方案都不起作用,在融云的工单里找到了相关提示,说是RongCallKit和RongCallLib不能混用,只能单一的使用,不然很多设置是不起作用的,相关的效果是无法显示的。如果在集成过程中遇到相似的情况,可以检查下kit和lib是否混用了。
