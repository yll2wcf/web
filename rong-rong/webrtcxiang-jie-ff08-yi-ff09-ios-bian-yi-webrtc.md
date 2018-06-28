###WebRTC详解 (一)iOS编译WebRTC

webRTC（Web Real-Time Communication）是实现实时语音对话或视频对话的技术。2011年5月开放了工程的源代码，在行业内得到了广泛的支持和应用，成为下一代视频通话的标准。现在主流的即时通信、音视频对话等功能基本都是基于该技术进行开发的。

使用webRTC技术前，需要在本地搭建webRTC环境、编译代码等一系列的操作，本文主要介绍iOS鍴如何对其进行编译。iOS编译需要以下几个步骤：

####一. 前期准备
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

####二. 获取WebRTC源码
#####1. 创建目录
在我们的编译工作目录webrtc_build下创建一个webtrtc子目录来存放代码：

```
mkdir webrtc  
cd webrtc

```
#####2. 获取源码

```
fetch --nohooks webrtc_ios 
gclient sync
```

漫长的代码下载后能会获取到一个src文件目录，如下图

![](/assets/屏幕快照 2018-06-28 上午10.12.09.png)

#####3. 更新源码


```
// 更新源码 
cd src 
$git fetch 
$git pull 
```
#####3. 更新编译工具

```
// 更新编译工具 
$cd .. 
$gclinet sync
```

####三. 编译成可使用的Framework
这里的编译过程就和普通编译framework的方式一样，由于篇幅有限就不多介绍了，这里主要介绍一些可能遇到的问题，供大家参考。

将WebRTC.framework导入工程后，运行会报错：

dyld: Library not loaded: @rpath/WebRTC.framework/WebRTC  Referenced from: /Users/MFJun/Library/Developer/CoreSimulator/Devices/4441D192-28A0-46AF-9CA9-C8945BB3442C/data/Containers/Bundle/Application/34E51961-4516-4478-AA59-579342D8D3A5/WebScoketTest.app/WebScoketTest  Reason: image not found 

仔细看错误原因，是没有找到framework文件包，解决方法是：
TARGETS -> Build Phases -> New Copy Files
![](/assets/03.png)

接着再点击下面+号，选择自定义的framework
![](/assets/04.png)

在arm64的环境下，运行会报错
![](/assets/05.png)

把TARGETS -> Build Settings -> Enable Bitcode设置为NO
![](/assets/06.png)

最后，Build Success！

