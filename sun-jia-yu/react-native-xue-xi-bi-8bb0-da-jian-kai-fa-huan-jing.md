##必需的软件
1.Homebrew

>Homebrew, Mac系统的包管理器，用于安装NodeJS和一些其他必需的工具软件。
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
在Max OS X 10.11（El Capitan)版本中，homebrew在安装软件时可能会碰到/usr/local目录不可写的权限问题。可以使用下面的命令修复：
sudo chown -R `whoami` /usr/local

2.Node
>使用Homebrew来安装Node.js.
React Native目前需要NodeJS 8.0或更高版本。本文发布时Homebrew默认安装的是最新版本，一般都满足要求。
brew install node
安装完node后建议设置npm镜像以加速后面的过程（或使用科学上网工具）。
不能使用cnpm！cnpm安装的模块路径比较奇怪，packager不能正常识别！
npm config set registry https://registry.npm.taobao.org --globalnpm config set disturl https://npm.taobao.org/dist --global

3.Yarn、React Native的命令行工具（react-native-cli）

>Yarn是Facebook提供的替代npm的工具，可以加速node模块的下载。React Native的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务。
npm install -g yarn react-native-cli
安装完yarn后同理也要设置镜像源：
yarn config set registry https://registry.npm.taobao.org --global
yarn config set disturl https://npm.taobao.org/dist --global
如果看到EACCES: permission denied这样的权限报错，那么请参照homebrew译注，修复/usr/local目录的所有权：
sudo chown -R `whoami` /usr/local
安装完yarn之后就可以用yarn代替npm了，例如用yarn代替npm install命令，用yarn add 某第三方库名代替npm install --save 某第三方库名。


