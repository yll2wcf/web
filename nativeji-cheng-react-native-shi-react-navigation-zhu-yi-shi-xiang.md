# navigation 问题 {#navigation-问题}

ios端 每一个界面都是一个单独的`ViewController`，但是react-native中的渲染任何的内容都是在一个`ViewController`中，本质上react-native就是个`root-view`，那么在打卡助手中的导航器交互的拓展性会存在隐患？

假设我们从`ViewController`中，到跳转到 react-native开发的 员工管理界面。

![](https://xuanhescript.gitbooks.io/dakaaddition-issues/content/assets/import.png)

这是我们在react-native注册的组件名称`RNHighScores`

![](https://xuanhescript.gitbooks.io/dakaaddition-issues/content/assets/2.png)那么我们在ios中需要实例化一个moduleName 为 RNHighScores 的 RCTRootView，然后这个view实例化的内容是整个MyApp，如下图。

![](https://xuanhescript.gitbooks.io/dakaaddition-issues/content/assets/3.png)

那么在react-native端我们实例化的永远都是App这个组件，也就是`react-navigation`这个导航器，并且initialRouteName永远是`IndexPage`。其实在正常场景下的跳转是没有问题的，但是如果遇到native想要固定调起某个react-native下的route就会无法实现。

如果上面描述的问题成立的话，有2种解决方案

1.每个放弃react-navigation，使用`AppRegistry.registerComponent`注册多个`RCTRootView`，然后实现跳转需要依赖native代码去跳转不同的`ViewController`。这样的缺点是js代码无法自由的跳转到任意的界面，需要依赖native提供方法

2.保留react-navigation，利用react-navigation的`initialRouteName`属性，在native端实例化rootView的需要传入`initialProperties`参数，js通过参数中的initialRouteName来处理渲染哪个route。这样js依然可以在react-navigation内部使用js去完成跳转。

参数也可以通过`initialRouteParams`来设置。

例如：

```swift
- (IBAction)highScoreButtonPressed:(id)sender {
    NSLog(@"High Score Button Pressed");
    NSURL *jsCodeLocation = [NSURL URLWithString:@"http://localhost:8081/index.bundle?platform=ios"];

    RCTRootView *rootView =
      [[RCTRootView alloc] initWithBundleURL: jsCodeLocation
                                  moduleName: @"RNHighScores"
                           initialProperties:
                             @{
                               @"initialRouteName" : @"ScanResultPage",
                               @"initialRouteParams" : @{
                                 @"id": 1
                               }
                             }
                               launchOptions: nil];
    UIViewController *vc = [[UIViewController alloc] init];
    vc.view = rootView;
    [self presentViewController:vc animated:YES completion:nil];
}

```

```jsx
export default class Root extends Component {
    render() {
        const {
            initialRouteName,
            initialRouteParams,
        } = this.props
        const Container = App({ initialRouteName, initialRouteParams})
        return (
            <View style={{flex:1}}>
                <Container/>
            </View>
        );
    }
}
```



