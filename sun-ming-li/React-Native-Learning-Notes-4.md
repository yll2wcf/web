本片将文章介绍了将React Native项目打包ipa的过程。

1. ##### 打开React Native项目文件，在根目录下找到package.json文件，打开在 "scripts"加入生成ios bundle资源文件的指令：

"bundle-ios":"react-native bundle --entry-file index.js --platform ios --dev false --bundle-output ./ios/bundle/index.ios.jsbundle --assets-dest ./ios/bundle"

--entry-file ,ios或者android入口的js名称，比如index.js

--platform ,平台名称\(ios或者android\)

--dev ,设置为false的时候将会对JavaScript代码进行优化处理。

--bundle-output, 生成的jsbundle文件的名称，比如./ios/bundle/index.ios.jsbundle

--assets-dest 图片以及其他资源存放的目录,比如./ios/bundle

![](/assets/11.png)

##### 2.在React Native项目ios文件夹下创建一个空的bundle文件夹，如果之前打过包不是空的最好清空一下。

##### 3.打开终端进入到React Native项目的跟路径下，执行npm run bundle-ios命令，执行成功后生成如下三个文件

![](/assets/12.png)![](/assets/16.png)

##### 4.用Xcode打开ios工程，将assets文件夹和index.ios.jsbundle文件，_拖到工程下一定要选择Create folder references的方式，添加完是蓝色文件夹图标_，否则的话即使打包成功，我们使用bundle资源包运行ios项目时也会出现图片加载不出来的情况。

![](/assets/13.png)

##### 5.再进入AppDelegate.m文件中将jsCodeLoaction定位到bundle资源

![](/assets/14.jpg)

##### 6.选中工程Edit Scheme将Run 和Achive的Build Configuration改为release，打包ipa即可。

![](/assets/15.png)

![](/assets/16.png)

#### 

#### ios打包注意事项：

####  1.离线包如果开启了chrome调试，会访问调试服务器，而且会一直loading出不来。

#### 2.如果bundle的名字是main.jsbundle,app会一直读取旧的,改名就好了。。。非常奇葩的问题，我重新删了app，clean工程都没用，就是不能用main.jsbundle这个名字。

#### 3.必须用Create folder references【蓝色文件夹图标】的方式引入图片的assets，否则引用不到图片

#### 4.执行bundle命令之前，要保证相关的文件夹都存在



参考文章：[https://blog.csdn.net/xu\_song/article/details/52870593](https://blog.csdn.net/xu_song/article/details/52870593)

```
              https://www.jianshu.com/p/7bdf90d9f966
```



