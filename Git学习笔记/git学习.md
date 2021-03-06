#Git学习

#安装Git
###首先，你可以试着输入`git`，看看系统有没有安装Git:
	zxw-lzqdeMacBook-Air:~ zxw-lzq$ git
	
没安装,可以下载Xcode,便自动安装
###安装完成后，还需要最后一步设置，在命令行输入：
	$ git config --global user.name "Your Name"
	$ git config --global user.email "email@example.com"
因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。

注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。  

---
###创建版本库
> 什么是版本库呢？版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

####创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：

	$ mkdir learngit
	$ cd learngit
	$ pwd
	/Users/michael/learngit
	
pwd:查看当前目录路径
####第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：
	$ git init
	Initialized empty Git repository in /Users/michael/learngit/.git/
	
瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。  

也不一定必须在空目录下创建Git仓库，选择一个已经有东西的目录也是可以的。不过，不建议你使用自己正在开发的公司项目来学习Git，否则造成的一切后果概不负责。  
  
####现在我们编写一个readme.rtf文件，内容如下：
	Git is a version control system.
	Git is free software.
	
一定要放到learngit目录下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。
##把一个文件放到Git仓库只需要两步
#####第一步，用命令`git add`告诉Git，把文件添加到仓库：
	
	$ git add readme.txt
执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。    
#####第二步，用命令`git commit`告诉Git，把文件提交到仓库：
	$ git commit -m "wrote a readme file"
	[master (root-commit) cb926e7] wrote a readme file
	 1 file changed, 2 insertions(+)
	 create mode 100644 readme.txt
简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。  
git commit命令执行成功后会告诉你，1个文件被改动（我们新添加的readme.txt文件），插入了两行内容（readme.txt有两行内容）。

为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：

	$ git add file1.txt
	$ git add file2.txt file3.txt
	$ git commit -m "add 3 files."  
---

#时光机穿梭
我们已经成功地添加并提交了一个readme.txt文件，现在，是时候继续工作了，于是，我们继续修改readme.txt文件，改成如下内容：

	Git is a distributed version control system.
	Git is free software.
现在，运行`git status`命令看看结果：

	$ git status
	# On branch master
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#    modified:   readme.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")  
`git status`命令可以让我们时刻掌握仓库当前的状态，上面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。  
虽然Git告诉我们readme.txt被修改了，但如果能看看具体修改了什么内容，需要用`git diff`这个命令看看

	$ git diff readme.txt 
	diff --git a/readme.txt b/readme.txt
	index 46d49bf..9247db6 100644
	--- a/readme.txt
	+++ b/readme.txt
	@@ -1,2 +1,2 @@
	-Git is a version control system.
	+Git is a distributed version control system.
	 Git is free software.  
`git diff`顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到，我们在第一行添加了一个“distributed”单词。      

知道了对readme.txt作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是`git add`：
	
	$ git add readme.txt  
同样没有任何输出。在执行第二步`git commit`之前，我们再运行`git status`看看当前仓库的状态： 
 
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       modified:   readme.txt
	#  
  
`git status`告诉我们，将要被提交的修改包括readme.txt，下一步，就可以放心地提交了:  
  
	$ git commit -m "add distributed"
	[master ea34578] add distributed
	 1 file changed, 1 insertion(+), 1 deletion(-)  
提交后，我们再用`git status`命令看看仓库的当前状态：  

	$ git status
	# On branch master
	nothing to commit (working directory clean)  
  

**要随时掌握工作区的状态，使用`git status`命令。**

**如果git status告诉你有文件被修改过，用`git diff`可以查看修改内容。**      

---
##版本回退
现在，你已经学会了修改文件，然后把修改提交到Git版本库，现在，再练习一次，修改readme.txt文件如下：

	Git is a distributed version control system.
	Git is free software distributed under the GPL.  
  
然后尝试提交：  
  
	$ git add readme.txt
	$ git commit -m "append GPL"
	[master 3628164] append GPL
	 1 file changed, 1 insertion(+), 1 deletion(-)  
像这样，你不断对文件进行修改，然后不断提交修改到版本库里，就好比玩RPG游戏时，每通过一关就会自动把游戏状态存盘，如果某一关没过去，你还可以选择读取前一关的状态。有些时候，在打Boss之前，你会手动存盘，以便万一打Boss失败了，可以从最近的地方重新开始。Git也是一样，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失。  
  
