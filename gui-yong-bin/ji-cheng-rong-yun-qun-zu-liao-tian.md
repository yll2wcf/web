集成融云群组聊天：

融云的群组聊天需要自己准备服务器维护自己的app里的群关系。APP端的对群操作都通过请求访问服务器来完成。

要确定已经添加了接收会话的类型里有群组，会话列表才会接收跟群组有关的消息。
```java
   Uri uri = Uri.parse("rong://" + getApplicationInfo().packageName).buildUpon()
                    .appendPath("conversationlist")
                    .appendQueryParameter(Conversation.ConversationType.PRIVATE.getName(), "false") //设置私聊会话是否聚合显示
                    .appendQueryParameter(Conversation.ConversationType.GROUP.getName(), "false")     .build();//群组
   ```
>>发送通知消息（小灰条）

用来发送在聊天会话页面显示的提示条（小灰条）通知。

消息类名：InformationNotificationMessage

消息 ObjectName：RC:InfoNtf

消息状态行为标识：MessageTag.ISPERSISTED

消息的结构：{"message":"请在聊天中注意人身财产安全",extra:""}
```java
  InformationNotificationMessage msg = new InformationNotificationMessage(words);
        msg.setMessage(words);
        msg.setExtra(extra);

  RongIMClient.getInstance().sendMessage(Conversation.ConversationType.GROUP, groupId, msg, null, null, new IRongCallback.ISendMessageCallback() {
            @Override
            public void onAttached(Message message) {
                // 消息成功存到本地数据库的回调
            }

            @Override
            public void onSuccess(Message message) {
                // 消息发送成功的回调
                Log.e("tagMsgSuccess", message.getContent() + "");
                refreshConversation();
                refreshConversationList();
            }

            @Override
            public void onError(Message message, RongIMClient.ErrorCode errorCode) {
                // 消息发送失败的回调
                Log.e("tagMsgError", errorCode + "");
            }
        });
```
   
>>群组@别人
首先在rc_config.xml 里开启@功能 <bool name="rc_enable_mentioned_message">true</bool>
其次要给融云设置当前群组的联系人
打卡助手里把这个方法 放在ConversationActivity的oncreate()里就可以了。这个是用来请求群组里的全部联系人，并设置给融云的，还有对应的点击事件


```java

private void setGroupMemberProvider() {

        if (mConversationType.equals(Conversation.ConversationType.GROUP)) {
            final ArrayList<UserInfo> userInfos = new ArrayList<>();


            ApiRequestManager.createApi().getGroupInfo(mTargetId)
                    .compose(ApiRequestManager.<GroupChatDetailInfo>applySchedulers())
                    .subscribe(new AbsAPICallback<GroupChatDetailInfo>(ConversationActivity.this, true) {
                        @Override
                        public void onNext(GroupChatDetailInfo info) {

                            final List<GroupChatDetailInfo.DataBean.UsersBean> users = info.getData().getUsers();

                            if (users != null && users.size() > 0) {
                                for (int i = 0; i < users.size(); i++) {
                                    GroupChatDetailInfo.DataBean.UsersBean user = users.get(i);
                                    UserInfo userInfo = new UserInfo(user.getEmployee_id(), user.getName(), Uri.parse(user.getPortraitUri()));
                         
                        
                                    userInfos.add(userInfo);
                                }

                            }

                            RongIM.getInstance().setGroupMembersProvider(new RongIM.IGroupMembersProvider() {
                                @Override
                                public void getGroupMembers(String groupId, RongIM.IGroupMemberCallback callback) {
                                    callback.onGetGroupMembersResult(userInfos);
                                }
                            });
                        }
                    });


            RongMentionManager.getInstance().setMentionedInputListener(new IMentionedInputListener() {
                @Override
                public boolean onMentionedInput(Conversation.ConversationType conversationType, String s) {
                    Intent intent = new Intent(ConversationActivity.this, ShowGroupMembersToAtActivity.class);

                    intent.putParcelableArrayListExtra("group_members", friends);

                    startActivity(intent);
                    return true;
                }
            });
        }
    }

```

请求到之后设置给融云,用的是下面这个方法。
```java
  
    RongIM.getInstance().setGroupMembersProvider(new RongIM.IGroupMembersProvider() {
                                @Override
                                public void getGroupMembers(String groupId, RongIM.IGroupMemberCallback callback) {
                                    callback.onGetGroupMembersResult(userInfos);
                                }
                            });
```
如果希望跳转到自己定义要@别的联系人列表。开启自定义的时候，一定要返回true。默认的是返回false,会跳转到融云默认的界面里选择联系人。
```java
RongMentionManager.getInstance().setMentionedInputListener(new IMentionedInputListener() {
                @Override
                public boolean onMentionedInput(Conversation.ConversationType conversationType, String s) {
                    Intent intent = new Intent(ConversationActivity.this, ShowGroupMembersToAtActivity.class);

      

                    startActivity(intent);
                    return true;
                }
            });
```























                    
   
                    