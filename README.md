# Git学习记录

## Git和代码托管中心
* 局域网：GitLab
* 互联网：
  * GitHub(外网)
  * Gitee码云(国内网站)
## 安装
* 非中文无空格目录（最好是默认）
## 生成ssh
* `cat ~/.ssh/id_rsa.pub`查看是否有key若有这复制后添加到git战虎下ssh,若无则继续
* `ssh-keygen -t rsa -C 'git地址'`在C:\user\Administrator.ssh下生成两个文件：私钥：id_rsa公钥：id_rsa.pub
## Git常用命令

命令名称|作用
:--:|:--:
`git config --global user.name 用户名`|设置用户名
`git config --global user.email 邮箱`|设置签名
`git reflog`|查看历史记录（显示简要信息）
`git log`|查看历史记录(显示完整信息)
`git reset --hard 版本号`|版本穿梭
`git config --global alias.ci 'commit'`|命名别名（给commit添加ci别名）