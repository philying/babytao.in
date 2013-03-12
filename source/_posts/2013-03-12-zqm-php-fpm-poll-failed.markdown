---
layout: post
title: "ZMQ PHP 扩展碰到的问题"
date: 2013-03-12 15:05
comments: true
categories: System
---

在使用[ØMQ](http://www.zeromq.org/)的PHP扩展的时候，发现一个奇怪的问题，线上一台php-fpm的机器始终会报这样一个错误：

{% codeblock %}
we11sh php-fpm: pool www: Poll failed: Interrupted system call
{% endcodeblock %}

查了很多资料没有结果，同样在另一台apache部署的机器上却没有这个问题。因此判断是php-fpm本身的问题，在php-fpm的debug模式下发现这样一个warning信息：

{% codeblock %}
[12-Mar-2013 14:55:20.112009] WARNING: pid 2830, fpm_request_check_timed_out(), line 271: 
pool www child 2907, script '/data1/www/release/2013Q1M3/index.php' (request: "GET /
index.php") executing too slow (1.226974 sec), logging
{% endcodeblock %}

这个是php-fpm记录slow request的一个配置，目前设置的是1S，这个难道会有影响么？修改了slow request的时间配置这个问题就修复了，但根本原因还有待研究，可能需要看一下ZMQ for php的extension的代码。