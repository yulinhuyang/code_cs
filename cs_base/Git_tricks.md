
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
	
[rebase用法小结](https://www.jianshu.com/p/4a8f4af4e803)

