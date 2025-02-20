## git配置

- [SSH](git@github.com:SuperSaiyanJcy/learn-repo.git)

```shell
git init [project-name]
git config --global user.name "SuperSaiyanJcy"
git config --global user.email "SuperSaiyanJcy@gmail.com"
git config --global credential.helper store
git config --global --list                                  
git config --global init.defaultBranch main     # 设置默认分支为main
git config --global --get init.defaultBranch                
```

## SSH密钥

```shell
ssh-keygen -t rsa -b 4096   # 默认路径为：~/user/.ssh
cat id_rsa.pub
```
```shell
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
- `-t rsa`：指定密钥类型为RSA（常用类型）
- `-b 4096`：指定密钥位数为4096位，增加密钥安全性（2048/4096?）
- `-C "your_email@example.com"`：为密钥添加注释

- 使用`SSH`密钥进行无密码登录需要将公钥`id_rsa.pub`添加到远程服务器的 `~/.ssh/authorized_keys` 文件中

## git 基本用法

```shell
git status
git add *.txt
git add .
git log [--graph] [--oneline]
git log --oneline --graph --decorate --all
git commit -m "msg"     # 提交所有暂存区的文件到仓库
git commit -am "msg"    # 提交所有已修改的文件到仓库
git ls-files            # 查看暂存区文件
git rm file             # 暂存区和工作区同时删除
rm xxx.txt              # 删除工作区，暂存区并没删除
git rm --cached [file]  # 删除暂存区，保留工作区
git rm -r*              # 递归删除目录下所有子目录及文件
```

## git tag

```shell
# 使用git tag可以列出所有现有的标签
git tag                                     # 标签不会一起被push，需要手动push
git tag [tag_name]                          # git tag v1.0.0
# 创建带有注释的标签
git tag -a [tag_name] -m "your message"     # git tag -a v1.0.0 -m "First release"
# 创建标签并指定提交
git tag [tag_name] [commit_hash]            # git tag v1.0.0 9fceb02
# 创建带有注释的标签并指定提交
git tag -a <tag_name> <commit_hash> -m "your message"       # git tag -a v1.0.0 9fceb02 -m "Initial release"
# 删除本地标签
git tag -d [tag_name]
# 删除远程标签，先删除本地
git push --delete ori [tag_name]
# git tag -d v1.0.0
# git push --delete origin v1.0.0
git push ori [tag_name]                     # 推送单个标签到远程
git push ori --tags                         # 推送多个标签
# 查看标签信息
git show [tag_name]
```

## git remote

```shell
git remote -v               # 查看当前所有远程地址别名
git remote add remote-alias repo-addr
git remote set-url remote-alias new-repo-addr   # 更改远程仓库地址
git remote rename [old-name] [new-name]
```

## git push|fetch|pull|clone

```shell
# [-f] --force [-u] --set-upstream
git push [-f] [-u] remote-alias remote-branch[:local-branch]
# pull = fetch + merge
git fetch remote-alias remote-branch[:local-branch]
git pull remote-alias remote-branch[:local-branch]
git clone remote-addr
```

## git branch|merge|rebase

```shell
git branch [-r/-a]              # 默认查看本地分支 -r查看远程分支 -a查看所有分支
git branch [new-branch-name]
git checkout [-b] branch-name   # (-b 创建一个新的分支)切换到指定分支
git switch branch-name          # checkout也用来恢复文件，避免分支名与文件名冲突
git branch -vv                  # 查看远端与本地分支关联关系
git branch -d [branch-name]     # 删除已经合并分支
git branch -D [branch-name]     # 删除未合并的分支
git branch -m master main       # rename,master->main
git merge branch-name           # 将branch-name分支合并到该分支
git merge --abort               # 中止合并
git rebase branch-name          # 往上找到branch-name分支与当前分支的共同祖先，嫁接移植
----------------------------------------------------------------------------------
# main1 main2 main3       main4 main5 main6
#                         dev1 dev2 dev3
# git switch main
# git rebase dev
# main1 main2 main3 dev1 dev2 dev3 main4 main5 main6
# git switch dev
# git rebase main
# main1 main2 main3 main4 main5 main6 dev1 dev2 dev3
----------------------------------------------------------------------------------
git checkout -b branch-name version     # 恢复到删除的branch-name分支的这一时间点
```

## git reset

```shell
git reset --soft [版本号]       # 保留工作区与暂存区内容
git reset --hard [版本号]       # 丢弃工作区与暂存区内容
git reset --mixed [版本号]      # 保留工作区，丢弃暂存区
git reset --hard HEAD^          # HEAD~4，回退4个版本
# 误操作，查看操作历史记录，回退到版本号
git reflog
```

## git diff

```shell
# 红色表示删除的内容，绿色表示添加的内容 
git diff                        # 比较工作区和暂存区
git diff HEAD                   # 比较工作区和最新提交
git diff --cached               # 比较暂存区和最新提交（HEAD）
git diff 759eace d925f32/HEAD   # 比较两个提交之间的差异
git diff [branch-name]          # 比较当前分支和另一个分支的差异
git diff HEAD~ HEAD XXX.txt     # 只查看指定文件的差异
```

## git stash

```shell
# stash操作可以“储存”工作现场，等以后恢复现场继续工作，-u表示把所有未跟踪文件一起存储，-a表示所有未跟踪+已忽略的，save参数表示存储的信息，可以不写
git stash save "msg"
git stash list                  # 查看所有stash
git stash pop                   # 恢复最近一次stash
git stash pop stash@{2}         # 恢复指定的stash stash@{2}表示第三个，stash@{0}表示最近的
git stash apply                 # 重新接受最近一次的stash
git stash drop stash@{2}        # pop和apply的区别是，pop会把stash的内容删除，apply不会，可以通过此条命令删除
git stash clear                 # 删除所有stash

```

## .gitignore

```shell
# 再添加.gitignore之前已经添加到版本库中的不被忽略
# 常用于忽略编译产生的中间文件和结果文件，运行时生成的日志文件、缓存文件、临时文件，密钥等
# .gitignore  匹配规则，从上往下执行
# 以#号开头注释
----------------------------------------------------------------------------------
# *通配任意个字符
# ?匹配单个字符
# []匹配列表中的单个字符，e.g. [abc]表示a/b/c
# **匹配任意的中间目录
# [0-9],[a-z],[A-Z]
----------------------------------------------------------------------------------
# e.g.
# 忽略所有的.a文件
# *.a
# 但是跟踪所有的 lib.a 即使在前面忽略了 .a文件
# !lib.a
# 只忽略当前目录下的T0D0文件，而不忽略subdir/T0D0 subdir子目录
# /T0D0
# 忽略任何目录下名为build的文件夹
# build/
# 忽略 doc/11.txt 但不忽略 doc/sub/222.txt
# doc/*.txt
# 忽略doc/目录及其所有子目录下的.pdf文件
# doc/**/*.pdf
----------------------------------------------------------------------------------
```

## shell补充

```shell
alias graph="git log --oneline --graph --decorate --all"
unalias name(graph)
set-alias name              # powershell中
remove-item alias:name
echo "some log" > 111.log   
echo "some log" >> 111.log  # >> 另起一行追加内容
echo 111.log > .gitignore
rm -r dirname               # 删除文件夹
```

## git 工作流程

```
主版本      major version
次版本      minor version
修订版本    patch version
main hotfix release develop feature 功能开发分支，稳定后合并到develop
```
