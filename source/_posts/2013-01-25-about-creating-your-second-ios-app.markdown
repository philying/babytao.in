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

<!--more-->

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

本例的app运行的时候，navtion controller会自动加载master scene并在屏幕上方显示导航条。

画布图形化的展示了每个场景的内容和关联关系，点击屏幕左下角的概览按钮可以打开概览面板查看详情。

###总结

在本章中，你使用Xcode创建了一个基于Master-Detail应用模板的iOS app。当你运行app时，你会发现它和其他的导航类iOS应用非常类似，比如Mail和Contacts。

当你在画布里面打开storyboard，你了解了storyboard是怎么回事，并了解了他们最后展现出来的样子。

#设计Model层

每个app都是采用类似的方式处理数据。在本教程中，BirdWatching应用处理一个bird-sighting事件列表。遵循Mode-View-Controller（MVC）设计模式，你需要创建class来展示和管理数据。

本章中，你要设计2个类。第一个是用于处理主要的单元数据。第二个创建并管理数据单元的实例。他们组成了app显示出来的列表内容，我们称之为BirdWatching app的Model层。

注意：Master-Detail模板定义了一个含有默认内容的数组。在默认的app中点击Add按钮（+）来添加数据。在以后的步骤中，你将在你接下来创建的类中替换系统临时的内容。

##确定数据单元并创建Data Object Class

设计一个data object class，首先需要根据app的功能来确定哪些数据需要处理。比如：你可能会考虑要定义一个bird class和一个bird sighting class。但为了确保本教程尽量简单，你只要定义一个bird-sighting类包括所有属性，birdname，sighting location和date。

创建BirdWatching应用可用的bird-sighting对象，你需要添加一个自定义class。bird-sighting类没什么复杂的功能（基本上就是一个简单的Objective-C对象），创建一个NSobject的子类即可。

###创建bird-signting对象

1. 在Xcode里，选择File > New > File (或者command-N)
2. 在弹出的对话框中，在左边的对话框中选择Cocoa Touch in the iOS选项
3. 选择Objective-C类并点击Next
4. 在下一个对话框面板中，输入BirdSighting作为类的名字，在弹出的“Subclass of”菜单中选择NSObject，点击Next。按照惯例，由于data object class作为数据展示用所以一般以名词命名。
5. 接下来选择BirdWatching目录：

<img src="http://developer.apple.com/library/ios/documentation/iPhone/Conceptual/SecondiOSAppTutorial/Art/create_data_obj_files_2x.png" width="390" height="359">

6. 点击Create

Xcode会创建2个class，分别是BirdSighting.h和BirdSighting.m。默认情况下，Xcode在编辑区域中自动打开实现文件（BirdSighting.m）。

BirdSighting类需要一种方式来保持3部分定义bird sighting的信息。

首先，在头文件声明属性和自定义初始化方法。

1. 选择BirdSighting.h
2. 在@interface和@end中添加以下代码：

``` c++ BirdSighting.h
@property (nonatomic, copy) NSString *name;
@property (nonatomic, copy) NSString *location;
@property (nonatomic, strong) NSDate *date;
```
3. 在这3个属性定义后面添加以下代码：

``` c++ BirdSighting.h
-(id)initWithName:(NSString *)name location:(NSString *)location date:(NSDate *)date;
```

关于变量声明的一些学习记录
* 短线“-”表示这个是Objective-c方法的声明
* 短线后面是方法的返回类型
* 返回类型后面是函数名字
* 无参数的函数后面不需要加括号和冒号，直接是函数名结束加分号，比如：-(void)displayDateinfo;
* 有参数的后面加冒号和参数类型名字，比如：-(id)initWithName:(NSString *)name;//单个参数 -(void)setData:(int)param1 :(int)param2;//多个参数

Objective-c中缀符的语法，方法的名称和参数都是在和在一起的：比如initWithName


实现自定义初始化方法

1. 选择 BirdSighting.m
   
当BirdSighting.m在编辑器中打开时，Xcode会显示一个警告图标-在编辑器顶部的提示区域。在导航选择器点击警告图标可以看到警告信息。你会看到这样的显示：

<img src="http://developer.apple.com/library/ios/documentation/iPhone/Conceptual/SecondiOSAppTutorial/Art/warning_icon_expanded_2x.png" width="260" height="104.5">

在这里，Xcode提示你BirdSight类还没有实现头文件中定义的 initWithName方法。接下来你需要修复这个问题。

2. 在@implementation和@end之间，输入initWithName方法，可以在弹出的代码提示中选择自动完成输入。
3. 以下代码实现 initWithName方法

``` c++ BirdSighting.m
{
    self = [super init];
    if (self) {
       _name = name;
       _location = location;
       _date = date;
       return self;
    }
    return nil;
}
```

当你完成所有的代码以后，警告信息也会随之消失

注意，initWithName方法定义属性的时候使用了下划线开头（_name,_location,_date）。按照惯例，下划线打头的方法表明这些属性不应该被直接访问。在几乎你所有的代码中，你应该使用存取方法来访问对象的属性，比如self.name。只有2个地方你不应该使用存取方法，分别是initWithName和dealloc方法。