在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。版本控制系统肯定有某个命令可以告诉我们历史记录.
###在Git中，我们用`git log`命令查看：  
	$ git log
	commit 3628164fb26d48395383f8f31179f24e0882e1e0
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Tue Aug 20 15:11:49 2013 +0800

    	append GPL

	commit ea34578d5496d7dd233c827ed32a8cd576c5ee85
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Tue Aug 20 14:53:12 2013 +0800

	    add distributed

	commit cb926e7ea50ad11b8f9e909c05226233bf755030
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Mon Aug 19 17:51:55 2013 +0800

    	wrote a readme file
`git log`命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是append GPL，上一次是add distributed，最早的一次是wrote a readme file。

如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：    

	$ git log --pretty=oneline
	3628164fb26d48395383f8f31179f24e0882e1e0 append GPL
	ea34578d5496d7dd233c827ed32a8cd576c5ee85 add distributed
	cb926e7ea50ad11b8f9e909c05226233bf755030 wrote a readme file
	  
需要友情提示的是，你看到的一大串类似`3628164...882e1e0`的是`commit id`（版本号），和SVN不一样，Git的commit id不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示，而且你看到的`commit id`和我的肯定不一样，以你自己的为准。为什么`commit id`需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。  
  
每提交一个新版本，实际上Git就会把它们自动串成一条时间线。如果使用可视化工具查看Git历史，就可以更清楚地看到提交历史的时间线  
###现在我们启动时光穿梭机，准备把readme.txt回退到上一个版本  
首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`3628164...882e1e0`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`  
  
现在，我们要把当前版本“append GPL”回退到上一个版本“add distributed”，就可以使用`git reset`命令：  
  
	$ git reset --hard HEAD^
	HEAD is now at ea34578 add distributed  
  
看看readme.txt的内容是不是版本add distributed： `cat readme.text`   

	$ cat readme.txt
	Git is a distributed version control system.
	Git is free software.  
  
果然。

还可以继续回退到上一个版本wrote a readme file，不过且慢，然我们用`git log`再看看现在版本库的状态：    

	$ git log
	commit ea34578d5496d7dd233c827ed32a8cd576c5ee85
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Tue Aug 20 14:53:12 2013 +0800

    	add distributed

	commit cb926e7ea50ad11b8f9e909c05226233bf755030
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Mon Aug 19 17:51:55 2013 +0800

	    wrote a readme file  
  
最新的那个版本append GPL已经看不到了！好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？

办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个append GPL的`commit id`是`3628164...`，于是就可以指定回到未来的某个版本：    
	
	$ git reset --hard 3628164
	HEAD is now at 3628164 append GPL  
  
现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的commit id怎么办？  
  
在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到add distributed版本时，再想恢复到append GPL，就必须找到append GPL的`commit id`。Git提供了一个命令`git reflog`用来记录你的每一次命令：  
  
	$ git reflog
	ea34578 HEAD@{0}: reset: moving to HEAD^
	3628164 HEAD@{1}: commit: append GPL
	ea34578 HEAD@{2}: commit: add distributed
	cb926e7 HEAD@{3}: commit (initial): wrote a readme file  
  
###小结


HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。  
  
##工作区和暂存区    
  
> Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。先来看名词解释。  

> **工作区（Working Directory）**  
> 就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区.  
> **版本库（Repository）**  
> 工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。
> Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。  
  
分支和HEAD的概念我们以后再讲。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。  
  
因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
  
###俗话说，实践出真知。现在，我们再练习一遍，先对readme.txt做个修改，比如加上一行内容：    

	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.  
  
然后，在工作区新增一个LICENSE文本文件（内容随便写）。

先用git status查看一下状态：  
  
	$ git status
	# On branch master
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   readme.txt
	#
	# Untracked files:
	#   (use "git add <file>..." to include in what will be committed)
	#
	#       LICENSE
	no changes added to commit (use "git add" and/or "git commit -a")  
  
Git非常清楚地告诉我们，`readme.txt`被修改了，而`LICENSE`还从来没有被添加过，所以它的状态是`Untracked`。

现在，使用两次命令`git add`，把`readme.txt`和`LICENSE`都添加后，用`git status`再查看一下：  
  
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       new file:   LICENSE
	#       modified:   readme.txt
	#  
  
`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。  

	$ git commit -m "understand how stage works"
	[master 27c9860] understand how stage works
	 2 files changed, 675 insertions(+)
	 create mode 100644 LICENSE  
  
一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：  

	$ git status
	# On branch master
	nothing to commit (working directory clean)  
  
暂存区就没有任何内容了  
###小结

暂存区是Git非常重要的概念，弄明白了暂存区，就弄明白了Git的很多操作到底干了什么。  

---  
  
##管理修改    
Git管理的是修改，而不是文件  

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

