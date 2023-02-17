---
layout: post
title: "Windows批处理(bat)语法"
subtitle: "一些bat语法"
date: 2023-02-17
author: "Alien"
header-img: "img/post-bg-2015.jpg"
tags: [技术 windows]
---
### 1 常用命令

| 命令名 | 作用 | 备注 |
| ------ | ---- | ---- |
| cd  /d %~dp0 | 进入到bat脚本目录 | /d是为了切换盘符 |
| 命令 /? | 打开帮助 |      |
| taskkill /f /im notepad.exe | 终止进程 | /f是强制，/im是匹配名字 |
| %cd% | 当前文件夹 | |
| %0 - 9 | 函数传参 | |
|  |  | |

### 2 常见问题

| 问题        | 解决方案               | 备注 |
| ----------- | ---------------------- | ---- |
| cmd中文乱码 | 右键cmd窗口，属性，GBK |      |

### 3 基础语法

1.批处理文件是一个“.bat”结尾的文本文件，这个文件的每一行都是一条DOS命令。可以使用任何文本文件编辑工具创建和修改。

2.批处理是一种简单的程序，可以用 if 和 goto 来控制流程，也可以使用 for 循环。

3.批处理的编程能力远不如C语言等编程语言，也十分不规范。

4.每个编写好的批处理文件都相当于一个DOS的外部命令，把它所在的目录放到DOS搜索路径(path)中，即可在任意位置运行。

6.大小写不敏感(命令符忽略大小写)

7.批处理的文件扩展名为 .bat 或 .cmd。

8.在命令提示下键入批处理文件的名称，或者双击该批处理文件，系统就会调用Cmd.exe来运行该文件。

###  4 变量

常用系统变量

```bat
%CD%  获取当前目录
%PATH%  获取命令搜索路径
%DATE%  获取当前日期。
%TIME%  获取当前时间。
%RANDOM% 获取 0 和 32767 之间的任意十进制数字。
%ERRORLEVEL% 获取上一命令执行结果码
```

变量读取：

两个百分号之间加变量%varable%

变量设置：

```bat
D:\>set VAR1="I Love BAT Script"
D:\>echo %VAR1%
"I Love BAT Script"
```

### 字符串

##### 字符串截取。

使用命令 **echo %var:~n,k%，**其中"%var"，表示待截取字符的字符串，"~"取字符标志符，"n”表示字符截取起始位置，"k" 表示截取结束位置（不包含该字符）。举例如下 

```bat
set str=superhero //注意等号左右不能有空格
echo str=%str% 
echo str:~0,5=%str:~0,5%
echo str:~3=%str:~3%
echo str:~-3=%str:~-3% 
echo str:~0,-3=%str:~0,-3% 
//result
str=superhero
str:~0,5=super
str:~3=erhero
str:~-3=ero
str:~0,-3=superh
```

##### 字符串替换

使用命令**%var:old_str=new_str%** ，举例如下

```bat
set str=hello world!
set temp=%str:hello=good% 
echo %temp% 
```

字符串拼接


### 文件操作

```bat
//文件复制到目录下
copy d:\temp1\file1.txt d:\temp2 将文件file1.txt复制到temp2目录，有相同文件提示
copy d:\temp1\file1.txt d:\temp2 /y 将文件file1.txt复制到temp2目录,有相同文件覆盖原文件，不提示
copy d:\temp1\* d:\temp2 /y 将temp1目录下的所有文件复制到temp2目录,有相同文件覆盖原文件，不提示

//目录复制
xcopy temp1 d:\temp2 /s /e /y 将temp1目录下的文件复制到temp2目录，包括temp1子目录下的文件

// 重命名
ren d:\temp1\file1.txt file2.txt 修改temp目录下的file1.txt文件名为file2.txt

//删除
del d:\temp1\file1.txt 删除temp目录下的file1.txt文件
del d:\temp\*.txt  删除temp目录下的后缀为.txt的文件

rmdir d:\temp1 /s /q 删除temp1目录，包括子目录(/s)，并且删除时不提示(/q)
```

### 逻辑语句

##### for 

`for  %%I in (ABC) do echo %%I`

只能执行一条语句

##### if

```bat
if defined str (echo %str%) else (echo 变量str的值为空)
if "字符串1"=="字符串2" command 语句
::判断两个字符串是否相等
if 数值1 equ 数值2 command 语句
::equ neq lss gtr leq geq 
::判断两个数值是否相等
if exist filename command 语句
::判断判断驱动器，文件或文件夹是否存在
if defined 变量 command 语句
::判断变量是否已经定义
if errorlevel 数值 command 语句
::判断上个命令的返回值
```

### 函数

函数以冒号开始，并以goto:eof结束，

```bat
:myDosFunc    - 函数的开始，用一个标签标识  
echo. 函数体，可以执行很多命令  
echo.   
echo.
goto:eof
```

### 注释

1. ::注释内容--按行注释
2. REM 注释时，sh不执行后面的语句，但是会显示。
3. %注释内容%--按行注释 ::注意引用bat变量也是%%，容易混淆。
4. ：注释内容--注意注释文本不能与已有标签重名，因为：也可定义标签，建议不用，避免混淆。
5. goto +标签： 注释一段内容

### 参考文献

基础：https://www.cnblogs.com/linyfeng/p/8072002.html

参数：https://blog.csdn.net/albertsh/article/details/52788106

