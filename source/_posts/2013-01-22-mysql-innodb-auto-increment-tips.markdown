---
layout: post
title: "Mysql5.1.59自增主键问题"
date: 2013-01-22 15:39
comments: true
categories: System
---

MySQL的InnoDB引擎在对自增主键的处理上和MySIAM不同，按照官方的说法：

InnoDB uses the in-memory auto-increment counter as long as the server runs. When the server is stopped and restarted, InnoDB reinitializes the counter for each table for the first INSERT to the table, as described earlier.

A server restart also cancels the effect of the AUTO_INCREMENT = N table option in CREATE TABLE and ALTER TABLE statements, which you can use with InnoDB tables to set the initial counter value or alter the current counter value.

[官方解释](http://dev.mysql.com/doc/refman/5.1/en/innodb-auto-increment-handling.html)

就是说InnoDB重启后，会根据表当前Max(id)来觉得自增主键的offset，如果这时候表的记录为空，那即使之前即使写过数据，自增主键也会被重置成1。这个不是bug，MySQL的机制如此。

主键选择问题上，目前应用比较简单，所以还是无脑的选择了自增主键，接下来会推动一下这块的改造。

翻出了之前看过的文章，记录一下。

[分库设计中的主键选择](http://www.cnblogs.com/ylqmf/archive/2011/10/11.html)
