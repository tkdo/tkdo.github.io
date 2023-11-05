# cmake
## 语法特性介绍
- 基本语法格式：制造令(参数1 参数2...)
    - 参数使用括弧括起
    - 参数之间用空格或分号分开
### 指令大小写无关的，参数和变量大小写相关的
```c++
set(HELLO hello.cpp)
add_executable(hello main.cpp hello.cpp)
ADD_EXECUTABLE(hello main.cpp ${HELLO})
``` 
变量使用${}方式取值 ，但是在IF控制语句中直接使用变量名
### 重要指令
<!-- ToDo ： add a c++ project 多目录构建 -->
-  cmake_minimum_required 指定CMake的最小版本要求
    - 语法： cmake_minimum_required(VERSION versionNUmber[FATAL_ERROR])
```c++
# CMake 最小版本是2.8.3
cmake_minimum_required(VERSiON 2.8.3)
```
- project 定义工程名，并可制定工程支持的语言
    - 语法：project(projectname [CXX][C][Java])
```c++
# 指定工程名为HELLOWORd
project(HELLOWORD)
```
- set -显式的定义变量
    - 语法：set(VAR[VALUE][CACHE TYPE DOCSTRING [FORCE]])
```c++
# 定义SRC变量，其值为sayhello.cpp hello.cpp
set(SRC sayhello.cpp hello.cpp)
```
- include_directories - 向工程添加多一个特定的头文件搜索路径--->>相当于制定g++编译器的-I参数
    - 语法： include_directories([AFTER|BEFORE][SYSTEM] dir1 dir2 ...)
```c++
# 将/usr/include/myincludeforder 和 ./include 添加到头文件搜索路径
include_directories(usr/include/myincludeforder ./include)
```
- link_directories -向工程添加多个特定的库文件搜索路径 ---> 相当于指定g++编译器的-L参数
    - 语法：link_diretories(dir1 dir2 ...)
```c++
# 将/usr/lib/mylibfolder 和 ./lib 添加到库文件搜索路径
link_directoies(/usr/lib/mylibfolder ./lib)
```
- add_library -生成库文件
    - 语法： add_library(libname [SHRAED|STATIC|MODULE][EXCLUDE_FORM_ALL] source1 source2...sourceN)
```c++
# 通过变量SRC生成libhello.so共享库
add_library(hello SHARED ${SRC})
```
- add_compile_options -添加编译器参数 
 - 语法： add_compile_options(<option>...)
```c++
# 添加编译参数 -wall -std=c++11
add_compile_options(-wall -std=c++11 -o2)
```
- add_executable - 生成可执行文件
- 语法： add_executable(exename source1 source2 ... sourceN)
```c++
# 编译main.cpp生成可执行main
add_executable(main main.cpp)
```
- target_link_libraries -为target添加需要链接的共享库--->相同于制定g++的编译器-I参数
    - 语法： target_link_libraries(target library1<debug | optimized> library2 ...)
```c++
#将hello动态库文件链接到可执行的main
target_link_libraries(main hello)
```
- add_subdirectory - 向当前工程添加存放源文件目录，并可以指定中间二进制和目标二进制存放的位置
    - 语法：add_subdirectory(source_dir [binary_dir][EXCLUDE_FROM_ALL])
```
# 添加src子目录，src中需有一个CMakeLists.txt
add_subdirectory(src)
```
- aux_source_directory - 发现一个目录下所有的源代码文件并将列表存储到一个变量中，这个指令临时备用来自动构建源文件列表
    - 语法：aux_source_directory(dir VARIABLE)
```c++
# 定义SRC变量，其值为当前目录下所有文件的源代码
aux_source_directory(. SRC)
# 编译SRC变量所代表的源文件，生成main可执行文件
add_executable(main ${SRC})
```
### CMake 常用变量
- CMAKE_C_FLAGS gcc编译选项
- CMAKE_CXX_FLAGS g++编译选项
```c++
# 在CMAKE_CXX_FLAGS编译选项后追加-std=c++11
set(CMAKE_CXX_FLAGS "{CMAKE_CXX_FLAGS} -std=c++11")
```
- CMAKE_BUILD_TYPE 编译类型(Debug, Release)
```c++
# 设定编译类型为debug，调试时需要选debug
set(CMAKE_BUILD_TYPE Debug)
# 设定编译类型为release，发布时需要选择Release
set(CMAKE_BUILD_TYPE Release)
```
- CMAKE_BINARY_DIR
- PROJECT_BINARY_DIR
<projectname>_BINARY_DIR
1. 这三个变量指代的内容是一致的
2. 如果是in source build, 指的就是工程师顶层目录
2. 如果是out-of-source编译，指的是工程编译发生的目录
4. PROJECT_BINARY_DIR跟其他的指令稍有区别，不过现在，你可以理解他们是一致的。










# gdb
- 调试开始：执行gdb[exefilename]，进入gdb调试程序，其中execfilename为要调试的文件名
```bash
## 一下命令后括号内为命令简化使用，比如run(r)，直接输入命令r代替命令run
$(gdb)help(h)  # help
$(gdb)run(r)   #重新开始运行文件（run-text：加载文本文件，run-bin：加载二进制文件） 
$(gdb)start    # 单步执行，运行程序，停在第一行执行语句
$(gdb)list(l)  # 查看源代码（list-n，从第几行开始查看代码，list+函数名：查看具体的函数）
$(gdb)set      # 设置比变量的值
$(gdb)next(n)  # 单步调试（逐过程，函数直接执行）
$(gdb)step(s)  # 单步调试（逐语句，跳入自定义函数内部执行）
$(gdb)backtrace(bt) # 查看函数的调用栈帧和层级关系
$(gdb)frame(f)      # 切换函数栈帧
$(gdb)info(i)       # 查看函数内部局部变量的数值
$(gdb)finish        # 结束当前函数，返回函数调用点
$(gdb)continue(c)   # 继续运行
$(gdb)print(p)      # 打印当前的值和地址
$(gdb)quit(q)       # 推出gdb
```