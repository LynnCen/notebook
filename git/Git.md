# Git

文档：https://www.w3cschool.cn/git/git-basic-operations.html
https://gitmoji.dev/
回退到指定版本：

git reset --hard 139dcfaa558e3276b30b6b2e5cbbb9c00bbdca96

查看项目的远程地址：

git remote -v

查看远程项目所有分支，红色分支代表当前所在分支，其他的所列就是所有分支了：

git branch -a

查看历史提交信息：

git log

检查修改内容：

git status

查看当前仓库基本信息：

git remote show origin

git commit -m '' 提交信息

### 分支管理

查看远程分支 ` git branch -a`
查看本地分支 ` git branch`
创建分支: `git branch (brachname) `
切换分支：` git checkout (brachname)`
合并分支命令 ` git merge`
删除分支 ` git branch -d (branchname)`

撤销commit
git reset --soft HEAD^

git push --set-upstream origin bugfix-20240411

github 贡献值不显示问题：
提交必须使用与github账号关联的电子邮箱地址
` git config user.email`
`  git config --global user.email 邮箱地址` # 对所有仓库设置电子邮箱地址
`  git config --local user.email 邮箱地址` # 仅对当前仓库设置电子邮箱地址


