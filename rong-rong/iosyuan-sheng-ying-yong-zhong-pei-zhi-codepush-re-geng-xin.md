###iOS原生应用中配置CodePush热更新

iOS集成CorePush配置前需要在RN项目中先行集成CodePush插件，然后才可进行配置，具体分为以下4个步骤：

* Xcode的项目导航视图中的PROJECT下选择你的项目，选择Info页签 ，在Configurations节点下单击 + 按钮 ，选择Duplicate "Release Configaration，输入Staging
![](/assets/088714c0-331c-11e6-9504-5469d9a59d74.png)
完成后效果如下图：

![](/assets/屏幕快照 2018-06-19 下午2.13.41.png)


*  选择Build Settings tab，搜索Build Location -> Per-configuration Build Products Path -> Staging，将之前的值：$(BUILD_DIR)/$(CONFIGURATION)$(EFFECTIVE_PLATFORM_NAME) 改为：$(BUILD_DIR)/Release$(EFFECTIVE_PLATFORM_NAME)

![](/assets/屏幕快照 2018-06-19 下午2.17.59.png)

* 选择Build Settings tab，点击 + 号，选择Add User-Defined Setting，将key设置为CODEPUSH_KEY，Release 和 Staging的值为前面创建的key，直接输入对应的key值

![](/assets/屏幕快照 2018-06-19 下午2.20.26.png)

* 打开Info.plist文件，在CodePushDeploymentKey中输入$(CODEPUSH_KEY)，并修改Bundle versions为三位

![](/assets/屏幕快照 2018-06-19 下午2.22.59.png)

至此，iOS平台CodePush环境集成完毕。

**注意**
集成之后iOS打静态包的时候需要命名为 main.jsbundle 