# Windows配置Mingw编译
这一年打Code用的基本上都是Ubuntu的Vim，这次突然转到Windows，瞬间不想用Dev-cpp,于是想自己下个编译器，这里介绍如何安装并配置Mingw

首先装个Dev-cpp（可以到网上搜），虽然说Dev-cpp只是包含的Mingw，但是因为Mingw下载的速度感人，亲测装Dev-cpp比直接装Mingw快不知道多少倍

然后在文件管理器里，计算机->属性->高级系统设置->环境变量，然后再系统变量里找到Path，编辑，再最后加上(假如Dev-cpp装在C盘DE Program Files（x86)里:
```
C:\Program Files (x86)\Dev-Cpp\MinGW64\bin
```
然后保存Path，在系统变量里新建一个LIBRARY_PATH，内容为：
```
C:\Program Files (x86)\Dev-Cpp\MinGW64\lib
```
然后保存，再新建一个`C_INCLUDE_PATH`，内容为:
```
C:\Program Files (x86)\Dev-Cpp\MinGW64\include
```

配置完毕

然后再cmd中输入g++，发现可用