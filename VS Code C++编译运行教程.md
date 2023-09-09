---
title: VS Code C++编译运行教程
date: 2022-06-07 22:57:31
tags: 学习
---

# VS Code C++编译运行教程

基于https://zhuanlan.zhihu.com/p/77074009教程，加上我的做法

## Mingw安装

https://sourceforge.net/projects/mingw-w64/files/ 在这里下载Mingw的最新版本，记得下载下面的MinGW-W64 GCC

而后将Mingw放入一个位置，然后配置环境变量即可，此处不赘述

## VSCode

### VSCode 插件安装

![image-20230607223425182](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230607223425182.png)

### 配置文件处理

这个是最重要的，有三个配置文件需要进行配置

![image-20230607223702763](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230607223702763.png)

#### c_cpp_properties.json

该文件可由VScode自动生成，生成方式为

1. 同时按**Ctrl** +**Shift**+**P**，打开命令面板，输入C/C++，选择**编辑配置 (UI)**或者**Edit Configurations (UI)**。
2. 其中**编辑器路径**根据需要修改为mingw的gcc.exe或g++.exe，**IntelliSense模式**修改为windows-gcc-x64，
3. 关闭窗口，**c_cpp_properties.json**文件会自动生成

#### tasks.json

这个文件博大精深，首先我们要谈一谈编译和运行这两个概念，Vscode中编译和运行实际上是能分开的

编译最简单的方法莫过于命令行，`g++ test.cpp -o test.exe `这是一个非常简单的编译命令，然而，我们也可以使用**终端**中的任务来进行将编译这一过程自动化。

在.vscode文件夹不存在tasks.json文件时，选择运行，然后调试，其会出现![image-20230607231054335](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230607231054335.png)

选择生成和调试活动文件即可，而实际上这个选项本来就在终端的 配置任务 之中（如下图）。![image-20230607231446417](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230607231446417.png)

打开后，会自动生成tasks.json文件

![image-20230607231528748](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230607231528748.png)

上述tasks.json完成的就是编译程序的任务，其中type这个选项是可以去掉的，然后label可以对task进行修改，一个tasks文件可以有多个task，在tasks的打括号后面加上逗号，再加上大括号即可。

当对多个文件进行编译时，可以在args中插入该文件的位置，例如`${fileDirname}\\cJSON.c`，非常灵活。

#### launch.json

该文件在运行和调试中自动生成

![image-20230607232028469](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230607232028469.png)

创建后可在右下角添加配置，选择启动即可，Bash启动没看懂是啥，暂时不用。

需要修改的大概是两处地方，program和miDebuggerPath，program修改为`${workspaceFolder}/${fileBasenameNoExtension}.exe`，需要注意program可灵活修改，miDebuggerPath修改为对应的mingw文件夹下的bin文件夹中的gdb.exe文件。

![image-20230607232425865](http://yyh-blogimage.oss-cn-shanghai.aliyuncs.com/img/image-20230607232425865.png)

## 编译与运行

配置完成后，即可进行编译与运行，点击终端，其中的运行任务中就会有编译的task，然后运行点击运行中的非调试运行即可
