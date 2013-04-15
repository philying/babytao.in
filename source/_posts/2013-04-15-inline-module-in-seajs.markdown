---
layout: post
title: "inline module in seajs"
date: 2013-04-15 15:54
comments: true
categories:fontend 
---

最近在项目中使用seajs管理js的模块，由于框架实现了js的合并和打包，因此有时候需要写一些inline模块，查了一圈文档没发现怎么写法，后来在一个google group里面找到了答案：

{% codeblock %}
define('~/console', null, function(require, exports) {
    exports.log = function(msg) {
        if (window.console && console.log) {
            console.log(msg);
        }
        else {
            alert(msg);
        }
    };
});
{% endcodeblock %}

“~”表示inline的模块。

