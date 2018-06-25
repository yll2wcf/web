本片将文章介绍了将React Native项目打包ipa的过程。

##### 1. 打开React Native项目文件，在根目录下找到package.json文件，打开在 "scripts"加入生成ios bundle资源文件的指令：

"bundle-ios":"react-native bundle --entry-file index.js --platform ios --dev false --bundle-output ./ios/bundle/index.ios.jsbundle --assets-dest ./ios/bundle"

--entry-file ,ios或者android入口的js名称，比如index.js

--platform ,平台名称\(ios或者android\)

--dev ,设置为false的时候将会对JavaScript代码进行优化处理。

--bundle-output, 生成的jsbundle文件的名称，比如./ios/bundle/index.ios.jsbundle

--assets-dest 图片以及其他资源存放的目录,比如./ios/bundle

![](/assets/11.png)

##### 



