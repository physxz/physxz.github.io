---
title: Linux系统下用VSCode玩Arduino
tags:
  - Arduino
  - VSCode
categories:
  - MCU
index_img: /img/20220127-arduino-vscode.webp
typora-root-url: ..
abbrlink: 10019
date: 2022-01-27 01:07:00
---

今天把之前买的Arduino板子也移植到Linux系统下，虽然Arduino IDE有Linux版本，但是不够智能，用起来不爽。那么就要请出宇宙第一编辑器VSCode了。<!--more-->

# 软件及插件

下载[Linux 64bits](https://www.arduino.cc/en/software)版，解压到合适路径下，一会配置会用到。貌似不用安装。由于我配置VSCode之前就已经安装过Arduino了，所以暂时没有明确需不需要安装，但是就后续配置来看，没有用到安装路径，而是用的解压后的文件夹的路径。所以先不用安装，如果需要安装后再再安也不迟。

下载安装VSCode。`Ctrl+Shift+X`搜索安装C/C++插件（Arduino插件依赖该插件），再搜索安装`Arduino`，两者都是带有Microsoft标志的。`Ctrl+Shift+P`打开命令面板并输入`settings.json`，打开`Preferences: Open Settings (JSON)`，添加或替换如下代码，注意路径改为刚才解压的文件夹的路径（一定是解压文件的路径而不是安装路径）

```json
"arduino.path": "/home/zhao/Downloads/arduino-1.8.19/",
```

保存后在左下方应该就会有一个`ARDUINO EXAMPLES`文件夹，打开会发现里面就是Arduino自带的一些示例。如果没有重启以下VSCode，如果还没有就是路径配置有问题，请重新检查路径。

# 配置及Debug

然后用USB连接Arduino到电脑，就可以配置Arduino开发板了。在VSCode下方状态栏靠右有三个选项需要选择，我们从由往左选，首先是串口`Slect Serial Port`，一般默认是`/dev/ttyACM0`，然后是开发板`Show Board Config `，我这里选择Arduino Uno开发板，再然后是烧录器`Select Programmer`，可以选择`avrisp mkii`或者`arduino as isp`都可以。这是你会发现当前工作目录下多了一个文件夹`.vscode`，里面有一个文件`arduino.json`，打开发现就是刚才选择的那些配置信息。

新建`.ino`文件或打开已有文件，写一段测试代码

```c
#include <stdio.h>
```

首先，敲下两个字母就`Enter`的感觉太爽了，这要比在Arduino IDE下敲代码舒服得多。但是敲完`<stdio.h>`你就发现怎么没有自动补全，而且还出现了红色波浪线错误提示：`检测到 #include 错误。请更新 includePath`，这说明当前工作环境没有检测到这些`.h`头文件的存在，需要配置一些`C`的工作路径，`Ctrl+Shift+P`输入`Edit Configurations (JSON)`，添加以下第3行代码（参考了[这篇文章](https://blog.csdn.net/wh201906/article/details/103878857)）

```json
"includePath": [
    "${workspaceFolder}/**",
    "/home/zhao/Downloads/arduino-1.8.19/**"
],
```

路径也是刚才解压的文件夹，`**`表示搜索路径下所有子文件夹和文件。保存后就会发现红色波浪线消失了，又可以愉快的敲代码了。

```c
#include <stdio.h>

void setup() {
    Serial.begin(9600);
}
```

又出问题了！`Serial`红色波浪线提示`未定义标识符`，找到[这篇文章](https://blog.csdn.net/weixin_45488643/article/details/105966613)完美解决了这个问题。

`主要问题是头文件索引丢失，intellisense不能自动找到需要的头文件路径。需要在用户设置中强制*intellisense*使用*Tag Parser*,递归方式检索头文件。`

还是在第一次添加路径的`Preferences: Open Settings (JSON)`文件内添加如下中间2行代码

```json
"C_Cpp.updateChannel": "Insiders",
"C_Cpp.intelliSenseEngineFallback": "Disabled",
"C_Cpp.intelliSenseEngine": "Tag Parser",
"arduino.commandPath": "arduino"
```

至此，基本完成Linux下Arduino+VSCode的折腾。以后如遇到新问题再更新。