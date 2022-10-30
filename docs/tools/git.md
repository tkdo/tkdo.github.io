## 删除分支
- 删除本地分支
    ```shell 
    git branch -d localBranchName
    ```
- 删除远程分支
    ```shell
    git push origin --delete   remotes/origin/remoteBranchName
    ```
## 保存用户名密码
- 设置记住密码（默认15分钟）：
    ```bash
    git config --global credential.helper cache
    git config credential.helper cache
    ```
- 如果想自己设置时间，可以这样做：
    ```bash
    git config credential.helper 'cache --timeout=3600' #这样就设置一个小时之后失效
    ```
- 长期存储密码：
    ```bash 
    git config --global credential.helper store
    git config credential.helper store
    ```
##  本地代码回滚
- 本地回滚
    ```bash
    git reset --hard 17336756 <commit_id>
    ```
## 查看代码差异
- 显示出branch1和branch2中差异的部分
    ```bash 
    git diff branch1 branch2 --stat
    ```
- 显示指定文件的详细差异
    ```bash
    git diff branch1 branch2 具体文件路径
    ```
-  显示出所有有差异的文件的详细差异
    ```bash 
    git diff branch1 branch2
    ```
- 查看branch1分支有，而branch2中没有的log
    ```bash
    git log branch1 ^branch2
    ```
- 查看branch2中比branch1中多提交了哪些内容
    ```bash
    git log branch1..branch2
    # 注意，列出来的是两个点后边（此处即dev）多提交的内容。
    ```
- 不知道谁提交的多谁提交的少，单纯想知道有什么不一样
    ```bash
    git log branch1...branch2
    ```
- 在上述情况下，在显示出每个提交是在哪个分支上
    ```bash
    git log -lefg-right branch1...branch2
    # 注意 commit 后面的箭头，根据我们在 –left-right branch1…branch2 的顺序，左箭头 < 表示是 branch1 的，右箭头 > 表示是branch2的。
    ```

