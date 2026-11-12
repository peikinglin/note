git的四个概念   远程主干、远程个人分支、本地主干、本地个人分支

origin/main 是远程仓库主干（网页端）
origin/pengshanlin_dev 是服务器上你的个人远程分支（网页端，用于将个人开发的代码合并到主干）

main：个人本地主干（方便快速拿到主干最新代码，只用来同步主干）
pengshanlin_dev 个人本地开发分支（在这里做开发）

git status 查看当前指向的路径，如果是指向本地main，有可能会显示落后远程main若干个commit的情况
                            如果是指向本地个人分支，则有可能会显示本地个人分支的改动情况（相对本地main）

A。更新主干最新代码git指令用法
方法一、先更新本地main，再更新本地分支
git checkout main    #切本地main
git pull origin main #把本地main更新到服务器最新  等价于git fetch origin + git merge origin/main
git checkout pengshanlin_dev #切本地分支
git merge main       #合并【本地main】到自己分支

# 👉 这里如果出现代码冲突，解决冲突后：
# git add .
# git commit -m "merge main最新代码到pengshanlin_dev"

#推送本地开发分支，更新远程个人分支 origin/pengshanlin_dev
git push origin pengshanlin_dev

方法二、绕过本地main，直接更新本地分支
git checkout pengshanlin_dev #切本地分支
git fetch origin
git merge origin/main      #直接拿远程仓库合并本地分支

# 👉 这里如果出现代码冲突，解决冲突后：
# git add .
# git commit -m "merge main最新代码到pengshanlin_dev"

#推送本地开发分支，更新远程个人分支 origin/pengshanlin_dev
git push origin pengshanlin_dev

B.代码提交
commit 之前，查看具体改动点
前提：代码改完，**还没 git add** / 刚 git add，还没 commit
--查看工作区（未 add）和当前分支的差异（最常用）
git diff
直接输出：**哪些行新增、哪些行删除**，`+`新增，`-`删除。
只对比：**工作区文件 vs 暂存区**，如果已经执行 `git add`，`git diff` 就看不到内容了
--如果已经 git add 加入暂存了，想看暂存里面的改动
git diff --staged
--只想看改动文件列表
git status
--查看某个指定文件的详细改动
git diff src/test.py
# add之后想看这个文件暂存改动
git diff --staged src/test.py
