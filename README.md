# personal-git-study

## 查看 Git 配置

git config --local -l

## git push

git push --set-upstream origin branch1: 如果是首次将本地的新建分支上传到远程仓库,需要进行这一步

- git push --set-upstream origin branch1:将本地分支推到远程仓库进行管理
- git push 不带任何参数:只会将活动分支 push 到远程仓库，不会推送所有分支。
- 以后在此分支开发时,只需要 git push 就可以上传该分支的更新

## git pull

git pull origin branch1:branch1: 如果刚进入分支开发工作,拉取同事们在分支上已有的成果时,需要进行这一步

- git pull origin branch1:branch1: 当本地不存在 branch1 分支,而远程仓库已有 branch1 分支时, 将远程 branch1 分支拉到本地
- git pull 不带任何参数:只会更新当前活动分支，不会更新所有分支。
- 以后在此分支开发时,只需要 git pull 就可以拉取该分支的更新

## git merge

1. git checkout branch1
2. git pull
3. git checkout master
4. git pull
5. git merge branch1 --no-ff --no-commit
6. 进行该版本的回归测试
7. 如果回归测试无法通过, 需要回头检查问题时,通过 git reset --hard HEAD^ 可以放弃所有的 merge, 回到 branch1 分支修改错误, 再重新进行一次合并流程
8. 如果回归测试通过, git commit -m"合并完成"
9. git push
10. 如果 git push 失败, 代表 master 上有新的提交, 那么 git reset --hard HEAD^ 放弃所有的 merge, 回到步骤 3 重新更新 master 分支后,再按顺序执行分支合并. 这样做的好处是可以保持 master 上的提交一直被纳入到 master 分支的第一个父级历史中.

- 先完成需要合并分支的最后更新, 然后回到 master 分支进行合并操作
- git merge 之前, 回到 master 分支完成 master 分支的更新
- git merge 一定要在 master 分支上进行
- git merge 分支时, 加上 --no-ff 参数防止快速合并, 这样 branch1 分支上的这些提交就不会纳入到 master 分支的第一父级历史中
- git merge 分支时, 加上 --no-commit 参数防止合并分支结果被提交, 正式提交合并结果前,最好进行一次回归测试, 没有任何问题后,再手动进行一次 commit
- 最后,git push 将合并了分支的 master 分支上传远程仓库
- git reset 操作后，可以通过 git reflog 查看 git 操作日志，找回丢失的 commit 号

## 删除分支

1. git branch -d branch1: 删除本地分支 branch1

- 这个操作相对安全,因为如果分支内容没 merge 入主分支, 删除操作会报错(危险操作: -D 参数可以强制删除分支)

2. git push origin --delete branch1: 删除远程分支 branch1

## 特殊情况, 分支开发完成之前, master 上产生了一些重要更新, 并且分支需要应用这些更新

1. git checkout master
2. git pull --ff-only : 只允许快进合并,防止在 master 上做一次意外的合并
3. git checkout branch1
4. git merge --no-ff master
