## 项目记录-Git

>  Git 操作，提交代码时需要在 commit 中添加故事 ID，无法直接 commit

删除最新的提交，**并保留**已完成的工作：`git reset --soft HEAD~1`

删除最近 100 次的提交，**并保留**已完成的工作：`git reset --soft HEAD~100`

删除最新的提交，**放弃**已完成的工作:`git reset --hard HEAD~1`

删除最近 100 次的提交，**放弃**已完成的工作:`git reset --hard HEAD~100`