你看，我们前面讲了，Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

提交后，用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别：

	$ git diff HEAD -- readme.txt 
	diff --git a/readme.txt b/readme.txt
	index 76d770f..a9c5755 100644
	--- a/readme.txt
	+++ b/readme.txt
	@@ -1,4 +1,4 @@
	 Git is a distributed version control system.
	 Git is free software distributed under the GPL.
	 Git has a mutable index called stage.
	-Git tracks changes.
	+Git tracks changes of files.  
  
那怎么提交第二次修改呢？你可以继续`git add`再`git commit`，也可以别着急提交第一次修改，先`git add`第二次修改，再`git commit`，就相当于把两次修改合并后一块提交了：

第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`  
  
###小结

现在，你又理解了Git是如何跟踪修改的，每次修改，如果不add到暂存区，那就不会加入到commit中。

---  
  
##撤销修改  
自然，你是不会犯错的。不过现在是凌晨两点，你正在赶一份工作报告，你在readme.txt中添加了一行  

	$ cat readme.txt
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes of files.
	My stupid boss still prefers SVN.  
  
在你准备提交前，一杯咖啡起了作用，你猛然发现了“stupid boss”可能会让你丢掉这个月的奖金！

既然错误发现得很及时，就可以很容易地纠正它。你可以删掉最后一行，手动把文件恢复到上一个版本的状态。如果用git status查看一下：  
  
	$ git status
	# On branch master
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   readme.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")  
  
你可以发现，Git会告诉你，`git checkout -- file`可以丢弃工作区的修改：  

	$ git checkout -- readme.txt  
	
命令`git checkout -- readme.txt`意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态,`git commit`时的状态

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态,`git add`时的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。    

`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。  
  
现在假定是凌晨3点，你不但写了一些胡话，还`git add`到暂存区了：  

	$ cat readme.txt
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes of files.
	My stupid boss still prefers SVN.
	
	$ git add readme.txt

庆幸的是，在`commit`之前，你发现了这个问题。用`git status`查看一下，修改只是添加到了暂存区，还没有提交：  

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       modified:   readme.txt
	#  
  
Git同样告诉我们，用命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区：  

	$ git reset HEAD readme.txt
	Unstaged changes after reset:
	M       readme.txt  
  
`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。    

还记得如何丢弃工作区的修改吗？  

	$ git checkout -- readme.txt
	
	$ git status
	# On branch master
	nothing to commit (working directory clean)  
整个世界终于清静了！  
  
现在，假设你不但改错了东西，还从暂存区提交到了版本库，怎么办呢？还记得版本回退一节吗？可以回退到上一个版本。不过，这是有条件的，就是你还没有把自己的本地版本库推送到远程。还记得Git是分布式版本控制系统吗？我们后面会讲到远程版本库，一旦你把“stupid boss”提交推送到远程版本库，你就真的惨了……  
  
###小结

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。  
  
---
##删除文件  
在Git中，删除也是一个修改操作，我们实战一下，先添加一个新文件test.txt到Git并且提交：  

	$ git add test.txt
	$ git commit -m "add test.txt"
	[master 94cdc44] add test.txt
	 1 file changed, 1 insertion(+)
	 create mode 100644 test.txt  
  
一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：  

	$ rm test.txt  
  
这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

	$ git status
	# On branch master
	# Changes not staged for commit:
	#   (use "git add/rm <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       deleted:    test.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")  
  
现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

	$ git rm test.txt
	rm 'test.txt'
	$ git commit -m "remove test.txt"
	[master d17efd8] remove test.txt
	 1 file changed, 1 deletion(-)
	 delete mode 100644 test.txt  
  
现在，文件就从版本库中被删除了。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：  

	$ git checkout -- test.txt  
  
`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。  
###小结

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会**丢失最近一次提交后你修改的内容**。  

---
  
#远程仓库  
>到目前为止，我们已经掌握了如何在Git仓库里对一个文件进行时光穿梭，你再也不用担心文件备份或者丢失的问题了。

>可是有用过集中式版本控制系统SVN的童鞋会站出来说，这些功能在SVN里早就有了，没看出Git有什么特别的地方。

>没错，如果只是在一个仓库里管理文件历史，Git和SVN真没啥区别。为了保证你现在所学的Git物超所值，将来绝对不会后悔，同时为了打击已经不幸学了SVN的童鞋，本章开始介绍Git的杀手级功能之一（注意是之一，也就是后面还有之二，之三……）：远程仓库。  

完全可以自己搭建一台运行Git的服务器，不过现阶段，为了学Git先搭个服务器绝对是小题大作。好在这个世界上有个叫GitHub的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。  
  
