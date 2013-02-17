---
layout: post
title: "创建你的第二个iOS应用"
date: 2013-01-25 15:09
comments: true
toc: true
tocstartlv: 2
categories: iOS
---

翻译:Phil

英文原文：[Your Second iOS App](http://developer.apple.com/library/ios/#documentation/iPhone/Conceptual/SecondiOSAppTutorial/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011318-CH1-SW1)

#关于创建你的第二个iOS应用
{:toc}

您的第二个iOS应用：这个教程使用了串联图（Storyboards）来创建一个导航应用程序。你可以从主列表查看每个单项的详细信息，也可以创建新的单项到主列表。完成后的App长成这样：

<img src="http://developer.apple.com/library/ios/documentation/iPhone/Conceptual/SecondiOSAppTutorial/Art/finished_product_2x.png" width="199.5" height="389">

完成本教程后，你将了解：

* 设计一个model层来展现和控制应用的数据
* 在storyboard中创建新的场景和场景切换
* 从不同的场景中传递和展现数据

想更好的学习本教程，你应该已经对iOS应用开发和Objective-C语言有一定的了解。如果你是个新手，在开始学习本教程之前，可以先阅读 [马上着手开发iOS应用程序](http://developer.apple.com/library/ios/#referencelibrary/GettingStarted/RoadMapiOSCh/chapters/Introduction.html#//apple_ref/doc/uid/TP40012668)。

##概览

你的第二个iOS应用：Storyboards基于你在[第一个iOS应用](http://developer.apple.com/library/ios/#referencelibrary/GettingStarted/RoadMapiOSCh/chapters/RM_YourFirstApp_iOS/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40012668-TP40012323-CH1-SW1)积累的知识，这里将会想你介绍串联图（storyboards）提供的一些强大的功能以及一些table view的使用方法。

###设计并实现一个Model层

一个设计优雅的app需要有一个包含数据和管理数据对象的Model层。本教程中的Model层非常简单，设计和实现它将给你在以后更复杂的应用设计打下基础。

相关章节：“设计Model层”

###设计并实现Master/Detail Scenes

这是一个基于master-detail模式的应用，在这个模式下用户选择主列表的某一项后，屏幕会滑动显示这项的详细内容。很多iOS应用像Mail、Setting都是基于master-detail模式。这个教程将想你展示如何简单的使用串联图来构建这种模式。

相关章节：“设计并实现Master Scene”,"在详情场景显示信息"

###创建新场景

大部分Xcode模板在串联图里预设了一个或多个场景，你可以按需进行增删。在这个教程里面，你需要增加一个带导航栏的场景，让用户能够在主场景里面增加新信息。

相关章节：“添加新item”

###解决问题并思考下一步做什么

在创建这个应用过程中，你可能会碰到一些你不知道如何解决的问题。教程的最后会包括常见问题、代码清单来帮助你处理问题。

有很多方法来提高你的iOS应用开发技巧，教程最后还提供了一些学习新技巧的方法建议。

相关章节：“问题排查”，“代码清单”，“下一步”

<!--more-->

#开始

本教程需要Xcode 4.5和iOS SDK 6.0以上版本。Xcode包括iOS SDK、开发界面。

在本章你将创建新的Xcode项目，编译运行默认创建的项目，并了解如果通过串联图设置用户界面和行为。

##创建新项目

本教程的app会显示一个list，当你选择其中某一项时，会跳转新的屏幕。用户也可以添加新的内容。

列表视图和详情视图之前的过渡是iOS应用的设计模式中很常见的一部分，很多iOS应用允许用户在多个screen里一层层的向下查看数据。比如：Mail允许用户从最外面的用户列表一层层的点下去直到指定的邮件内容。

Xcode提供的Master-Detail模板使得创建类似的应用变得简单。

###创建新项目BirdWatching

1. 打开Xcode选择 File > New > Project
2. 在iOS选项对话框左侧选择Application
3. 在对话框中选择Master-Detail Application，点击继续，在弹出窗口中给你的app起个名字并选择一些附加选项
4. 在弹出框中填写以下信息：
	* 项目名称 BirdWatching
	* 公司名称 如果你有公司的话可以填写你公司的名字，没有的话也可以天Self
	* 公司标识 package标识，没有的话你也可以用edu.self标识用于学习 	* 类前缀 Birds
5. 在Devices下拉菜单中，确认iPhone选项被选中
6. 选中 Use Storyboards 和 Use Automatic Reference Counting 选项，不要选择Use Core Data和 Include Unit Test

当你填写完以上信息，对话框应该是这个样子：

<img src="http://developer.apple.com/library/ios/documentation/iPhone/Conceptual/SecondiOSAppTutorial/Art/options_dialog_filled_2x.png" width="391.5" height="305.5">
	
7. 点击 Next
8. 保存项目，不要选择Source Control选项。Xcode会打开一个名为workspace window的新项目窗口。

##编译运行默认项目

和别的template-based项目一样，你可以在正式写代码前编译运行他。这是个不错的注意，这有助于你理解这个模板提供的功能。

###在iOS Simulator中运行默认项目

1. 确保Xcode工具栏中的弹出选项菜单显示 BirdWatching > iPhone 6.0 Simulator。
2. 在工具栏中点击Run按钮（或选择Product > Run）

编译完成后，iOS Simulator会自动启动，iOS Simulator是一个iPhone模拟器程序，显示的窗口的样子和iPhone一样很直观。在这个模拟器的iPhone屏幕上，默认的app已经被打开，看上去是这个样子：

<img src="http://developer.apple.com/library/ios/documentation/iPhone/Conceptual/SecondiOSAppTutorial/Art/default_product_2x.png" width="209.5" height="398.5">

在这个默认的app上已经有了很多功能。比如：导航栏包括一个添加按钮（+）和一个编辑按钮，你可以主列表中添加和删除项。

注意：本教程暂时不使用编辑按钮

在模拟器中，点击添加按钮（+）可以在列表中添加一项（默认内容是当前日期和时间）。点击添加的项目会跳转到详情页。默认的详情页如下所示：

<img src="http://developer.apple.com/library/ios/documentation/iPhone/Conceptual/SecondiOSAppTutorial/Art/default_detail_screen_2x.png" width="209.5" height="398.5">

在详情页的坐上角可以看到一个返回按钮（Master），这个是master-detail模板的导航控制器自动创建的。点击按钮可以返回上一层屏幕。

在下一节，你将使用storboard来实现更多的用户交互。现在先退出iOS模拟器。

###在显示场景中检查Storyboard

主要是描述一些场景的操作，比如放大缩小视图等，不详述。

Master-detail默认的storyboard包括3个scence和2个segue。

Scene 是由view controller控制的在屏幕上显示内容的区域。view controller是控制一系列屏幕显示的对象。（这里scene、view controller是一个意思）最左边的scene显示“navigation controller”，navigation controller。navigation controller又称为view controller容器，除了它本身以外，他还管理了一些别的view controller。比如：在BirdWatching app里面，除了你运行app时看到的导航栏和返回按钮，navigation controller还管理着master/detail view controller。

Segue 表示了场景之间的过度方式（从源场景到目标场景）。比如：在默认项目里，master scene是源场景，detail scene是目标场景。当你测试app的时候，从master list点击某个项的时候会触发一个从源场景到目标场景的segue。在这个例子里，segue是一个push segue，这个segue的效果是目标场景会从屏幕右侧滑出覆盖源场景。push segue的图标如下所示：

<img src="http://developer.apple.com/library/ios/documentation/iPhone/Conceptual/SecondiOSAppTutorial/Art/segue_icon_2x.png" width="27.5" height="27.5">

当你在两个场景中创建segue后，你可以在他们之间传递数据。关于如何传递数据，可参考 "[Get the User's Input](http://developer.apple.com/library/ios/documentation/iPhone/Conceptual/SecondiOSAppTutorial/CreatingAddView/CreatingAddView.html#//apple_ref/doc/uid/TP40011318-CH6-SW4)"

Relationship 是一种场景之间的连接类型。在这里，relationship和segue类似，图标如下所示：

<img src="http://developer.apple.com/library/ios/documentation/iPhone/Conceptual/SecondiOSAppTutorial/Art/relationship_icon_2x.png" width="27.5" height="27.5">

待续。