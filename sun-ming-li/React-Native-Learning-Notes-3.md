进行项目开发，不可避免的就会进行代码调试，本文主要介绍了React native 安卓模拟器的安装及RN代码的调试方法。

### 1. ios模拟器的安装 

在学习笔记（一）React Native环境安装与应用创建着重介绍了React Native ios开发环境的安装步骤，其中有一项就是安装ios的开发环境Xcode。

XCode中自带ios模拟器。在终端中进入到RN项目根目录下，输入react-native run-ios就可以启动ios模拟器了。

### 2. android模拟器安装

在学习笔记（一）React Native环境安装与应用创建中介绍了进行React Native Android开发需要安装的内容，创建RN项目使用Webstorm开发RN项目后，我们使用Genymotion来运行安卓模拟器。安装步骤如下：

##### （1）安装 免费VirtualBox虚拟机

安装到最后一步如果安装失败出现上面的错误提示，进行如下操作即可安装成功：

1、Download VirtualBox5.2 installer

2、Run the DMG, this creates a device

3、Attempt to install from .pkg file, it will fail at the validation step

4、Close installer and run the uninstaller.tool file. DO NOT DELETE THE INSTALLER DEVICE

5、Go to System Preferences -&gt; Security and Privacy -&gt; General and approve the blocked software from ‘Oracle America’

6、Run the install from the same .pkg file, it should now complete successfully

##### （2）去Genymotion的官网注册账号（填入正确邮箱会发链接验证、genymotio安装后需要用此账号登录）后下载，下载时可以选择“Genymotion for personal use”免费使用。

##### （3）安装完登录后，添加自己的虚拟设备，点击+按钮，选择自己需要的设备下载下来。

##### （3）第二个操作点击settings，选择adb设置Android SDK路径，查看路径的方法：在终端中输入echo $ANDROID\_HOME 命令，如果没有路径输入，就要检查下自己的Android Studio有没有安装成功，或者安装成功后路径有没有配置成功。

##### （4）在Genymotion中选择需要的模拟器，点击Start按钮运行起来。在RN项目路径下输入react-native run-android命令，即可实现RN代码在Android模拟器上的运行。可以在终端中输入adb命令，查看当前运行的安卓模拟器。

### 3.RN项目调试工具和技巧

[此篇文章](https://blog.csdn.net/quanqinyang/article/details/52215652)详细介绍RN的调试方法，本文不再赘述。

参考资料

[https://blog.csdn.net/quanqinyang/article/details/52215652](https://blog.csdn.net/quanqinyang/article/details/52215652)

[https://www.cnblogs.com/hello-lijj/p/8073236.html](https://www.cnblogs.com/hello-lijj/p/8073236.html)

[https://github.com/jaywcjlove/handbook/blob/master/Android/React-native Android环境搭建.md](https://github.com/jaywcjlove/handbook/blob/master/Android/React-native Android环境搭建.md)

[https://www.jianshu.com/p/cca65a599015](https://www.jianshu.com/p/cca65a599015)

[https://blog.csdn.net/u012369271/article/details/74002466](https://blog.csdn.net/u012369271/article/details/74002466)

