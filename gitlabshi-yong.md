## 目前创建的卓朗移动团队的gitlab地址：[http://172.26.1.194/](http://172.26.1.194/)

第一访问我们需要先注册用户

![](/assets/lj3.png)

进入gitlab后我们需要尝试创建一个project

![](/assets/lj5.png)

对应填写创建所需要的信息

![](/assets/lj6.png)包括项目名称，描述，项目权限（私人的，内部访问，开源的）

![](/assets/lj7.png)

现在我们创建了项目，如果你访问的这台电脑没有上传过SSH keys的话，会有这种提示

![](/assets/lj8.png)

这时候我们需要添加SSH Keys，这里有[帮助文档](http://172.26.1.194/help/ssh/README)

以macos为例：输入

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



