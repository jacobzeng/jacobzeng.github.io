---
layout: post
title:  "IntelliJ IDEA"
date:   2014-11-10 22:05:40
categories: tools
tags: tools sublime-text-3
---

OSX Questions
------

Q: 您需要安装旧 Java SE 6 运行环境才能打开
======
1. 用文本编辑器打开/Applications/IntelliJ IDEA 13.app/Contents/Info.plist
2. 搜索JVMVersion，将其值改为1.7*
3. 再次运行应用即可看到应用成功运行

Q: Dmaven.multiModuleProjectDirectory system propery is not set
======
In Maven–>Runner options set a following VM option: -Dmaven.multiModuleProjectDirectory=project_path_which_to_run