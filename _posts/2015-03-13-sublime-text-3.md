---
layout: post
title:  "Sublime Text 3"
date:   2015-03-13 22:05:40
categories: tools
tags: tools sublime-text-3
---

Install Package Control
-------
使用`` Ctrl+\` ``快捷键或者通过`View->Show Console`菜单打开命令行，粘贴如下代码

    import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())