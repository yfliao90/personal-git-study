# personal-git-study

## 查看 Git 配置

git config --local -l

## git push

1. git push --set-upstream origin branch1:将本地分支推到远程仓库进行管理
2. git push 不带任何参数:只会将活动分支 push 到远程仓库，不会推送所有分支。
3. git push origin [branch]:将本地分支[branch] push 到远程仓库

## git merge

git merge branch1 --no-ff

- git merge 一定要在 master 分支上进行
- git merge 分支时, 最好加上 --no-ff 参数防止快速合并
