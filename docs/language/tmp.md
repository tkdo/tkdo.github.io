    以下是每一部分的详细解释：
    cmake_minimum_required(VERSION 2.4.1): 这行指定了CMake的最低版本要求为2.4.1。
    find_package(Protobuf REQUIRED): 这个命令是用来查找并加载Protobuf相关的信息，比如库文件和头文件等。
    set (INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR} /usr/include): 这行设置了一个变量INCLUDE_DIR，它包含两个路径：CMAKE_CURRENT_SOURCE_DIR（当前源码目录）和/usr/include（系统级别的头文件目录）。
    set(LIB_DIR ${CMAKE_CURRENT_SOURCE_DIR}/lib /usr/lib): 这行设置了一个变量LIB_DIR，它包含两个路径：CMAKE_CURRENT_SOURCE_DIR/lib（当前源码目录下的库文件目录）和/usr/lib（系统级别的库文件目录）。
    add_compile_options(-std=c++11 -g): 这行添加了编译选项，指定使用C++11标准并开启调试信息。
    include_directories(${INCLUDE_DIR}): 这行将之前设置的INCLUDE_DIR变量中的所有路径添加到包含目录中，以便编译器能找到头文件。
    link_directories(${LIB_DIR}): 这行将之前设置的LIB_DIR变量中的所有路径添加到链接目录中，以便链接器能找到库文件。
    FILE(GLOB ALL_PROTOS "*.proto"): 这行在当前目录下寻找所有扩展名为.proto的文件，并将这些文件的路径存储在ALL_PROTOS变量中。
    然后对每个.proto文件进行处理，使用PROTOBUF_PROTOC_EXECUTABLE（Protobuf编译器）来生成对应的C++源文件和头文件，并将它们存储在PROTO_META_BASE_DIR目录中。
    之后使用add_executable命令创建两个可执行文件：client和server，它们分别链接到client.cpp和server_main.cpp以及之前生成的C++源文件和头文件。
    最后，使用target_link_libraries命令来链接所需的库文件。库文件包括：gflags, brpc, protobuf, 和 leveldb。这些库在编译时会被链接到client和server可执行文件中。

