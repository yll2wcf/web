###WebRTC详解 (一)iOS编译WebRTC

webRTC（Web Real-Time Communication）是实现实时语音对话或视频对话的技术。2011年5月开放了工程的源代码，在行业内得到了广泛的支持和应用，成为下一代视频通话的标准。现在主流的即时通信、音视频对话等功能基本都是基于该技术进行开发的。

使用webRTC技术前，需要在本地搭建webRTC环境、编译代码等一系列的操作，本文主要介绍iOS鍴如何对其进行编译。iOS编译需要以下几个步骤：

####一.前期准备
#####1.安装 Git
* 下载：http://code.google.com/p/git-osx-installer/
* 创建一个全局用户名、全局邮箱作为配置信息

```
git config --global user.name "***" git config --global user.email "***@example.com"
```
* 安装成功后打开终端，执行cd ~进入根目录，输入命令ssh-keygen生成ssh-key，如果有提示，一直按回车，如下图
![](/assets/01.png)

* 将SSH key添加到GitHub。登录到GitHub页面，Account Settings->SSH Public Keys->Add another key
将生成的key(id_rsa.pub文件）内容copy到输入框中，save。
**注意：**id_rsa.pub在/Users/**/.ssh/目录里面可以看到

#####2.安装 depot_tools
* 创建一个目录专门来存放项目编译工具和项目代码仓库等，

**注意：** 确保该目录所在磁盘可用空间至少有8~10G.

* 打开系统的终端工具mkdir webrtc_build 

* 执行命令行
在执行下面命令之前,请确保你已经连上VPN已经FQ了，或者你已经给git单独配置了有效的socksFQ代理。开始安装depot_tools，这是一套Google用来编译Chromium或者WebRTC的构建工具，在我们后续的编译过程中也将使用它。

a.进入路径
```
cd webrtc_build 
```

b.克隆
```
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
```

c. 把 depot_tools 加入环境变量
```
vi ~/.bash_profile
```
PATH=${PATH}:depot_tool的绝对路径执行 ~/.bash_profile 

d. 使PATH设置生效:source ~/.bash_profile 
e. echo $PATH查看设置是否生效。

至此我们已经完成了所有前期准备工作，下面开始正式获取WebRTC源码。