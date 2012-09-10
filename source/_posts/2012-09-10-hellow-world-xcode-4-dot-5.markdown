---
layout: post
title: "Hello world xcode 4.4.1"
date: 2012-09-10 16:22
comments: true
categories: iOS
---

升级了山猫之后，XCode也升级到了相应的4.4.1，作为一个很严重的拖延症患者，学习Objective-c的进度还没有XCode的版本更新来的快，Things上也已经堆积了10几个代办事项了。给自己定了个目标每周至少更新一篇学习的记录。

目前Xcode已经是4.4.1，SDK的版本是IOS5，虽然网上HelloWorld教程很多，那个斯坦福的教学视频也很不错，但是大多基于XCode3.*+SDK3.0，很多东西已经完全不一样了，创建项目也变了。

打开XCode4.4

![xcode](http://farm9.staticflickr.com/8460/7969579324_3c37dd48ce_b.jpg)

新建项目

![新建项目](http://farm9.staticflickr.com/8459/7969658732_c80d215fba_b.jpg)

选择“single view Application”，以前版本的window-based/View-based Application都已经没有了，点击“Next”

![新建项目](http://farm9.staticflickr.com/8179/7969662006_fa236eb57e_b.jpg)

这里“Use Storyboards”和“Use Automatic Reference Counting”是新增的特性，如果选择“Use Storyboard”，XCode会创建.storyboard文件，不选则默认创建.xib文件，Storyboard故事板提供一种全新的方式来定义应用程序的用户界面，具体怎么用还不清楚。“Use Automatic Reference Counting”是IOS5.0开始支持的ARC，编译器都帮你写了retain和release的代码。

点击“Next”，选择项目保存的路径，一个HelloWorld项目就创建好了，XCode会自动生成项目的框架文件。

![框架](http://farm9.staticflickr.com/8435/7969915522_4e7abc6761_o.png)

.h和.m就是配对好的接口文件和实现文件。XCode集成了git，管理代码很方便。这时候一行代码都不写已经可以运行，运行效果只有一个灰色的背景。

先从Object Library中拖2个控件到View中

![控件](http://farm9.staticflickr.com/8299/7969950278_bd0390ae0f_b.jpg)

然后需要把控件绑定到oulet变量，点击右上角“Show the Assistant editor”按钮，选中Label，按住Ctrl，拖动控件到右边ViewController.h头文件中

![绑定](http://farm9.staticflickr.com/8300/7970028722_4043bbe2c6_o.jpg)

同样的方法绑定Button按下事件

![绑定](http://farm9.staticflickr.com/8311/7970028930_ae148a9741_o.jpg)

``` c++ ViewController.h
//
//  ViewController.h
//  HelloWorld
//
//  Created by Phil on 12-8-24.
//  Copyright (c) 2012年 Phil. All rights reserved.
//

#import <UIKit/UIKit.h>

@interface ViewController : UIViewController{
    UILabel *lblHello;
}

@property (strong, nonatomic) IBOutlet UILabel *lblHello;
- (IBAction)sayHello;

@end
``` 
修改事件代码sayHello

``` c++ ViewController.m
- (IBAction)sayHello {
    NSString *strMessage = [NSString stringWithFormat:@"Hello iPhone"];
    lblHello.text = strMessage;
    NSLog(@"字符串长度:%d", [strMessage length]);
    if ([strMessage compare:@"Hello iPhone"]) {
        NSLog(@"equal");
    }
}
```
点击运行（Command+r)，Hello World完成。

![运行](http://farm9.staticflickr.com/8460/7970047214_0e8da9c461.jpg)





