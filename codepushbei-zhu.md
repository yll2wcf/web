## 打开助手

```
code-push release-react dakaIOS ios --plistFile ./ios/DaKaNew/Info.plist -m --description "Modified the header color" --bundleName index.ios.jsbundle
```

`appName` : dakaIOS

`plistFile` : Info.plist 文件目录

`m` : 强制更新

`description` : 版本描述

`bundleName` : 编译导出的bundle名称（要与bundle名称设置一直）

`deploymentName` : 默认是Staging，如果要部署到生产环境需要指定为 `Production`

