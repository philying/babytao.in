---
layout: post
title: "使用RBTools提交code review请求"
date: 2012-11-16 17:35
comments: true
categories: System
---

今天升级了下RBTools，安装RBTools需要git、python。

###安装配置

输入：

```
git clone git://github.com/reviewboard/rbtools.git
cd rbtools
sudo python setup.py install 
```

OK，post-review已经更新好了，post-review --version发现版本已经更新到0.4.2了

然后需要配置一下，在svn项目的根目录下，建立：.reviewboardrc文件，在文件中输入：

```
REVIEWBOARD_URL = "http://rb.yourcompany.com"
REPOSITORY_URL = "https://dev.yourcompany.com/svn/xxxx"
```
这2行分别代表你们公司的review board地址和svn仓库地址。发现0.4有个变化，原来是REPOSITORY=xxx，现在需要写成REPOSITORY_URL=xxx，否则会提示：

```
The --repository-url option requires either the --revision-range option or the --diff-filename option.
```
要求你指定仓库地址。

###使用

使用就很简单了，只需在项目目录下输入post-review即可。svn和git的使用略有区别，svn review的是uncommit的代码，git review的是本地仓库的最新更新，使用svn还是打击了我提交代码的积极性啊。

post-review还可以更新以前提交的diff,方法是用 -r 指定review number即可。具体的可以用post-review –help来查看详细信息。也可以去review board官网查看详细的说明：[RBTools使用说明](http://www.reviewboard.org/docs/codebase/dev/getting-started/#rbtools)