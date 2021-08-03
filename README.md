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

## `git rebase`
* `git rebase`用于把一个分支的修改合并到当前分支
* `git rebase`配置如下：

命令名称|作用
:--|:--
`--continue`|用户解决完冲突后先`add`然后使用`git rebase --continue`
`--abort`|放弃代码合并回到操作前的样子
`--quit`|代码发生冲突后退出cherry pick，但是不回到操作前
* `git rebase -i  [startpoint]  [endpoint]`弹出交互式的界面让用户编辑完成合并操作前开后闭（可以使用startpoint^让其包含前）
  * 交互式界面参数配置：
    * pick：保留该commit（缩写:p）
    * reword：保留该commit，但我需要修改该commit的注释（缩写:r）
    * edit：保留该commit, 但我要停下来修改该提交(不仅仅修改注释)（缩写:e）
    * squash：将该commit和前一个commit合并（缩写:s）
    * fixup：将该commit和前一个commit合并，但我不要保留该提交的注释信息（缩写:f）
    * exec：执行shell命令（缩写:x）
    * drop：我要丢弃该commit（缩写:d）
* 使用场景
  1. 合并分支多个commit为一个进行提交
    * `git rebase -i HEAD~4`
    * 自动进入 vi 编辑模式根据上面交互式配置选择操作
    * 如果异常退出了执行`git rebase --edit-todo`再次编辑，完成执行`git rebase --continue`
    * `git log`查看结果
  2. 分支合并情况一
    * 先从 master 分支切出一个 dev 分支，进行开发
    * 这时候，你的同事完成了一次 hotfix ，并合并入了 master 分支，此时 master 已经领先于你的 feature1 分支了
    * 恰巧，我们想要同步 master 分支的改动也想要保持一份干净的 commit
    * 在feature1 分支上`git rebase master`
      * 备注rebase 做了什么操作呢？首先， git 会把 feature1 分支里面的每个 commit 取消掉；其次，把上面的操作临时保存成 patch 文件，存在 .git/rebase 目录下；然后，把 feature1 分支更新到最新的 master 分支；最后，把上面保存的 patch 文件应用到 feature1 分支上；
  3. 分支合并情况二
    * 从 master 分支切出一个 dev 分支，进行开发
    * master有了新的提交
    * dev也有了新的提交，现在要将量分支代码进行合并
    * 在dev分支`git rebase -i master`编辑完成
    * `git push -f`将dev分支代码强制更新提交
    * 切换到master分支`git rebase dev`这样master代码和dev就一样了
    * `git push`将master分支代码提交
    * 完成合并
    
### `git cherry-pick`
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
## Git里程碑
命令名称|作用
:--|:--
`git tag <tagName>`|创建本地tag
`git push origin :<tagName>`|推送某个tag到远程仓库
`git push origin --tags`|推送所有tag到远程仓库
`git tag -a <tagName> <commitId>`|以某一个特定的提交创建tag
`git show <tagName>`|查看本地某个 tag 的详细信息
`git tag`或者`git tag -l`|查看本地所有 tag
`git ls-remote --tags origin`|查看远程所有 tag
`git tag -d <tagName>`|本地 tag 的删除
`git push origin :refs/tags/<tagName>`|远程 tag 的删除(`git push origin :refs/tags/12345`)
`git tag -a <tagname> -m "XXX..."`|指定标签信息——例`git tag -a v0.1.0 -m "release 0.1.0 version"`：创建附注标签。
`git checkout [tagname]`|切换标签
## 修改提交日志
* 未push:`git commit --amend`
* 已push:
  1. `git commit --amend`
  2. `git push origin master --force`
## Git版本回退
* reset(不推荐)
  1. `git reset --hard <111>`
  2. `git push -f -u origin [dev]`
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