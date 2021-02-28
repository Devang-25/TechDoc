# Git Command

__Git常用命令__
| 命令 | 说明 |
| ---- | ---- |
| git add | 用添加文件到暂存区(stage) |
| git commit -m <commit_message> | 提交代码到本地仓库 |
| git status | 查看工作区 |
| git fetch -p | 同步远程仓库到本地 |
| git diff HEAD -- <file_name> | 可以查看工作区和版本库里面最新版本的区别 |
| git log --graph | 可以看到分支合并图 |
| git log -1 | 让其显示最后一次提交信息 |
| git reflog | 记录你的每一次命令 |
| git reset --hard HEAD | 回滚到上一个版本。在Git中，用HEAD表示当前版本，也就是最新的提交，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100 |
| git reset --hard <commit_id> | 回滚到commit ID指定的版本 |
| git reset HEAD <file_name> | 可以把暂存区的修改撤销掉(unstage)，重新放回工作区。git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区 |
| git checkout <branch_name> | 切换分支 |
| git checkout -b <branch_name> | 创建branch_name分支，然后切换到branch_name分支，-b参数表示创建并切换，相当于以下两条命令：git branch branch_name，git checkout branch_name |
| git checkout -b <branch_name> origin/<branch_name> | 创建远程origin的branch_name分支到本地 |
| git checkout -- <file_name> | 把file_name文件在工作区的修改全部撤销，这里有两种情况：一种是file_name自修改后还没有被放到暂存区，现在撤销修改就回到和版本库一模一样的状态；一种是file_name已经添加到暂存区后，又作了修改，撤销修改就回到添加到暂存区后的状态。总之，就是让这个文件回到最近一次git commit或git add时的状态。git checkout -- <file_name>命令中的--很重要，没有--，就变成了"切换到另一个分支"的命令 |
| git rm <file_name> | 删除文件时你通常直接在文件管理器中把没用的文件删了，确实要从版本库中删除该文件，那就用命令git rm删掉。另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本git checkout -- <file_name> |
| git remote | 查看远程库的信息 |
| git remote -v | 显示更详细的信息 |
| git push origin <branch_name> | 推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上。如果要推送其他分支，比如dev，就改成：git push origin dev |
| git push origin <local_branch>:<remote_branch> | 把指定本地分支上推送到指定远程分 |
| git push origin -d <branch_name> | 远程删除git服务器上的分支 |
| git branch | 查看当前分支 |
| git branch -a | 查看远程分支 |
| git branch -d <branch_name> | 删除branch_name分支 |
| git branch --set-upstream <branch_name> origin/<branch_name> | 指定本地branch_name分支与远程origin/branch_name分支的链接 |
| git merge | 用于合并指定分支到当前分支，注意到上面的Fast-forward信息，Git告诉我们，这次合并是"快进模式"，也就是直接把master指向dev的当前提交，所以合并速度非常快 |
| git merge --no-ff -m <commit message> <branch_name> | 通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。请注意--no-ff参数，表示禁用Fast forward |
| git rebase -i <commit_id> | 合并commit_id以后的提交 |
| git stash | Git还提供了一个stash功能，可以把当前工作现场"储藏"起来，等以后恢复现场后继续工作 |
| git stash list | 执行完git stash后，用git status查看工作区，就是干净的，除非有没有被Git管理的文件。工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令查看 |
| git stash apply stash@{0} | 你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash。恢复有两个办法：一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除。另一种方式是用git stash pop，恢复的同时把stash内容也删了 |
| git tag <tag_name> | 打一个名称为tag_name新标签 |
| git tag | 查看所有标签 |
| git tag <tag_name> <commit_id> | 默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？方法是找到历史提交的commit id，然后打上就可以了 |
| git show <tag_name> | 查看标签信息 |
| git tag -a <tag_name> -m "tag_message" <commit_id> | 创建带有说明的标签，用-a指定标签名，-m指定说明文字 |
| git tag -d <tag_name> | 删除标签 |
| git push origin <tag_name> | 推送某个标签到远程 |
| git push origin --tags | 一次性推送全部尚未推送到远程的本地标签。如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：git tag -d tag_name。然后，从远程删除。删除命令也是push，但是格式如下：git push origin :refs/tags/tag_name |
| git config --global color.ui true | 让Git显示颜色，会让命令输出看起来更醒目。配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用，配置文件放哪了？每个仓库的Git配置文件都放在.git/config文件中，而当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中 |
| git config --global alias.st status | 告诉Git，以后st就表示status|