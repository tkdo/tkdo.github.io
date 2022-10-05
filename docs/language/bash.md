
# 常用命令
## 1. 修改linux提示
- 命令
      ```bash
      export PS1='[r@v\W]$'
      ```
- 效果
      >[r@vworkspace]


## 2. 查看显存状态
- 命令
      ``` bash
      nvidia-smi   -lms 5000 # 每隔5s打印一次
      ```
- 效果

## 3. scp命令
- 命令
      ```bash
      scp -P port fileName user@ip:/dirName
      ```
- 解释
      ```bash
      -P: 大写的P, 指定端口号
      fileName: 指需要传输的文件路径, 建议使用绝对路径
      user: 远程主机用户名
      ip: 远程主机IP地址
      dirName: 远程主机可以写入文件的目录
      ```

参考：
- https://www.cnblogs.com/wsnan/p/11769838.html


