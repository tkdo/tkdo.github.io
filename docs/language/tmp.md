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
### 变量使用${}方式取值 ，但是在IF控制语句中直接使用变量名
## 重要的指令和CMake常用变量
#### 重要指令
<!-- ToDo ： add a c++ project 多目录构建 -->
-  cmake_minimum_required 指定CMake的最小版本要求
    - 语法： cmake_minimum_required(VERSION versionNUmber[FATAL_ERROR])
```c++
# CMake 最小版本是2.8.3
cmake_minimum_required(VERSiON 2.8.3)
```
- project 定义工程名，并可制定工程支持的语言
    - 语法：project(projectname [CXX][C][Java])





