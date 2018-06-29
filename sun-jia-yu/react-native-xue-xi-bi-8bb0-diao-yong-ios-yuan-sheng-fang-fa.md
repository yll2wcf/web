##React-Native调用iOS原生方法

1、导入RCTBridgeModule头文件
>`#import <React/RCTBridgeModule.h>`

2、引入协议
>`#import <Foundation/Foundation.h>#import ` `<React/RCTBridgeModule.h>`
`@interface NativeTest : NSObject<RCTBridgeModule>`
`@end`

3、导出模块和方法
>`#import "NativeTest.h"`
`@implementation NativeTest`
// 导出模块，不添加参数即默认为这个类名
RCT_EXPORT_MODULE();
// 导出方法，桥接到js的方法返回值类型必须是void
`RCT_EXPORT_METHOD(doSomething:(NSString *)testStr){`
`  NSLog(@"%@ ===> doSomething",testStr);`
`}`
`@end`

4、在React-Native调用
>// 创建原生模块
  `  var NativeTest = require('react-``native').NativeModules.NativeTest;`
    // 方法调用
 `   NativeTest.doSomething('zw name');`

带有回调的方法

1、Response

>`#pragma mark - Response`// 导出方法，桥接到js的方法返回值类型必须是void/* 回调参数必须为两个，第一个为状态，第二个为参数 */
`RCT_EXPORT_METHOD(doSomething:(NSString *)testStr resolver:``(RCTResponseSenderBlock)callback){`
  `NSLog(@"%@ ===> doSomething",testStr);`
`  NSString *callbackData = @"Callback`数据"; //准备回调回去的数据
 ` callback(@[[NSNull null],callbackData]);`
`}`
调用方法：
`clickAction(){`
    // Response 调用方式
    // 创建原生模块
    `var NativeTest = require('react-``native').NativeModules.NativeTest;`
    // 方法调用
  `  NativeTest.doSomething(('RN->原生的数据'),(error,events) => {`
        if (error) {
            console.warn(error);
        } else {
            alert(events)//返回的数据
        }
    });
  }

2、Promise

>`pragma mark - Promise`
// 导出方法，桥接到js的方法返回值类型必须是void/*有两个回调，一个为正确的，一个为error*/
`RCT_REMAP_METHOD(testPromisesEvent,name:(NSString *)testStr`
                ` resolver:(RCTPromiseResolveBlock)resolve`
            `     rejecter:(RCTPromiseRejectBlock)reject)`
{
 ` NSString *PromisesData = @"Promises`数据"; //准备回调回去的数据
 ` if (PromisesData) {`
    resolve(testStr);
  } else {
    NSError *error=[NSError errorWithDomain:@"我是Promise回调错误信息..." code:101 userInfo:nil];
    reject(@"no_events", @"There were no events", error);
  }
}
调用方法：
`clickAction2(){`
    // Promise 调用方式
    // 创建原生模块
    var NativeTest = require('react-native').NativeModules.NativeTest;
    // 方法调用
    NativeTest.thePromisesTest(('zw')).then((events)=>{
        alert(events+1111)
    });
  }

