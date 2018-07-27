### 目录说明：

#### 所有的react-native的code统一放置在 `src` 目录下

#### `src` 目录结构如下：

* #### `Root.js`  react-native注册入口
* #### `store`  redux的store创建入口
* #### `containers`  react-native容器渲染入口
* #### `pages`  界面存放目录
* #### `components`  组件存放目录
* #### `actions`  redux action 存放目录，相当于mvc的modal或者行为层
* #### `sagas`  redux 中间件的存放地址，用来处理请求或者action传递的数据，完成reducer的更新
* #### `reducers`  决定store的state结构，用于搭建state结构使用
* #### `constants`  常量目录，用于存放action的type常量和saga的type常量
* #### `utils`  工具类存放目录，目前主要包含 fetch,theme等
* #### `assets`  静态资源存放目录，包含图片等

#### containers 内部结构说明：

* #### `index.js`  容器导出入口，用作实例化导航器，app初始化方法等
* #### `navigator.js`  导航器配置文件
* #### `authLoadingScreen.js`  初始化导航器验证界面



