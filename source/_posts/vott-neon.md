---
title: 图像视频标注工具
date: 2019-11-06 20:23:11
tags: 标注
categories: 深度学习
---
网上开源的标注工具很多，但总是缺少很多功能。在生成中造成极大的困惑，本贴提供可标注矩形、点、线、多边形等的标注工具，具有缩放平移等功能。

<!-- more -->
[vott](https://github.com/Microsoft/VoTT)是微软旗下的标注工具，众所周知，微软大刀总会砍向好的项目，在步入2.0版本时候，开了倒车，没有增加图像缩放功能，还把多边形标注功能砍了。

我在1.7.2版本上添加了图像缩放功能，增加了多组标签功能，增加标签编辑和搜索功能。

1.7.2.2版本更新日志

```
添加标签编辑窗口，按E键编辑，按tab键切换标签
按alt键可以使选中框外的其他框锁定
修正图像缩放功能，能够标注一些小目标
添加画人车3D功能，可调整大小
修正鼠标停在目标区域导致下一张不能创建标注的问题
点下一张图会自动保存，当前图s或者ctl+s保存
多段线功能将原矩形拖动改为线条拖动，多边形将原矩形拖动改为多边形内部
将原有的着色改为：1.默认为线条加0.5透明的边沿；2.鼠标悬停为0.8透明矩形；3.线条选中仅显示多段线
多标签问题：由于Pascal导出只能使用一个类别，为了更加直观，以标注颜色为统一，选对应颜色的那个类别，为靠前的类别
还存在的bug: Ctrl点击锚点会删除该点，要移动任意一点才能保存
```
![](/imgs/2019-11-07-vott-test.png)

在线演示[github](https://neophack.github.io/vott-ct/test/)

下载链接[Windows版本](https://www.lanzous.com/i77ui8f)
