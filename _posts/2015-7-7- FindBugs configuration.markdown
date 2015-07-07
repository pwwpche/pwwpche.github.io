---
layout: post
title:  "配置FindBugs 0.5.2"
date:   2015-7-7
tags:
- FindBugs
- Lab
---

配了一天没反应后，终于把FindBugs 0.5.2的源码跑起来了。GUI似乎版本过旧，IDEA不认。弄了一下后跑起来命令行版的了。

找了他们在Google Code上的Repository，导入到Github里，然后一个个找他们commit的东西，直到找到0.5.2的commit，似乎是（81d5139885ae6a2f462eb16f5a842873371fafa9）

然后clone下来（还是用zip吧），IDEA新建一个Java项目，把源码扔进去。

下载Java JDK5，扔到IDEA的SDK里，Language Level也改成5(似乎是为了支持generic)

把BCEL的包导进去，把他们弄的包名Ctrl+Shift+R全都替换成findbugs，然后Deprecated那里选择bcel的Deprecated

把一堆没实现的method都实现好

然后没了。







