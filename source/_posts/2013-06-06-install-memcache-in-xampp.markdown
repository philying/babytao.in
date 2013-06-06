---
layout: post
title: "在Mac OS XAMPP 编译 memcache"
date: 2013-06-06 16:50
comments: true
categories: System
---

XAMPP是一个比较方便的lamp集成开发环境，默认没有安装memcache扩展，这里记录一下在安装过程中碰到的一些问题。

* 编译扩展需要安装Developer Package，否则编译会出错
* 默认使用pecl install memcache编译后报错

```
PHP Warning:  PHP Startup: Unable to load dynamic library '/Applications/XAMPP/xamppfiles/lib/php/php-5.3.1/extensions/no-debug-non-zts-20090626/memcache.so' - dlopen(/Applications/XAMPP/xamppfiles/lib/php/php-5.3.1/extensions/no-debug-non-zts-20090626/memcache.so, 9): no suitable image found.  Did find:\n\t/Applications/XAMPP/xamppfiles/lib/php/php-5.3.1/extensions/no-debug-non-zts-20090626/memcache.so: mach-o, but wrong architecture in Unknown on line 0
```
应该是默认编译成32位的关系，需要在编译前指定一些参数

编译步骤：

* pecl download memcache
* 解压后进入memcache 目录
* sudo /Applications/XAMPP/xamppfiles/bin/phpize
* sudo MACOSX_DEPLOYMENT_TARGET=10.7 CFLAGS="-arch i386 -arch x86_64 -g -Os -pipe -no-cpp-precomp" CCFLAGS="-arch i386 -arch x86_64 -g -Os -pipe" CXXFLAGS="-arch i386 -arch x86_64 -g -Os -pipe" LDFLAGS="-arch i386 -arch x86_64 -bind_at_load" ./configure --with-apxs=/Applications/XAMPP/xamppfiles/bin/apxs --with-php-config=/Applications/XAMPP/xamppfiles/bin/php-config
* sudo make & make install
* php.ini添加extension='memcache.so'