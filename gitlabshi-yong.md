## 目前创建的卓朗移动团队的gitlab地址：[http://172.26.1.194/](http://172.26.1.194/)

#### 第一次访问我们需要先注册用户

#### ![](/assets/lj3.png)

#### 进入gitlab后我们需要尝试创建一个project

#### ![](/assets/lj5.png)

#### 对应填写创建所需要的信息

#### ![](/assets/lj6.png)包括项目名称，描述，项目权限（私人的，内部访问，开源的）

![](/assets/lj7.png)

#### 现在我们创建了项目，如果你访问的这台电脑没有上传过`SSH keys`的话，会有这种提示

#### ![](/assets/lj8.png)

#### 这时候我们需要添加`SSH Keys`，这里有[帮助文档](http://172.26.1.194/help/ssh/README)

#### 以macos为例：输入

```
cat ~/.ssh/id_rsa.pub
```

来验证当前本机是否创建过SSH Key

如果没有创建过，输入

```
ssh-keygen -t rsa -C "your.email@example.com" -b 4096
```

接下来，系统将提示您输入文件路径以保存SSH密钥对。

输入文件路径后，系统将提示您输入密码以保护SSH密钥对。

下一步是复制公共SSH密钥，因为我们之后将需要它。

```
pbcopy < ~/.ssh/id_rsa.pub
```

最后一步是将您的公共SSH密钥添加到GitLab。

![](/assets/lj9.png)

#### 我们添加`SSH Key`后就可以正常拉取项目了

#### 在这里推荐使用SourceTree用来管理git项目

![](/assets/lj10.png)

![](/assets/lj11.png)

#### 这里我们填入git地址

#### ![](/assets/lj12.png)

#### 会要求我们输入用户名密码，这里我们输入登陆gtilab的用户密码就可以

#### clone后，我们的项目就被拉取到了本地 ![](/assets/lj13.png)

#### 关于权限的分配，我们可以在`menbers`下进行分配

#### ![](/assets/lj14.png)

#### 不同的身份，权限不同，[在这里可以查看身份对应的权限](http://172.26.1.194/help/user/permissions)

#### 如果每一个项目都要设置一次权限就非常不又好了，所以我们可以利用`group`来创建组

#### ![](/assets/lj15.png)创建了group后，未来我们的项目在创建时可以都选择创建在group下

#### ![](/assets/lj16.png)

#### 这样组内的成员都具有了查看这个项目的权限，我们只需要再修改组内成员的权限即可。

#### 这些只是gitlab的冰山一角，gitlab还具有，插件，持续集成，及时部署等功能。



