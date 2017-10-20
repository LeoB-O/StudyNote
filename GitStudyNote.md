> 笔记纯属个人理解，并不严谨  
> 仅供参考  
> By LeoB_O
###Git 基本概念及常用命令

#### 1. Git 简介
    - Git 是一个版本控制系统(Version Control System)
    - Git 与其他CVS相比更加高效，使用也更加快捷方便
#### 2. Git 基本概念
![Three Area](https://git-scm.com/book/en/v2/images/areas.png)

+ Git 分为三个区，包括 Git仓库(Git Repository)、工作目录(Working Directory)、暂存区域(Staging Area)
    - Git仓库是保存大部分数据的地方
    - 工作目录是项目的某个版本独立提取出来的内容
    - 暂存区域保存了下次将会提交(commit)的文件列表
+ Git的基本工作流程
    1. 在工作目录中修改(modify)文件
    2. 暂存文件，将文件快照放入暂存区域(add)
    3. 提交(commit)更新，将暂存区文件保存至Git Repository。
#### 3. 建立一个项目仓库
>Git 仓库是一个用来保存各种文件的地方。在我的理解中，仓库与文件夹的概念十分相似。位于仓库中的文件会被Git所“关注”，你可以选择追踪或者不追踪一个文件。被追踪的文件的内容一旦被修改，Git会马上“知道”，并可以采取相应的措施。

**(1). 初始化一个仓库(init)**
 
如果想使用Git对现有项目进行管理，只需要将当前项目所在的文件夹设置为Git仓库即可。在命令行中进入到项目所在目录，输入：

    $ git init


**(2). 克隆一个仓库(clone)**

除了直接新建一个仓库外，还可以选择克隆一个已有的远程Git仓库。输入：

    $ git clone [url] [name]
例如克隆Git的一个可链接库libgit2：

    $ git clone https://github.com/libgit2/libgit2 mylibgit

#### 4.追踪文件

工作目录下的文件状态不外乎两种：`已追踪` 和 `未追踪`。已追踪的文件是指那些被纳入了版本控制的文件。未追踪则表示不需要进行版本控制的文件。如果新建仓库时是通过已有项目新建的，那么新建仓库时项目目录中的所有文件都被纳入追踪的文件，后续添加的文件则需要手动添加追踪。

![Status of file](https://git-scm.com/book/en/v2/images/lifecycle.png)

已追踪的文件继续细分还可以分为：`未修改(Unmodified)`、 `已修改(Modified)` 和 `已缓存(Staged)`。 状态间的转换条件见上图。

+ 查看当前文件状态**(status)**
#
    $ git status
这个命令可以指出当前工作目录下所有文件的状态。

+ 添加跟踪文件**(add)**
#
	$ git add [文件名]
这个命令可以将你想跟踪(track)的文件添加至跟踪目录，以便文件内容发生修改时可以查询。

如果文件已经被添加至追踪目录，那么这个命令可以被用来在文件内容发生改变时将文件保存至缓存区，以便下次`commit`时保存。

+ 忽略不想追踪的文件

如果当前工作目录下存在未跟踪的文件，Git会持续提醒你。如果想忽略这种提醒，可以将不想追踪的文件添加至一个目录里。  
创建一个名为`.gitignore`的文件，在其中列出要忽略的文件的名称的模式(pattern)。 例如：

	$ cat .gitignore
    *.[oa]
    *~
第一行告诉 Git 忽略所有以 .o 或 .a 结尾的文件。一般这类对象文件和存档文件都是编译过程中出现的。 第二行告诉 Git 忽略所有以波浪符（~）结尾的文件，许多文本编辑软件（比如 Emacs）都用这样的文件名保存副本。 此外，你可能还需要忽略 log，tmp 或者 pid 目录，以及自动生成的文档等等。 要养成一开始就设置好 .gitignore 文件的习惯，以免将来误提交这类无用的文件。

#### 5.查看修改
`git status`能粗略地指出文件的状态，如果想知道具体改变了什么地方，可以使用：

	$ git diff
不带任何参数的`diff`命令会比较尚未暂存(unstaged)和已暂存的(staged)文件的区别。

    $ git diff --cached
    $ git diff --staged

这两个带参数的命令功能一致。用于比较已暂存的和已经commit的文件之间的变更。

#### 6.提交更新*(commit)*

commit命令用于将已经暂存的文件保存至Git Repository中。  
具体命令如下：

    $ git commit
不带任何参数提交方式会启动文本编辑器以输入提交的说明（message）。

    $ git commit -m [message]
带`-m`参数的命令可以将提交信息与命令放在同一行。

    $ git commit -a
带`-a`参数的命令可以跳过`add`步骤，直接commit所有已经跟踪过的所有文件。

#### 7.移除文件
+ 最普通的移除文件的方法是将文件从工作目录中删除，然后commit。但是这样的操作较为繁琐。
+ `git rm`可以一次性完成上述工作。
+ 如果文件已经添加至暂存区，那么需要使用`-f`参数强制删除，确保安全。因为删除之后文件不可恢复。
+ 如果只是想把文件从Git仓库(暂存区)中删除，但是不从磁盘中删除，那么可以使用带`--cached`参数的命令

具体命令如下：

    $ git rm [Filename]
    相当于
    $ rm [Filename]
    $ git add [Filename]
    $ git commit

#### 8.移动文件
移动文件的命令可以用于移动文也可以用于重命名。具体命令如下：

    $ git mv File_From File_To
	相当于
	$ mv File_From File_To
	$ git rm File_From
	$ git add File_To

#### 9.查看提交历史

###### **--不带参数**
    $ git log
不带参数的`git log`命令会列出所有的更新、每个commit的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。最近的更新排在最上面。

###### **-p选项**
	$ git log -p -time
带`-p`选项的命令用来显示每次内容提交的差异，`-time`表示仅显示最近`time`次的commit。
	
###### **--stat选项**
	$ git log --stat  //列出每个commit的简略统计信息。
#

###### **--Pretty 选项**
 这个选项可以指定使用不同于默认格式的方式展示提交历史。 这个选项有一些内建的子选项供你使用。 比如用 `oneline` 将每个提交放在一行显示，查看的提交数很大时非常有用。 另外还有 `short`，`full` 和 `fuller` 可以用，展示的信息或多或少有些不同，请自己动手实践一下看看效果如何。

`--format`可以定制要显示的记录格式。

例子：

	$ git log --pretty=format:"%h - %an, %ar : %s"
	ca82a6d - Scott Chacon, 6 years ago : changed the version number
	085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
	a11bef0 - Scott Chacon, 6 years ago : first commit


| 水果        | 价格    |  数量  |
| --------   | -----:   | :----: |
| 香蕉        | $1      |   5    |
| 苹果        | $1      |   6    |
| 草莓        | $1      |   7    |
