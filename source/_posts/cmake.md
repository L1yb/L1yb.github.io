---
title: "CMake"
date: 2022/2/11 21:17
---

# CMake

## CMakeLists.txt 

```cmake
PROJECT(projectname [CXX] [C] [Java])
```
用这个指令**定义工程名称**，并且可以指定工程支持的语言，支持的语言列表是可以忽略的，默认情况表示支持所有语言。这个指令隐式的定义了两个cmake的变量：
```cmake
<projectname>_BINARY_DIR
<projectname>_SOURCE_DIR
```
这两个变量可以用（这样不用担心写错工程名称）。
```cmake
PROJECT_BINARY_DIR
PROJECT_SOURCE_DIR
```
```cmake
SET(VAR [VALUE] [CACHE TYPE DOCSTRING [FORCE]])
```
这里先了解**SET指令可以用来显示的定义变量**即可。这里是
```cmake
SET(SRC_LIST main.c)
```
如果有多个源文件，也可以定义为：
```cmake
SET(SRC_LIST main.c t1.c t2.c)
```
```cmake
MESSAGE([SEND_ERROR | STATUS | FATAL_ERROR] "message")
```

这个指令是向终端输出用户定义的信息，包含三种类型：    

```cmake
SEND_ERROR#产生错误，生成过程被跳过。
STATUS#输出前缀为--d的信息。
FATAL_ERROR#立即终止所有的cmake过程。
```

```cmake
ADD_EXECUTABLE(hello ${SRC_LIST})
```
**ADD_EXECUTABLE定义了一个为hello的可执行文件**，相关的源文件是SRC_LIST中定义的源文件列表。

本例可以简化为如下CMakeList.txt
```cmake
PROJECT(HELLO)
ADD_EXECUTABLE(hello main.c)
```

## 基本语法规则   
使用**${}**方式来取得变量中的值，而在IF语句中则直接使用变量名。
>指令（参数1 参数2 …） 

参数之间使用空格或者分号分隔开。例如加入一个函数fun.c
```cmake
ADD_EXETABLE(hello main.c;fun.c)
```
*指令是大小写无关的，参数和变量是大小写相关的*。但是推荐你全部使用大写指令。
可以使用双引号“”将源文件包含起来。处理特别难处理的名字比如`fun c.c`，则使用`SET(SRC_LIST "fun c.c")`可以防止报错。


## 清理工程
可以使用`make clean`清理makefile产生的中间的文件，但是，不能使用`make distclean`清除`cmake`产生的中间件。如果需要删除cmake的中间件，可以采用`rm -rf ***`  来删除中间件。

## examples
本小节的任务：
1. 为工程添加一个子目录`src`，用来放置工程源代码
2. 添加一个子目录`doc`，用来放置工程源代码
3. 在工程目录添加文本文件`COPYRIGHT`，`README`
4. 在工程目录添加一个`runhello.sh`脚本，用来调用hello二进制
5. 将构建后的目标文件放入构建目录的bin子目录；
6. 最终安装这些文件：将hello二进制与`runhello.sh`安装到`/usr/bin`，将doc目录的内容以及`COPYRIGHT/README`安装到`/usr/share/doc/cmake/t2`。

### 1、准备工作

创建t2文件夹，并添加`CMakeLists.txt`。



###  2、添加子目录

创建src文件夹，并添加`CMakeLists.txt`、`main.cpp`

顶层CMakeLists：

```cmake
PROJECT(name)

add_subdirectory(src bin)
```
src下CMakeLists:
```cmake
add_executable(hello main.cpp)
```

```bash
mkdir build
cd build/
cmake ..
make
```
> use`rm -rf *` 清空build文件夹内    

构建成功后会在build/bin中发现目标文件hello。

语法解释：
```cmake
ADD_SUBDIRECTORY(source_dir [binary_dir] [EXCLUDE_FROM_ALL] )
```
>这个指令用于**向当前工程添加存放源文件的子目录。并可以指定中间二进制和目标二进制存放的位置**。EXCLUDE_FROM_ALL参数的含义是将这个目录从编译过程中排除，比如，工程中的example，可能就需要工程构建完成后，再进入example目录单独进行构建（当然，你可以通过定义依赖来解决此类问题）。

上面的例子定义了将src子目录加入工程，并指定编译输出（包含编译中间结果）路径为bin目录。如果不进行bin目录的指定，那么编译结果（包括中间结果）都将存放在build/src目录（这个目录跟原来的src目录对应），指定bin目录后，相当于在编译时将src重命名为bin，所有的中间结果和目标二进制都贱存放在bin目录中。

如果在上面的例子中将`ADD_SUBDIRECTORY(src bin)`改成`SUBDIRS(src)`。那么在build目录中将出现一个src目录，生成的目标代码hello将存在在src目录中。

这里提一下，SUBDIRS指令，使用的方法是：`SUBDIRS(dir1 dir2 …)`,但是这个指令已经不推荐使用了。他可以一次添加多个子目录，并且，即是外部编译，子目录体系仍然会被保存。

### 3、换个位置保存二进制文件

不管是`SUBDIRS`还是`ADD_SUBDIRECTORY`指令（不论是否指定编译输出目录），我们都可以通过SET指令**重新定义**`EXECUTABLE_OUTPUT_PATH`和`LIBRARY_OUTPUT_PATH`变量来指定最终的目标二进制的位置（指最终生成的hello或者最终的共享库，不包括编译生成的中间文件）
```cmake
SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
```
如果是外部编译，指的是外部编译所在目录，也就是本例中的build目录。
那么有这么多的CMakeLists.txt，应该把以上的两条指令写在哪？一个简单原则，在哪里`ADD_EXECUTABLE`或者`ADD_LIBRARY`，如果需要改变目标存放路径，就在哪里加上上述的定义。在这个例子中，则是src下的CMakeLists.txt。



