---
title: 轻量级编辑器SublimeText&VisualStudioCode
tags:
  - python
  - VSCode
categories:
  - 技术
index_img: /img/20211228-sublime-text.png
typora-root-url: ..
abbrlink: 10013
date: 2021-12-28 21:54:00
---

PyCharm确实强大好用，但是对于新手和小项目太过笨重。而Sublime Text这一轻量级编辑器满足了这一需求。**<然而最后我还是转战VSCode了，Sublime的代码补全功能跟稍重一点的轻量级编辑器VSCode根本没法比！>**<!--more-->

# 下载安装

我是在ubuntu下使用的。下载安装参考[官方文档](https://www.sublimetext.com/docs/linux_repositories.html)。

安装完成后需要配置一番，就可以成为我们的生产力工具了。

# 配置

对于sublime的配置，我看了很多人写的教程博客，但基本都是一样的错误或没必要，上Youtube搜了以下，果然，配置起来不是很复杂。Sublime打开或新建一个python文件，一是易于观察后面的变化，二是你必须通过后缀名告诉sublime这是python文件，他才能加载解释器和高亮代码等。主要参考[这个视频](https://www.youtube.com/watch?v=xFciV6Ew5r4)和[这个视频](https://www.youtube.com/watch?v=oHmPrjSzmwU&t=456s)。

## Python3解释器

在终端输入命令（或者直接用sublime新建文件，`Ctrl+B`运行）查看python3解释器路径

```
python3
import sys
print(sys.executable)
print(sys.version)
```

我的路径是`/usr/bin/python3`，打开工具栏`Tools->Build System->New Build System...`，用下面代码替换原内容，注意其中路径改为刚才所得路径，重命名为Python3，后缀不要改动，保存重启即可用Python3解释器编译。

```
{
    "cmd": ["/usr/bin/python3", "-u", "$file"],
    "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
    "quiet": true
}
```

## Python代码自动补全

Sublime text的强大好用很大程度得益于它的众多扩展插件。为了使用这些插件，首先要安装插件管理工具，按下`Ctrl+Shift+P`打开命令面板，输入`Package Control`，回车或点击，可以看到下方有进度条等待安装。安装完成后再次打开命令面板，输入`Install Package`回车，输入`jedi python auto completion`回车等待安装，重启sublime后敲几行代码试试看，这是应该可以`Tab`或回车补全代码了。

![](/ass/20211228-sublime.png)

# 其他插件

侧边栏右键菜单栏增强：`BracketHighlighter`

括号匹配高亮：`Side Bar Enhancements`

主题1：`Material Theme`

主题2：`Predawn`

# 特色及快捷键

参考[这个视频](https://www.youtube.com/watch?v=TB3qztdM7V8&t=1s)

如何使用使效能最大化？

Linux系统：新建项目文件夹->右键打开终端->`subl .`即可在sublime中打开当前文件夹，然后就开始旅程吧！

另：刚发现typora也可以这么玩！

# 最后

转战Visual Studio Code了，只下载一个`python`插件就比Sublime配置半天要好用。当然也可能是我不会配置，但这也正说明了Sublime不适合我这种小白。

## VSCode的简单说明

### 插件

Python：`Python`

路径自动补全：`Path Autocomplete`

### 快捷键

终端打开命令：`code .`

运行：`Ctrl+F5`

命令面板：`Ctrl+Shift+P`

### 其他

如果环境中配置了ipython，也可以在VSCode中使用Jupyter Notebook。

`Ctrl+Shift+P`打开命令面板，输入`jupyter`，单击`Jupyter: Create New Jupyter Notebook`，即可新建`.ipynb`文件，享受jupyter的分块代码的乐趣了。[参考](https://matters.news/@youdaily/vs-code%E4%B8%8A%E4%B9%9F%E8%83%BD%E7%8E%A9%E8%BD%ACjupyter-notebook-%E8%BF%99%E6%98%AF%E4%B8%80%E4%BB%BD%E5%AE%8C%E6%95%B4%E6%95%99%E7%A8%8B-bafyreic6ok64s5t773c722v2dsumg2c67bbhro6terda44umcubc6xwi2m)

快捷键：`Shift+Enter`运行当前Cell，新建Cell并跳转

`Win+Enter`运行当前Cell