在继续阅读后续内容前，请自行注册GitHub账号。由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

###第1步  
创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

	$ ssh-keygen -t rsa -C "youremail@example.com"  
你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。
###第2步  
登陆GitHub，打开“Account settings”，“SSH Keys”页面：
![image](http://www.liaoxuefeng.com/files/attachments/001384908342205cc1234dfe1b541ff88b90b44b30360da000/0)  

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容.  

点“Add Key”，你就应该看到已经添加的Key
![image](http://www.liaoxuefeng.com/files/attachments/0013849083502905a4caa2dc6984acd8e39aa5ae5ad6c83000/0)      

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。  
  
最后友情提示，在GitHub上免费托管的Git仓库，任何人都可以看到喔（但只有你自己才能改）。所以，不要把敏感信息放进去。

如果你不想让别人看到Git库，有两个办法，一个是交点保护费，让GitHub把公开的仓库变成私有的，这样别人就看不见了（不可读更不可写）。另一个办法是自己动手，搭一个Git服务器，因为是你自己的Git服务器，所以别人也是看不见的。这个方法我们后面会讲到的，相当简单，公司内部开发必备。

确保你拥有一个GitHub账号后，我们就即将开始远程仓库的学习。    

###小结

“有了远程仓库，妈妈再也不用担心我的硬盘了。”——Git点读机

---  
##添加远程库  
>现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。  
  
###首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：
![image](http://www.liaoxuefeng.com/files/attachments/0013849084639042e9b7d8d927140dba47c13e76fe5f0d6000/0)  
  
###在Repository name填入`learngit`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：
![image](http://www.liaoxuefeng.com/files/attachments/0013849084720379a3eae576b9f417da2add578c8612a2e000/0)  

目前，在GitHub上的这个`learngit`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。    

###现在，我们根据GitHub的提示，在本地的learngit仓库下运行命令：    
	$ git remote add origin git@github.com:GitHub用户名/learngit.git  
添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。  
  
###下一步，就可以把本地库的所有内容推送到远程库上：  
	$ git push -u origin master
	Counting objects: 19, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (19/19), done.
	Writing objects: 100% (19/19), 13.73 KiB, done.
	Total 23 (delta 6), reused 0 (delta 0)
	To git@github.com:michaelliao/learngit.git
	 * [new branch]      master -> master
	Branch master set up to track remote branch master from origin.  
把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支master推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：
![image](http://www.liaoxuefeng.com/files/attachments/00138490848464619aebd9a2bb0493c83e132ca1eed6f66000/0)  
###从现在起，只要本地作了提交，就可以通过命令：  
	$ git push origin master  
把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！  
  
###SSH警告

当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告：    

	The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
	RSA key fingerprint is xx.xx.xx.xx.xx.
	Are you sure you want to continue connecting (yes/no)?  
这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：  
  
	Warning: Permanently added 'github.com' (RSA) to the list of known hosts.  
  
这个警告只会出现一次，后面的操作就不会有任何警告了。

如果你实在担心有人冒充GitHub服务器，输入`yes`前可以对照GitHub的`RSA Key`的指纹信息是否与SSH连接给出的一致。  
  
###小结

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

克隆分支:`git clone -b dev  git@github.com:hevi1991/learngit.git`

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！  
  
---  
  
##从远程库克隆  
上次我们讲了先有本地库，后有远程库的时候，如何关联远程库。

###现在，假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。  
  
首先，登陆GitHub，创建一个新的仓库，名字叫`gitskills`：  
![image](http://www.liaoxuefeng.com/files/attachments/0013849085474010fec165e9c7449eea4417512c2b64bc9000/0)    
我们勾选`Initialize this repository with a README`，这样GitHub会自动为我们创建一个`README.md`文件。创建完毕后，可以看到`README.md`文件：  
![image](http://www.liaoxuefeng.com/files/attachments/0013849085607106c2391754c544772830983d189bad807000/0)  
  
现在，远程库已经准备好了，下一步是用命令git clone克隆一个本地库：  
	
	$ git clone git@github.com:GitHub用户名/gitskills.git
	Cloning into 'gitskills'...
	remote: Counting objects: 3, done.
	remote: Total 3 (delta 0), reused 0 (delta 0)
	Receiving objects: 100% (3/3), done.
	
	$ cd gitskills
	$ ls
	README.md  
注意把Git库的地址换成你自己的，然后进入gitskills目录看看，已经有README.md文件了。  

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

你也许还注意到，GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。实际上，Git支持多种协议，默认的git://使用`ssh`，但也可以使用https等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放`http`端口的公司内部就无法使用`ssh协议`而只能用`https`。

###小结

要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。  
  
---  
  
#分支管理  
分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。

如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了Git又学会了SVN！  
![image](http://www.liaoxuefeng.com/files/attachments/001384908633976bb65b57548e64bf9be7253aebebd49af000/0)    
分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

其他版本控制系统如SVN等都有分支管理，但是用过之后你会发现，这些版本控制系统创建和切换分支比蜗牛还慢，简直让人无法忍受，结果分支功能成了摆设，大家都不去用。

但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。    
  
---
##创建与合并分支  
在`版本回退`里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master分支`。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：  
![image](http://www.liaoxuefeng.com/files/attachments/0013849087937492135fbf4bbd24dfcbc18349a8a59d36d000/0)  
  
每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长：  
[link](http://github.liaoxuefeng.com/sinaweibopy/video/master-branch-forward.mp4)  

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把HEAD指向`dev`，就表示当前分支在dev上：  
![image](http://www.liaoxuefeng.com/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)  
  
你看，Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：  
![image](http://www.liaoxuefeng.com/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0)  
  
假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。`Git`怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：  
![image](http://www.liaoxuefeng.com/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)  
  
所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除`dev`分支。删除dev分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：  
  
![image](http://www.liaoxuefeng.com/files/attachments/001384908867187c83ca970bf0f46efa19badad99c40235000/0)  
  
整个流程:
[link](http://github.liaoxuefeng.com/sinaweibopy/video/master-and-dev-ff.mp4)  
  
###开始实战  
####首先，我们创建dev分支，然后切换到dev分支：    
	$ git checkout -b dev
	Switched to a new branch 'dev'
####`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：  
	$ git branch dev
	$ git checkout dev
	Switched to branch 'dev'
####然后，用`git branch`命令查看当前分支：  
	$ git branch
	* dev
	  master  
`git branch`命令会列出所有分支，当前分支前面会标一个*号。  
然后，我们就可以在`dev`分支上正常提交，比如对readme.txt做个修改，加上一行：    

	Creating a new branch is quick.    
  
然后提交：  

	$ git add readme.txt 
	$ git commit -m "branch test"
	[dev fec145a] branch test
	 1 file changed, 1 insertion(+)  
  
####现在，`dev`分支的工作完成，我们就可以切换回`master`分支：  

	$ git checkout master
	Switched to branch 'master'  
切换回`master`分支后，再查看一个`readme.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并**没有变**：    
![image](http://www.liaoxuefeng.com/files/attachments/001384908892295909f96758654469cad60dc50edfa9abd000/0)  
####现在，我们把`dev`分支的工作成果合并到`master`分支上：  

	$ git merge dev
	Updating d17efd8..fec145a
	Fast-forward
	 readme.txt |    1 +
	 1 file changed, 1 insertion(+)

`git merge`命令用于合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。  

当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

####合并完成后，就可以放心地删除`dev`分支了：  

	$ git branch -d dev
	Deleted branch dev (was fec145a).
	
删除后，查看`branch`，就只剩下`master`分支了：  
  
	$ git branch
	* master  
  
因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。    
  
###小结

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`    
  
---
##解决冲突

>人生不如意之事十之八九，合并分支往往也不是一帆风顺的。  

准备新的`feature1`分支，继续我们的新分支开发：

	$ git checkout -b feature1
	Switched to a new branch 'feature1'
	
修改readme.txt最后一行，改为：  

	Creating a new branch is quick AND simple.

在`feature`1分支上提交：  

	$ git add readme.txt 
	$ git commit -m "AND simple"
	[feature1 75a857c] AND simple
	 1 file changed, 1 insertion(+), 1 deletion(-)

切换到`master`分支：  
  
	$ git checkout master
	Switched to branch 'master'
	Your branch is ahead of 'origin/master' by 1 commit.  
  
Git还会自动提示我们当前`master`分支比远程的`master`分支要超前1个提交。    

在master分支上把readme.txt文件的最后一行改为：    

	Creating a new branch is quick & simple.  
  
提交：  
  
	$ git add readme.txt 
	$ git commit -m "& simple"
	[master 400b400] & simple
	 1 file changed, 1 insertion(+), 1 deletion(-)  
现在，master分支和feature1分支各自都分别有新的提交，变成了这样：  
  
![image](http://www.liaoxuefeng.com/files/attachments/001384909115478645b93e2b5ae4dc78da049a0d1704a41000/0)  
  
这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：  

	$ git merge feature1
	Auto-merging readme.txt
	CONFLICT (content): Merge conflict in readme.txt
	Automatic merge failed; fix conflicts and then commit the result.  
  
果然冲突了！Git告诉我们，readme.txt文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件：  
  
	$ git status
	# On branch master
	# Your branch is ahead of 'origin/master' by 2 commits.
	#
	# Unmerged paths:
	#   (use "git add/rm <file>..." as appropriate to mark resolution)
	#
	#       both modified:      readme.txt
	#
	no changes added to commit (use "git add" and/or "git commit -a")
  
我们可以`直接`查看readme.txt的内容：  
  
	Git is a distributed version control system.
	Git is free software distributed under the GPL.
	Git has a mutable index called stage.
	Git tracks changes of files.
	<<<<<<< HEAD
	Creating a new branch is quick & simple.
	=======
	Creating a new branch is quick AND simple.
	>>>>>>> feature1    
Git用`<<<<<<<，=======，>>>>>>>`标记出不同分支的内容，我们修改如下后保存：    

	Creating a new branch is quick and simple.
  
再提交：  
	  
	$ git add readme.txt 
	$ git commit -m "conflict fixed"
	[master 59bc1cb] conflict fixed      
现在，master分支和feature1分支变成了下图所示：  
![image](http://www.liaoxuefeng.com/files/attachments/00138490913052149c4b2cd9702422aa387ac024943921b000/0)  
  
用带参数的git log也可以看到分支的合并情况：    
	
	$ git log --graph --pretty=oneline --abbrev-commit
	*   59bc1cb conflict fixed
	|\
	| * 75a857c AND simple
	* | 400b400 & simple
	|/
	* fec145a branch test
	...  
  
最后，删除`feature1`分支：  
  
	$ git branch -d feature1
	Deleted branch feature1 (was 75a857c).
  
###小结

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

用`git log --graph`命令可以看到分支合并图。

---  

##分支管理策略  
通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的`commit`，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-ff`方式的`git merge`：

###首先，仍然创建并切换dev分支：  
  
	$ git checkout -b dev
	Switched to a new branch 'dev'
	修改readme.txt文件，并提交一个新的commit：
	
	$ git add readme.txt 
	$ git commit -m "add merge"
	[dev 6224937] add merge
	 1 file changed, 1 insertion(+)
###现在，我们切换回`master`：

	$ git checkout master
	Switched to branch 'master'
###准备合并dev分支，请注意`--no-ff`参数，表示`禁用Fast forward`：
	
	$ git merge --no-ff -m "merge with no-ff" dev
	Merge made by the 'recursive' strategy.
	 readme.txt |    1 +
	 1 file changed, 1 insertion(+)
###因为本次合并要创建一个新的`commit`，所以加上`-m`参数，把`commit`描述写进去。

合并后，我们用`git log`看看分支历史：

	$ git log --graph --pretty=oneline --abbrev-commit
	*   7825a50 merge with no-ff
	|\
	| * 6224937 add merge
	|/
	*   59bc1cb conflict fixed
	...

要是没有使用--no-ff参数,就会少了  `* 6224937 add merge这一部分的分支`

可以看到，不使用`Fast forward`模式，`merge`后就像这样：  
![image](http://www.liaoxuefeng.com/files/attachments/001384909222841acf964ec9e6a4629a35a7a30588281bb000/0)  
  

###分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

所以，团队合作的分支看起来就像这样：  
  
![image](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)  
  
###小结

Git分支十分强大，在团队开发中应该充分应用。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

---  
  
##Bug分支  
  
软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。  

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：  

	$ git stash
	Saved working directory and index state WIP on dev: 6224937 add merge
	HEAD is now at 6224937 add merge

工作区是干净的，刚才的工作现场用`git stash list`命令看看：  
  
	$ git stash list
	stash@{0}: WIP on dev: 6224937 add merge

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，`stash`内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：    

	$ git stash pop
	# On branch dev
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       new file:   hello.py
	#
	# Changes not staged for commit:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   readme.txt
	#
	Dropped refs/stash@{0} (f624f8e5f082f2df2bed8a4e09c12fd2943bdd40)

###小结
当手头工作没有完成时，先把工作现场`git stash`一下，然后开新的分支去修复bug，修复后，再`git stash pop`，回到工作现场。

---
##Feature分支  
软件开发中，总有无穷无尽的新的功能要不断添加进来。

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。
开发一个新feature，最好新建一个分支；  


###如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。  

---
  
##多人协作  
    
当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`：

	$ git remote
	origin  
  
或者，用`git remote -v`显示更详细的信息：  
 
	$ git remote -v
	origin  git@github.com:michaelliao/learngit.git (fetch)
	origin  git@github.com:michaelliao/learngit.git (push)  
  
上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。  
  
###推送分支  
  推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：  
  
	$ git push origin master
如果要推送其他分支，比如dev，就改成：

	$ git push origin dev  
但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

`master`分支是主分支，因此要时刻与远程同步；

`dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

`bug`分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

`feature`分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！    

###抓取分支    

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：
  
	$ git clone git@github.com:michaelliao/learngit.git
	Cloning into 'learngit'...
	remote: Counting objects: 46, done.
	remote: Compressing objects: 100% (26/26), done.
	remote: Total 46 (delta 16), reused 45 (delta 15)
	Receiving objects: 100% (46/46), 15.69 KiB | 6 KiB/s, done.
	Resolving deltas: 100% (16/16), done.
	
当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的`master`分支。不信可以用`git branch`命令看看：
	
	$ git branch
	* master
####现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：

	$ git checkout -b dev origin/dev
	

####现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

	$ git commit -m "add /usr/bin/env"
	[dev 291bea8] add /usr/bin/env
	 1 file changed, 1 insertion(+)
	$ git push origin dev
	Counting objects: 5, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (2/2), done.
	Writing objects: 100% (3/3), 349 bytes, done.
	Total 3 (delta 0), reused 0 (delta 0)
	To git@github.com:michaelliao/learngit.git
	   fc38031..291bea8  dev -> dev  
你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

	$ git add hello.py 
	$ git commit -m "add coding: utf-8"
	[dev bd6ae48] add coding: utf-8
	 1 file changed, 1 insertion(+)
	$ git push origin dev
	To git@github.com:michaelliao/learngit.git
	 ! [rejected]        dev -> dev (non-fast-forward)
	error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
	hint: Updates were rejected because the tip of your current branch is behind
	hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
	hint: before pushing again.
	hint: See the 'Note about fast-forwards' in 'git push --help' for details.
推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

	$ git pull
	remote: Counting objects: 5, done.
	remote: Compressing objects: 100% (2/2), done.
	remote: Total 3 (delta 0), reused 3 (delta 0)
	Unpacking objects: 100% (3/3), done.
	From github.com:michaelliao/learngit
	   fc38031..291bea8  dev        -> origin/dev
	There is no tracking information for the current branch.
	Please specify which branch you want to merge with.
	See git-pull(1) for details
	
	    git pull <remote> <branch>
	
	If you wish to set tracking information for this branch you can do so with:
	
	    git branch --set-upstream dev origin/<branch>
`git pull`也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

	$ git branch --set-upstream dev origin/dev
	Branch dev set up to track remote branch dev from origin.
	再pull：
	
	$ git pull
	Auto-merging hello.py
	CONFLICT (content): Merge conflict in hello.py
	Automatic merge failed; fix conflicts and then commit the result.
这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push：

	$ git commit -m "merge & fix hello.py"
	[dev adca45d] merge & fix hello.py
	$ git push origin dev
	Counting objects: 10, done.
	Delta compression using up to 4 threads.
	Compressing objects: 100% (5/5), done.
	Writing objects: 100% (6/6), 747 bytes, done.
	Total 6 (delta 0), reused 0 (delta 0)
	To git@github.com:michaelliao/learngit.git
	   291bea8..adca45d  dev -> dev   
因此，多人协作的工作模式通常是这样：

首先，可以试图用`git push origin branch-name`推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！

如果`git pull`提示`“no tracking information”`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。 

###小结

查看远程库信息，使用`git remote -v`；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。    
  
---
#标签管理  
发布一个版本时，我们通常先在版本库中打一个标签，这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。  

##创建标签
在Git中打标签非常简单，首先，切换到需要打标签的分支上：

	$ git branch
	* dev
	  master
	$ git checkout master
	Switched to branch 'master'
然后，敲命令`git tag <name>`就可以打一个新标签：

	$ git tag v1.0
可以用命令`git tag`查看所有标签：

	$ git tag
	v1.0
默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

	$ git log --pretty=oneline --abbrev-commit
	6a5819e merged bug fix 101
	cc17032 fix bug 101
	7825a50 merge with no-ff
	6224937 add merge
	59bc1cb conflict fixed
	400b400 & simple
	75a857c AND simple
	fec145a branch test
	d17efd8 remove test.txt
	...
比方说要对`add merge`这次提交打标签，它对应的commit id是`6224937`，敲入命令：

	$ git tag v0.9 6224937
再用命令`git tag`查看标签：

	$ git tag
	v0.9
	v1.0
注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息：

	$ git show v0.9
	commit 622493706ab447b6bb37e4e2a2f276a20fed2ab4
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Thu Aug 22 11:22:08 2013 +0800
	
	    add merge
	...
可以看到，`v0.9`确实打在`add merge`这次提交上。

还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：

	$ git tag -a v0.1 -m "version 0.1 released" 3628164
用命令`git show <tagname>`可以看到说明文字：

	$ git show v0.1
	tag v0.1
	Tagger: Michael Liao <askxuefeng@gmail.com>
	Date:   Mon Aug 26 07:28:11 2013 +0800
	
	version 0.1 released
	
	commit 3628164fb26d48395383f8f31179f24e0882e1e0
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Tue Aug 20 15:11:49 2013 +0800
	
	    append GPL
还可以通过`-s`用私钥签名一个标签：

$ git tag -s v0.2 -m "signed version 0.2 released" fec145a
签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错：

	gpg: signing failed: secret key not available
	error: gpg failed to sign the data
	error: unable to sign the tag
如果报错，请参考GnuPG帮助文档配置Key。

用命令`git show <tagname>`可以看到PGP签名信息：

	$ git show v0.2
	tag v0.2
	Tagger: Michael Liao <askxuefeng@gmail.com>
	Date:   Mon Aug 26 07:28:33 2013 +0800
	
	signed version 0.2 released
	-----BEGIN PGP SIGNATURE-----
	Version: GnuPG v1.4.12 (Darwin)
	
	iQEcBAABAgAGBQJSGpMhAAoJEPUxHyDAhBpT4QQIAKeHfR3bo...
	-----END PGP SIGNATURE-----
	
	commit fec145accd63cdc9ed95a2f557ea0658a2a6537f
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Thu Aug 22 10:37:30 2013 +0800
	
	    branch test
###小结

命令`git tag <name>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；

`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

`git tag -s <tagname> -m "blablabla..."`可以用PGP签名标签；

命令`git tag`可以查看所有标签。

---  
##操作标签  
  
如果标签打错了，也可以删除：

	$ git tag -d v0.1
	Deleted tag 'v0.1' (was e078af9)
因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令`git push origin <tagname>`：

	$ git push origin v1.0
	Total 0 (delta 0), reused 0 (delta 0)
	To git@github.com:michaelliao/learngit.git
	 * [new tag]         v1.0 -> v1.0
或者，一次性推送全部尚未推送到远程的本地标签：

	$ git push origin --tags
	Counting objects: 1, done.
	Writing objects: 100% (1/1), 554 bytes, done.
	Total 1 (delta 0), reused 0 (delta 0)
	To git@github.com:michaelliao/learngit.git
	 * [new tag]         v0.2 -> v0.2
	 * [new tag]         v0.9 -> v0.9
如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

	$ git tag -d v0.9
	Deleted tag 'v0.9' (was 6224937)
然后，从远程删除。删除命令也是push，但是格式如下：

	$ git push origin :refs/tags/v0.9
	To git@github.com:michaelliao/learngit.git
	 - [deleted]         v0.9
要看看是否真的从远程库删除了标签，可以登陆GitHub查看。  

###小结

命令`git push origin <tagname>`可以推送一个本地标签；

命令`git push origin --tags`可以推送全部未推送过的本地标签；

命令`git tag -d <tagname>`可以删除一个本地标签；

命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。  
  
---  
  
#使用GitHub
  
如何参与一个开源项目呢？比如人气极高的bootstrap项目，这是一个非常强大的CSS框架，你可以访问它的项目主页https://github.com/twbs/bootstrap，点`“Fork”`就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone：  

	git clone git@github.com:michaelliao/bootstrap.git  
  
一定要从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址`git@github.com:twbs/bootstrap.git`克隆，因为没有权限，你将不能推送修改。  
Bootstrap的官方仓库twbs/bootstrap、你在GitHub上克隆的仓库my/bootstrap，以及你自己克隆到本地电脑的仓库，他们的关系就像下图显示的那样：  
![image](http://www.liaoxuefeng.com/files/attachments/001384926554932eb5e65df912341c1a48045bc274ba4bf000/0)  
如果你想修复bootstrap的一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送。

如果你希望bootstrap的官方库能接受你的修改，你就可以在GitHub上发起一个`pull request`。当然，对方是否接受你的`pull request`就不一定了。    
  
##小结

在GitHub上，可以任意`Fork`开源仓库；

自己拥有`Fork`后的仓库的读写权限；

可以推送`pull request`给官方仓库来贡献代码。

---
#完,谢谢廖雪峰老师!
