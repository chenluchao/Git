# Git学习记录

## Git和代码托管中心
* 局域网：GitLab
* 互联网：
  * GitHub(外网)
  * Gitee码云(国内网站)
## 安装
* 非中文无空格目录（最好是默认）
## 生成ssh
1. `cat ~/.ssh/id_rsa.pub`查看是否有key若有这复制后添加到git战虎下ssh,若无则继续
2. `ssh-keygen -t rsa -C 'git地址'`在C:\user\Administrator.ssh下生成两个文件：私钥：id_rsa公钥：id_rsa.pub
## Git常用配置命令

命令名称|作用
:--|:--
`git config --global user.name 用户名`|设置用户名
`git config --global user.email 邮箱`|设置签名
`git reflog`|查看历史记录（显示简要信息）
`git log`|查看历史记录(显示完整信息)
`git reset --hard 版本号`|版本穿梭
`git config --global alias.ci 'commit'`|命名别名（给commit添加ci别名）
`git config branch.分支名.description '备注信息'`|给分支添加备注信息
`git config branch.分支名.description`|查看分支备注信息
## Git分支操作
命令名称|作用
:--|:--
`git branch 分支名`|创建分支
`git checkouk -b 分支名`|创建新分支并切换到新分支上
`git branch -v`|查看分支(不包括远程分支)
`git branch -a`|查看分支(包括远程分支)
`git checkout 分支名`|切换分支
`git merge 分支名`|把指定分支合并到当前分支上
## 远程仓库相关
参数名称|作用
:--|:--
`git remote -v`|查看当前所有远程地址别名
`git remote add 别名 远程地址`|给远程地址添加别名
`git push 别名 分子名`|将该分支提交到该远程库
`git pull 别名 分子名`|从远程代码库的某分支上拉取该代码
## 代码合并
* `git cherry-pick [commitHash]`|合并某次提交的代码
* `git cherry-pick`配置如下：

参数名称|作用
:--|:--
`-e,--edit`|打开外部编辑器，编辑提交信息
`-n,--no-commit`|只更新工作区和暂存区，不产生新的提交
`-x`|在提交信息的末尾添加一行（`cherry pick from commit ...`）方便以后查到这个提交是如何产生的
`-s,--signoff`|在提交信息的末尾添加一行坐着的签名，表示是谁进行了操作
* `git cherry-pick`发生冲突解决：

命令名称|作用
:--|:--
`--continue`|用户解决完冲突后先`add`然后使用`git cherry-pick --continue`
`--abort`|放弃代码合并回到操作前的样子
`--quit`|代码发生冲突后退出cherry pick，但是不回到操作前
## Git保存当前进度
命令名称|作用
:--|:--
`git stash save '日志信息'`|保存进度
`git stash list`|显示进度列表
`git stash apply [--index] [<stash>]`|用存档记录覆盖当前代码
`git stash pop`|用存档覆盖当前代码同时删除该存档
`git stash drop [<stash>]`|删除一个存档
`git stash clear`|删除所有存档
`git stash branch <branchname> <stash>`|基于进度创建分支
## 修改提交日志
* 未push:`git commit --amend`
* 已push:
  1. `git commit --amend`
  2. `git push orign master --force`
## Git版本回退
* reset(不推荐)
  1. `git reset --hard <111>`
  2. `git push -f -u orgin [dev]`
  * 备注：问题服务器上代码虽然被还原了，但是其他人的版本依然是回退前的版本，如果别人再提交那么回退操作就白做了。
* revert(推荐)
  1. `git revert -n 版本号`
  2. `git commit -m '信息'`
  3. `git push`
  * 这种回退方式是生成一个新的版本，只是该版本代码和需要回退的目标版本代码一致。但是在提交日志上看是一致往前走的，此时只要别人更新代码就回退了。
## 将多个commit合并未一个push
1. 使用`--soft`参数调用重置命令，回到最近两次提交之前：`git reset --soft HEAD^^`
2. 查看版本状态：`git status`
3. 查看日志信息：`git log -1`
4. 执行提交操作，即完成最新两次提交压缩为一个提交的操作：`git commit -m 'message'`
5. 查看提交日志（确认悔棋操作成功）：`git log --stat -pretty==oneline -2`

## 一些问题
* 清除npm缓存`npm cache clean --force`
* 合并代码发生冲突，解决完冲突add后commit后不能添加文件名，否则报错。
* git忽略文件无效
  * `git rm -r --cached .` 清除缓存（该操作会删除本地所有文件的缓存）
  * `git add .`
  * `git commit -m 'message'`
  * `git push`