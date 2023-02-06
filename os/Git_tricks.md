
#### 概念和设计

**Git暂存区**

.git/index（暂存区）实际上就是包含文件索引的的目录树，记录的（用于跟踪工作区文件的）时间戳、长度等信息判断工作区文件是否改变

存在git对象库，文件索引建立了文件和对象库中对象实体的对应关系。

	         git add：文件加入暂存区                    git commit -m：暂存区提交到分支
	工作区  --------------------------->暂存区（stage、index） -------------------------> 分支

**git对象**

	Git 内核包含blob，tree，commit三类对象，blob存数据，tree存结构，commit存历史，blob和tree之间通过hash key关联。

	commit和tree关联，和blob不直接关联
	
	commit -->tree-->blob

	commit: Head，tag，branch作为版本控制的业务领域概念，属于功能层，仅仅通过commit关联
	
	git记录整个文件快照

**git命令**

git checkout:
	
	git checkout [-q][<commit>][--]<paths>
	
	git checkout [<branch>]
	
	git checkout [-m][[-b|--orphan] <new_branch> [start_point]]


git reset [--soft | --mixed | --hard ][<commit>] 默认mixed
	
	hard = 1 2 3, soft = 1,	mixed = 1 2
	
	1 替换引用   2 替换暂存区  3 替换工作区

git log --pretty=（online行显示）（format:"%h %s" --graph）
	
		git log --graph --pretty=oneline --abbrev-commit

git cat-file -p  + commit --->  git cat-file -p + tree ID  查看提交内容
 
git merge XXX

git stash ---> git stash list -->git stash pop --> git stash save “message”

修改历史的命令行：

1 amend:  git commit –amend

2 pick:

git tag

checkout + cherry pick：打上tag 1 2 3 4,checkout 1,然后cherry pick 3  4，然后git reset --soft HEAD^^,然后git commit -m 

	git reset HEAD^            # 回退所有内容到上一个版本

3 rebase 变基：

强大的git rebase 是对提交执行变基操作，即可以实现将制定范围的提交“嫁接”到另外一个提交之上：

git rebase –onto <newbase>  <since> <till>

git rebase   [startpoint]   [endpoint]  --onto  [branchName]: [startpoint] [endpoint]指定的是一个前开后闭的区间

	git rebase -i HEAD~4   合并最近4次的提交记录
	
	git rebase –onto 1 3^ 4  相对于cherry pick
	
git rebase dev，作用也一样是在当前分支合并Dev分支，如果git rebase遇到冲突，第一步当然是解决冲突，然后 git add，之后并不需要git commit，而是直接运行git rebase --continue，这样git 就会继续应用剩下的补丁了，//假如你不想解决冲突且不再进行合并，那么可以使用git rebase --abort
	
**git fetch与merge**
	
git fetch        →→ 这将更新git remote 中所有的远程repo 所包含分支的最新commit-id, 将其记录到.git/FETCH_HEAD文件中
	
FETCH_HEAD： 是一个版本链接，记录在本地的一个文件中，指向着目前已经从远程仓库取下来的分支的末端版本。

git pull : 首先，基于本地的FETCH_HEAD记录，比对本地的FETCH_HEAD记录与远程仓库的版本号，然后git fetch 获得当前指向的远程分支的后续版本的数据，然后再利用git merge将其与本地的当前分支合并。	

git merge Dev // Dev表示某分支，表示在当前分支合并Dev分支
	
**git fetch + rebase**

使用git fetch和git rebase处理多人开发同一分支的问题: https://blog.csdn.net/azureternite/article/details/76154807
	
** git rebase成功后，如何撤销**

git rebase成功后，撤销： https://www.jianshu.com/p/e6241b7891bf

```git
git reflog，可以查看所有操作日志
	
git reset --hard HEAD@{26}  回到初始状态

	
```
	
	
	
	
	
	
	
	
	
