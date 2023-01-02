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
## tag用法
### 1.1 创建标签
- 附注标签
    -  git tag -a \<tagname\> -m \<commit\> 
    - 最简单的方式是当你在运行 tag 命令时指定 -a 选项
    -  案例：
        ```bash
        $ git tag -a v1.4 -m "my version 1.4"
        $ git tag
        v0.1
        v1.3
        v1.4
        ```
- 轻量标签
    -  git tag \<tagname\> -m \<commit\> 
    -  轻量标签本质上是将提交校验和存储到一个文件中——没有保存任何其他信息。 创建轻量标签，不需要使用 -a、-s 或 -m 选项，只需要提供标签名字
    - 案例：
        ```bash
        $ git tag v1.4-lw
        $ git tag
        v0.1
        v1.3
        v1.4
        v1.4-lw
        v1.5
        ```
-   标签diff
    - git show \<tagname\>
    -  如果在标签上运行 git show，你不会看到额外的标签信息。 命令只会显示出提交信息：
    - 案例：
        ```bash
        $ git show v1.4-lw
        commit ca82a6dff817ec66f44342007202690a93763949
        Date:   Mon Mar 17 21:52:11 2008 -0700
            changed the version number
        ```
### 1.2 共享标签
- git push origin \<tagname\>
    - 在创建完标签后你必须显式地推送标签到共享服务器上。
    - 案例：
        ```bash
        $ git push origin v1.5
        Counting objects: 14, done.
        Delta compression using up to 8 threads.
        Compressing objects: 100% (12/12), done.
        Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
        Total 14 (delta 3), reused 0 (delta 0)
        To git@github.com:schacon/simplegit.git
        * [new tag]         v1.5 -> v1.5
        ```
-  git push origin --tags
    -   如果想要一次性推送很多标签，也可以使用带有 --tags 选项的 git push 命令。 这将会把所有不在远程仓库服务器上的标签全部传送到那里。
    - 案例：
        ```bash
        $ git push origin --tags
        Counting objects: 1, done.
        Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
        Total 1 (delta 0), reused 0 (delta 0)
        To git@github.com:schacon/simplegit.git
        * [new tag]         v1.4 -> v1.4
        * [new tag]         v1.4-lw -> v1.4-lw
        ```
### 1.3 删除标签
- git tag -d \<tagname\>
    - 要删除掉你本地仓库上的标签.
    - 案例：可以使用以下命令删除一个轻量标签
        ```bash
        $ git tag -d v1.4-lw
        Deleted tag 'v1.4-lw' (was e7d5add)
        ```
- git push origin --delete <tagname>
    -  要删除掉你远程仓库上的标签

### 1.4 删除标签
- git checkout \<tagname\>
    - 虽然这会使你的仓库处于“分离头指针（detached HEAD）”的状态——这个状态有些不好的副作用：
    - 案例：
        ```
        $ git checkout 2.0.0
        Note: checking out '2.0.0'.

        You are in 'detached HEAD' state. You can look around, make experimental
        changes and commit them, and you can discard any commits you make in this
        state without impacting any branches by performing another checkout.

        If you want to create a new branch to retain commits you create, you may
        do so (now or later) by using -b with the checkout command again. Example:

        git checkout -b <new-branch>

        HEAD is now at 99ada87... Merge pull request #89 from schacon/appendix-final

        $ git checkout 2.0-beta-0.1
        Previous HEAD position was 99ada87... Merge pull request #89 from schacon/appendix-final
        HEAD is now at df3f601... add atlas.json and cover image
        ```
    - 在“分离头指针”状态下，如果你做了某些更改然后提交它们，标签不会发生变化， 但你的新提交将不属于任何分支，并且将无法访问，除非通过确切的提交哈希才能访问。 因此，如果你需要进行更改，比如你要修复旧版本中的错误，那么通常需要创建一个新分支。
- git checkout -b \<branchname\> \<tagname\>
    - 如果在这之后又进行了一次提交，version2 分支就会因为这个改动向前移动， 此时它就会和 v2.0.0 标签稍微有些不同，这时就要当心了。


    





