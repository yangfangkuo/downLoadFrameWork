## downLoadFrameWork 

 
####  支持热更新,动态更新,xcode 6 之后，苹果开放了 ios 的动态库编译权限。所谓的动态库。事实上就是能够在执行时载入。正好利用这一个特性，用来做ios的热更新。 

#### 即开发者在自己的开发项目中,留下动态展示项目的入口,一旦自己的项目某个类或者模块出了问题,即可在AppDelegate启动的时候,对比服务器上动态库的版本,如果需要更新,就可以去下载新的动态库(压缩版),然后再下载下来解压,在系统运行,动态的额替换原来项目的模块,起到修复bug和发布新版的功能

### JKDylib的.h文件
``` javascript
//
//  JKDylib.h
//  JKDylb
//
//  Created by yangfangkuo on 16/7/16.
//  Copyright (c) 2016年 yangfangkuo. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface JKDylib : NSObject

-(void)showViewAfterVC:(id)fromVc inBundle:(NSBundle*)bundle;

@end
```
### JKDylib的.m文件
``` javascript

//
//  JKDylib.m
//  JKDylb
//
//  Created by yangfangkuo on 16/7/16.
//  Copyright (c) 2016年 yangfangkuo. All rights reserved.
//

#import "JKDylib.h"
#import "JKViewController.h"

@implementation JKDylib

-(void)showViewAfterVC:(id)fromVc inBundle:(NSBundle*)bundle
{
    if (fromVc == nil ) {
        return;
    }
    
    JKViewController *vc = [[JKViewController alloc] init];
    UIViewController *preVc = (UIViewController *)fromVc;
    
    if (preVc.navigationController) {
        [preVc.navigationController pushViewController:vc animated:YES];
    }
    else {
        UINavigationController *navi = [[UINavigationController alloc] init];
        [navi pushViewController:vc animated:YES];
    }
    
}
@end
```
#### 上述代码意图很明显， 就是调用该动态库的时候
``` javascript

-(void)showViewAfterVC:(id)fromVc inBundle:(NSBundle*)bundle
```

在该函数中，创建一个viewController 然后使用mainBundler 的navigationController  push 新建的viewController，显示动态库的ui界面。
而动态库中的JKViewController 内容则能够依据须要随便定义。

#### 参考链接 http://www.bubuko.com/infodetail-2026817.html
