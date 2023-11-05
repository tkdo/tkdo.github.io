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
 - 语法： 







