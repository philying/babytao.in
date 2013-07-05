---
layout: post
title: "前端单元测试实践"
date: 2013-07-05 10:52
comments: true
categories: fontend
---

#纠结篇
{:toc}

随着页面逻辑复杂度越来越高，前端的javascript代码变得越来越庞大，如何维护成了一个问题。添加新功能如何才能放心的不出问题，单元测试无疑是一个不错的手段。But。。在做单元测试之前，不得不面临这么几个纠结的问题

* 小项目功能不复杂，没必要做单元测试浪费时间
* 老代码写的时候没考虑到单元测试，这些代码基本不可测，需要全部重构
* 原有的代码已经运行了很长时间了，而且经过QA的功能测试没问题，何必冒险去重构代码并补上单元测试呢
* 每个Milestone就这么点时间，项目多的情况下，没空做这事，先保证项目按期上线吧

而单元测试的好处是

* 有了单元测试，重构代码方便，重构代码出现的问题会及时被发现
* 单元测试是一个约定代码规范的好机会，因为你不按一定套路写，代码无法测试
* 和CI集成，定期得到项目的测试情况数据（老板愿意看）


做有用的事情，而不是为了做而做，这个是我的原则之一。因此我们针对单元测试的定位是前端开发用于组件、类库的测试，QA会负责前端的自动化回归测试，双方互补来覆盖核心功能点。

#工具篇

单测框架选择 [Jasmine](http://github.com/pivotal/jasmine)，一个“BBD for JavaScript” framework。

![jasmine](http://pivotal.github.io/jasmine/images/jasmine_logo.png)

1. jasmine的基础语法

jasmine单元测试有2个核心部分：describe函数和it函数。

``` javascript
describe('JavaScript addition operator', function () { 
    //建立it块
    it('adds two numbers together', function () { 
        //测试1+2是否等于3
        expect(1 + 2).toEqual(3); 
    }); 
});
```

describe和it函数都有二个参数：

* 第一个参数：测试描述
* 第二个参数：测试逻辑函数

2. jasmine使用

在项目中，目录结构这样规划

![目录结构](http://farm6.staticflickr.com/5505/9214900398_eba92bed77_o.png)

首先在测试页面中引入jasmine库

``` javascript
<link rel="stylesheet" type="text/css" href="http://pages.juxiao.mediav.com/jasmine/jasmine.css">
<script type="text/javascript" src="http://pages.juxiao.mediav.com/jasmine/jasmine.js"></script>
<script type="text/javascript" src="http://pages.juxiao.mediav.com/jasmine/jasmine-html.js"></script>
```

我们希望最后测试的结果能够和Jekins集成，所以输出的结果希望是Junit testcase格式，所以用了一个jasmine的junit format插件

``` javascript
<script type="text/javascript" src="http://pages.juxiao.mediav.com/jasmine/report/jasmine.junit_reporter.js"></script>
```

在测试页面中引入需要测试的代码文件

``` javascript
<script type="text/javascript" src="../common/libs/grid_galile.js"></script>
```

在测试页面中引入单元测试代码

``` javascript
<script type="text/javascript" src="common/galileoGridSpec.js"></script>
```

初始化jasmine，和默认的通用代码不同，使用的reporter是JUnitXmlReporter

``` javascript
(function() {
	var jasmineEnv = jasmine.getEnv();
	jasmineEnv.updateInterval = 1000;

	//var htmlReporter = new jasmine.HtmlReporter();
	var junitReporter = new jasmine.JUnitXmlReporter();

	jasmineEnv.addReporter(junitReporter);

	/*jasmineEnv.specFilter = function(spec) {
		return htmlReporter.specFilter(spec);
	};*/

	var currentWindowOnload = window.onload;

	window.onload = function() {
	    if (currentWindowOnload) {
	      currentWindowOnload();
	    }
		execJasmine();
	};

	function execJasmine() {
		jasmineEnv.execute();
	}
})();
```
接下来就可以在galileoGridSpec.js中写单元测试代码了。由于之前的代码并没有考虑到单元测试，因此目前来说需要有很多的重构工作，这部分的总结会在以后记录。

3. 集成到持续集成环境

使用[phantomjs](http://github.com/pivotal/jasmine)来跑测试代码，PhantomJS is a headless WebKit scriptable with a JavaScript API. It has fast and native support for various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG. 

![phantomjs](http://phantomjs.org/images/phantomjs-logo.png)

runner.sh脚本

``` sh
#!/bin/bash

# sanity check to make sure phantomjs exists in the PATH
hash /usr/bin/env phantomjs &> /dev/null
if [ $? -eq 1 ]; then
    echo "ERROR: phantomjs is not installed"
    echo "Please visit http://www.phantomjs.org/"
    exit 1
fi

# sanity check number of args
if [ $# -lt 1 ]
then
    echo "Usage: `basename $0` path_to_runner.html"
    echo
    exit 1
fi

#SCRIPTDIR=$(dirname `perl -e 'use Cwd "abs_path";print abs_path(shift)' $0`)
TESTFILE=""
while (( "$#" )); do
    if [ ${1:0:7} == "http://" -o ${1:0:8} == "https://" ]; then
        TESTFILE="$TESTFILE $1"
    else
        TESTFILE="$TESTFILE `perl -e 'use Cwd "abs_path";print abs_path(shift)' $1`"
    fi
    shift
done

/usr/bin/env phantomjs phantomjs-testrunner.js $TESTFILE
```

jasmine reporter的代码可以在这里找到。[jasmine-reporter](https://github.com/larrymyers/jasmine-reporters)

这样在shell执行命令

``` sh
. runner.sh http://g.local.dev.mediav.com/js/test/test.galileoGrid.html
```
就可以生成一个TEST-galileoGrid.xml的测试结果文件，这是一个标准的junit格式的testcase，可以直接集成到CI。

#总结

跑通整个单元测试的流程并不复杂，现在的工具都很强大，使用起来都非常的方便，真正的挑战是接下来如何让单元测试发挥作用，而不是仅仅流于形式。