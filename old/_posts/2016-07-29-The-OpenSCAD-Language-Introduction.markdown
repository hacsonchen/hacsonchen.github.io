---
layout: post
title:  "OpenSCAD语言 简介"
date:   2016-07-29 14:32:39 +0800
categories: openscad
---
OpenSCAD是对2D/3D和实体模型编程的一门函数式编程语言，可在屏幕上创建模型，可导出多种2D/3D格式的文件渲染到3D网格中。

在OpenSCAD中，一段脚本通常用于创建2D/3D模型。这脚本是声明action自由格式的集合。

{% highlight csharp %}
 object();
 variable = value;
 operator()   action();
 operator() { action();    action(); }
 operator()   operator() { action(); action(); }
 operator() { operator()   action();
              operator() { action(); action(); } }
{% endhighlight %}

**对象**
#Objects are the building blocks for models, created by 2D and 3D primitives.

组建成模型的2D/3D基本元素的模块称之为对象。

[opscad-lang]: https://en.wikibooks.org/wiki/OpenSCAD_User_Manual/The_OpenSCAD_Language