理论上说，避免直接访问成员变量有利于更好的封装，实践中还有2点好处：

* 一些Cocoa技术（尤其是key-value代码）依赖使用存取方法并切对存取方法有规定的命名。如果你不使用存取方法，你的app可能会错过一些Cocoa的特性。
* 一些属性的值是在需要的时候才有的，如果你直接访问这些变量可能会返回nil或未初始化的值。（View controller就是一个例子）

（如果你对此感兴趣，你可以阅读相关章节 “[Strong and Accessing Properties](http://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CocoaFundamentals/AddingBehaviortoaCocoaProgram/AddingBehaviorCocoa.html#//apple_ref/doc/uid/TP40002974-CH5-SW5)”）

BirdSighting class是BirdWatching app的model层的一部分。你需要设计一个controller class来实例化BirdSighting对象。数据控制对象允许app的其他对象能够访问BirdSighting对象和master list，不需要关心数据是哪来的。接下来你需要来创建这个数据控制类。

##创建Data Controller Class

Data controller class主要负责app数据的加载，保存和访问。虽然本例不访问文件也不保存数据，但仍然需要data controller创建BirdSighting对象并管理他们的集合。需要以下功能：

* 创建一个保持所有BirdSighting对象的集合
* 返回集合中BirdSighting对象的数量
* 从集合中指定的位置返回BirdSighting对象
* 通过用户输入来创建一个新的BirdSighting对象并添加到集合中

创建data controller文件：

1. 选择File > New > File (或者使用 Command-N)
2. 在弹出的对话框中，在左侧的iOS选择区域Cocoa Touch
3. 在对话框的主窗口中，选择Objective-c class，并点击Next
4. 在下一个显示面板中，输入类的名称为 BirdSightingDataController，subclass框选择NSObject，并点击Next
5. 在接下来的对话框中选择BirdWatching所在的目录并创建文件

在本教程中，BirdSighting对象的集合用数组来表示。数组是一个按顺序排列对象的集合对象，并能够访问制定位置的对象。BirdWatching app允许用户在主列表里添加新的bird sighting。所以你需要一个可变长的数组对象。

定义data controller的属性：

1. 在编辑器中打开BirdSightingDataController.h
2. 在@interface 和 @end之间添加以下代码

``` c++ BirdSightingDataController.h
@property (nonatomic, copy) NSMutableArray *masterBirdSightingList;
```

注意“copy”属性：指定应该使用对象的副本。稍后，在实现文件中，你将创建自定义的setter方法确保数组的副本也是不定长的。

先别关闭头文件，还有方法声明文件没加。在本节开始的时候提到过，data controller有4个功能。其中三项是提供方法给别的对象来获取BirdSighting对象的信息或者添加新的对象到列表，但“创建master集合”是data controller对象需要关心的。因为这个方法不需要暴露给别的对象，所以你不需要在header文件里声明这个文件。

添加三个访问数据的方法：

1. 在BirdSightingDataController.h文件中添加以下方法声明代码：

``` c++ BirdSightingDataController.h
- (NSUInteger)countOfList;
- (BirdSighting *)objectInListAtIndex:(NSUInteger)theIndex;
- (void)addBirdSightingWithSighting:(BirdSighting *)sighting;
```

注意到objectInListAtIndex左边显示了一个红色的警告图标。Xcode提示“Expect a type”。这表示Xcode无法识别BirdSighting这个返回类型。解决这个问题，你需要添加一个“forward declation（前置声明）”，他向编译器声明项目中包含这个类的定义。

2. 添加一个BirdSighting类的前置声明
在#import 和 @interface之间添加以下代码

``` c++ BirdSightingDataController.h
@class BirdSighting;
```
刚才那个报错信息就消失了。

头文件定义完了，接下来可以开始实现这些方法了，打开BirdSightingDataController.m你会发现Xcode会有一些错误提示信息，实现完头文件中定义的方法后，这些信息自然会消失。

之前提到过，data controller需要创建一个master list并且显示一个默认项。创建一个init方法是实现它的一个好办法。

实现list-creation方法

1. 导入BirdSighting头文件，这样data controller就能够引用它定义的那些方法，@interface 声明如下：

``` c++ BirdSightingDataController.h
#import "BirdSighting.h"
```

2. 在@implementation声明前添加@interface声明来定义list-creation方法

``` c++ BirdSightingDataController.h
- (void)initializeDefaultDataList;
``` 

这个声明用于定义一个类的私有方法。

3. 实现这个方法

``` c++ BirdSightingDataController.h
- (void)initializeDefaultDataList {
    NSMutableArray *sightingList = [[NSMutableArray alloc] init];
    self.masterBirdSightingList = sightingList;
    BirdSighting *sighting;
    NSDate *today = [NSDate date];
    sighting = [[BirdSighting alloc] initWithName:@"Pigeon" location:@"Everywhere" date:today];
    [self addBirdSightingWithSighting:sighting];
}
``` 

initializeDefaultDataList方法做了下面这些事情：首先，它给sightingList赋了一个新的可变长数组。然后，它用一些默认的值来创建了一个BirdSighting对象并传递给之前定义的addBirdSightingWithSighting方法，这样就在主列表中添加了一项。

虽然Xcode自动同步了accessor方法，你需要重写它的setter方法来确保新的数组是可变的。默认情况下，方法命名为setPropertyName（属性名字的首个字母大写）。你需要按照这种命名规范来命名accessor方法，否则accesor方法不会被调用。

实现一个自定义的setter属性

* 在BirdSightingDataController里添加一下代码

``` c++ BirdSightingDataController.m
- (void)setMasterBirdSightingList:(NSMutableArray *)newList {
    if (_masterBirdSightingList != newList) {
        _masterBirdSightingList = [newList mutableCopy];
    }
}
``` 

默认情况下，创建一个Objective-c类的时候Xcode不需要实现一个默认的init方法。因为大多数对象只需要执行[super init]。

初始化data controller对象

* 在@implementation区域输入以下代码

``` c++ BirdSightingDataController.m
- (id)init {
    if (self = [super init]) {
        [self initializeDefaultDataList];
        return self;
    }
   return nil;
}
``` 

这个方法给self赋予父类初始化返回的值。如果[super init]执行成功，这个方法会调之前定义的initializeDefaultDataList方法来初始化.

现在data controller已经可以创建主列表，显示默认项并初始化自身实例。现在我们来实现头文件中定义的三个方法让其他对象能够和主列表交互。

* countOfList
* objectInListAtIndex:
* addbirdSightingWithSighting:sight:

实现data controller的数据访问方法：

1. 实现countOfList方法

``` c++ BirdSightingDataController.m
- (NSUInteger)countOfList {
    return [self.masterBirdSightingList count];
}
```

count方法是NSArray的一个方法，返回数组的总数。因为masterBirdSightingList的类型是NSMutableArray，继承自NSArray，所以这个属性可以被调用到。

2. 实现objectInListAtIndex:方法

``` c++ BirdSightingDataController.m
- (BirdSighting *)objectInListAtIndex:(NSUInteger)theIndex {
    return [self.masterBirdSightingList objectAtIndex:theIndex];
}
```

3. 实现addBirdSightingWithSighting:sighting：方法

``` c++ BirdSightingDataController.m
- (void)addBirdSightingWithSighting:(BirdSighting *)sighting {
    [self.masterBirdSightingList addObject:sighting];
}
```

这个方法通过传递用户输入的name和location信息给initWithName:location:date来创建并初始化了一个新的BirdSighting对象，使用当前时间。然后，这个方法添加新的BirdSighting对象到数组。

代码写完后警告框就消失了。

你现在也可以运行项目，但看上去和一开始没什么两样，因为view controller并不知道你实现的data model。在下一章中，你将编辑master view controller，这样你就可以开始来操作数据了。

#设计主场景

当你运行BirdWathcing app，第一个眼看到的就是bird sighting的主列表。本章中，你会设计master list的展现并实现master list对数据模型层的实现。

##设计Master View Controller Scene

在之前的教程中你已经些了很多代码了，现在是时候需要你在画布上做一些设计工作了。选择MainStoryboard.storyboard打开storyboard。

iOS应用经常使用表哥视图来显示列表项，因为表格提供了一个有效的可自定义的方法来展现或大或小的数据。比如，Mail，Settings，Contacts。。在本章中，你会定义主场景的整体外观并设计表格行的样式。

定义主场景的展示：

1. 调整缩放级别（如果有必要的话）
2. 双击导航条上的标题（现在是Master）并输入“Bird Sightings”
3. 点击选择table view，你也可以在Birds Master View Controller面板选择
4. 点击打开功能面板
5. 在功能面板中点击属性按钮打开属性区域
6. xxx

2个常见的方法设计表格行（有时候叫单元行）：

* 动态属性允许你设计一个单元并用它作为其他单元的模板。当你希望所有的单元外观统一就使用动态外观，而且你不能预知多少个单元会被展示
* 静态单元允许你综合设计你的表格外观，包括单元的总数。外观维持不变，而且知道会展示什么信息的情况下使用静态单元

为bird-sighting list设计一个属性单元

1. 在属性设置的Table View部分，在Content菜单中选中Dynamic Prototypes
…

#增加新的ITEM

到现在为止，BirdWatching实现了主场景的item列表并能够在新的场景显示每个item的详细内容。你已经学习并了解通过串联图来设计并实现这一切是多么的简单。

在本章中，你将创建一个能够让用户自定义输入bird sighting信息的场景，你将了解：

* 添加一个新的导航控制器
* 展示一个场景
* 使用auto layout来定义UI元素之间的可视化关系
* 允许场景之间传递信息
* 提高应用的可用性

##创建新场景文件

在这步，你为view controller创建用于管理添加内容场景的接口文件和实现文件。