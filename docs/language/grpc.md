
## 1、安装依赖项：
```bash
sudo yum install -y epel-release
sudo yum install -y git gcc-c++ make openssl-devel gflags-devel protobuf-devel protobuf-compiler leveldb-devel cmake3
```

2、安装brpc
### Q
In file included from /root/gitee/brpc_playground/echo_service/client.cpp:1:0:
/root/gitee/brpc_playground/echo_service/client.h:1:26: 致命错误：brpc/channel.h：没有那个文件或目录
### A
下载brpc源码包：https://gitee.com/baidu/BRPC.git
```bash
mkdir bld
cd bld
cmake3 -DBUILD_SHARED_LIBS=ON ..
make -j 8
sudo make install
```

## 3、libbrpc不存在
### Q
./server: error while loading shared libraries: libbrpc.so: cannot open shared object file: No such file or directory
### A
```bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib64
find / -name libbrpc.so*
```

