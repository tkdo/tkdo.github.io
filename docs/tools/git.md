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
