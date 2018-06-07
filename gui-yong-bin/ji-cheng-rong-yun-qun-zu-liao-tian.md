集成融云群组聊天：

融云的群组聊天需要自己准备服务器维护自己的app里的群关系。APP端的对群操作都通过请求访问服务器来完成。

要确定已经添加了接收会话的类型里有群组，会话列表才会接收跟群组有关的消息。
```java
   Uri uri = Uri.parse("rong://" + getApplicationInfo().packageName).buildUpon()
                    .appendPath("conversationlist")
                    .appendQueryParameter(Conversation.ConversationType.PRIVATE.getName(), "false") //设置私聊会话是否聚合显示
                    .appendQueryParameter(Conversation.ConversationType.GROUP.getName(), "false")     .build();//群组
   ```
                    
   
                    