
# 安装Git

## 1 在Linux上安装 


```python
$ sudo yum install git
```

基于Debian的发行版上，尝试用apt-get：


```python
$ sudo apt-get install git
```

##  2 在Mac上安装 

最简单的方法是安装Xcode COmmand Line Tools 

或者安装git OS X 下载工具网址http://mac.github.com

##  3 在Windows上安装 

官方版本下载安装http://git-scm.com/download/win

# 初次运行Git前的配置

Git 自带一个 git config 的工具来帮助设置控制 Git 外观和行为的配置变量。
1. /etc/gitconfig 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果使用带有 --system 选项的
git config 时，它会从此文件读写配置变量。
2. ~/.gitconfig 或 ~/.config/git/config 文件：只针对当前用户。 可以传递 --global 选项让 Git
读写此文件。
3. 当前使用仓库的 Git 目录中的 config 文件（就是 .git/config）：针对该仓库。

## 1 用户信息

当安装完 Git 应该做的第一件事就是设置你的用户名称与邮件地址。


```python
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

如果使用了 --global 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。
当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 --global 选项的命令来配置。

## 2 检查配置信息


```python
$git config --list
```

# Git 基础

## 1 获取Git仓库

两种取得Git项目仓库的方法。第一种是在现有项目或目录下导入所有文件到Git中；第二种是从一个服务器克隆一个现有的Git仓库。

### 1.1 在现有目录中初始化仓库

如果你打算使用 Git 来对现有的项目进行管理，你只需要进入该项目目录并输入


```python
$ git init
```

该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是
Git 仓库的骨干。 但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪。

如果你是在一个已经存在文件的文件夹（而不是空文件夹）中初始化 Git 仓库来进行版本控制的话，你应该开始
跟踪这些文件并提交。 你可通过 git add 命令来实现对指定文件的跟踪，然后执行 git commit 提交：


```python
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```

### 1.2 克隆现有的仓库

克隆仓库的命令格式是 git clone [url] 。 比如，要克隆 Git 的可链接库 libgit2，可以用下面的命令：


```python
$ git clone https://github.com/libgit2/libgit2
```

这会在当前目录下创建一个名为 “libgit2” 的目录，并在这个目录下初始化一个 .git 文件夹，从远程仓库拉取下所有数据放入 .git 文件夹，然后从中读取最新版本的文件的拷贝。 如果你进入到这个新建的 libgit2 文件夹，你会发现所有的项目文件已经在里面了，准备就绪等待后续的开发和使用。

如果你想在克隆远程仓库的
时候，自定义本地仓库的名字，你可以使用如下命令：


```python
$ git clone https://github.com/libgit2/libgit2 mylibgit
```


      File "<ipython-input-1-faea11178ce9>", line 1
        $ git clone https://github.com/libgit2/libgit2 mylibgit
        ^
    SyntaxError: invalid syntax
    


这将执行与上一个命令相同的操作，不过在本地创建的仓库名字变为 mylibgit。

## 2 记录每次更新到仓库

### 2.1 检查当前文件状态

要查看哪些文件处于什么状态，可以用 git status 命令。

如果在克隆仓库后立即使用此命令，会看到类似这
样的输出：


```python
$ git status
On branch master
nothing to commit, working directory clean
```

这说明你现在的工作目录相当干净。换句话说，所有已跟踪文件在上次提交后都未被更改过。
此外，上面的信息还表明，当前目录下没有出现任何处于未跟踪状态的新文件，否则 Git 会在这里列出来。
最后，该命令还显示了当前所在分支，并告诉你这个分支同远程服务器上对应的分支没有偏离。
现在，分支名是 “master”,这是默认的分支名。

### 2.2 跟踪新文件

使用命令 git add 开始跟踪一个文件。所以，要跟踪 README 文件，运行：


```python
$ git add README
```

### 2.3 暂存已修改文件


```python
$ git add filename
```

### 2.4 状态简览

使用 git status -s 命令或 git status
--short 命令，你将得到一种更为紧凑的格式输出。


```python
$ git status -s
M README
MM Rakefile
A lib/git.rb
M lib/simplegit.rb
?? LICENSE.txt
```


```python
新添加的未跟踪文件前面有 ?? 标记，新添加到暂存区中的文件前面有 A 标记，修改过的文件前面有 M 标记。 你
可能注意到了 M 有两个可以出现的位置，出现在右边的 M 表示该文件被修改了但是还没放入暂存区，出现在靠左
边的 M 表示该文件被修改了并放入了暂存区。
```

### 2.5 忽略文件

一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文
件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以创建一个名为 .gitignore
的文件，列出要忽略的文件模式。 来看一个实际的例子：


```python
$ cat .gitignore
*.[oa]
*~
```

第一行告诉 Git 忽略所有以 .o 或 .a 结尾的文件。一般这类对象文件和存档文件都是编译过程中出现的。 第二
行告诉 Git 忽略所有以波浪符（~）结尾的文件，许多文本编辑软件（比如 Emacs）都用这样的文件名保存副
本。 此外，你可能还需要忽略 log，tmp 或者 pid 目录，以及自动生成的文档等等。 要养成一开始就设置好
.gitignore 文件的习惯，以免将来误提交这类无用的文件。

文件 .gitignore 的格式规范如下：

• 所有空行或者以 ＃ 开头的行都会被 Git 忽略。

• 可以使用标准的 glob 模式匹配。

• 匹配模式可以以（/）开头防止递归。

• 匹配模式可以以（/）结尾指定目录。

• 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。

星号（*）匹配零个或多个任意字符；
[abc] 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；

问号（?）只匹配一个任意字符；

如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配
（比如 [0-9] 表示匹配所有 0 到 9 的数字）。 

使用两个星号（*) 表示匹配任意中间目录，比如`a/**/z` 可以匹配 a/z, a/b/z 或 `a/b/c/z`等。

### 2.6 查看已暂存和未暂存的修改

如果 git status 命令的输出对于你来说过于模糊，你想知道具体修改了什么地方，可以用 git diff 命令。

若要查看已暂存的将要添加到下次提交里的内容，可以用 git diff --cached 命令。

### 2.7 提交更新


```python
$ git commit
```

在此之前，请一定要确认还有什么修改过的或新建的文件还没有
git add 过，否则提交的时候不会记录这些还没暂存起来的变化。 这些修改过的文件只保留在本地磁盘。 所
以，每次准备提交前，先用 git status 看下，是不是都已暂存起来了， 然后再运行提交命令 git
commit：

可以在 commit 命令后添加 -m 选项，将提交信息与命令放在同一行，如下所示：


```python
$ git commit -m "Story 182: Fix benchmarks for speed"
```

请记住，提交时记录的是放在暂存区域的快照。

### 2.8 跳过使用暂存区域

Git 提供了一个跳过使用暂存区域的方式， 只要在提交的时候，给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存
起来一并提交，从而跳过 git add 步骤：


```python
$ git commit -a
```

### 2.9  移除文件

要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。可以用 git rm 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清
单中了。


```python
$ git rm
```

如果删除之前修改过并且已经放到暂存区域的话，则必须要用
强制删除选项 -f（译注：即 force 的首字母）

另外一种情况是，我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录
中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 .gitignore 文件，不小
心把一个很大的日志文件或一堆 .a 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目
的，使用 --cached 选项：


```python
$ git rm --cached README
```

git rm 命令后面可以列出文件或者目录的名字，也可以使用 glob 模式。 比方说：


```python
$ git rm log/\*.log
```

### 2.9 移动文件


```python
$ git mv file_from file_to
```

## 3 查看提交历史

git log 命令

默认不用任何参数的话，git log 会按提交时间列出所有的更新，最近的更新排在最上面。

一个常用的选项是 -p，用来显示每次提交的内容差异。 你也可以加上 -2 来仅显示最近两次提交

如果你想看到每次提交的简略的统计信息，你可以使用 --stat 选项

正如你所看到的，--stat 选项在每次提交的下面列出所有被修改过的文件、有多少文件被修改了以及被修改过
的文件的哪些行被移除或是添加了。 在每次提交的最后还有一个总结。

另外一个常用的选项是 --pretty。 这个选项可以指定使用不同于默认格式的方式展示提交历史。

git log --pretty=format 常用的选项

|选项|说明|
|---|---
%H| 提交对象（commit）的完整哈希字串
%h| 提交对象的简短哈希字串
%T |树对象（tree）的完整哈希字串
%t |树对象的简短哈希字串
%P |父对象（parent）的完整哈希字串
%p |父对象的简短哈希字串
%an |作者（author）的名字
%ae |作者的电子邮件地址
%ad |作者修订日期（可以用 --date= 选项定制格式）
%ar |作者修订日期，按多久以前的方式显示
%cn |提交者（committer）的名字
%ce |提交者的电子邮件地址
%cd |提交日期
%cr |提交日期，按多久以前的方式显示
%s |提交说明

git log 的常用选项

选项|说明
---|---
-p |  按补丁格式显示每个更新之间的差异。
--stat |显示每次更新的文件修改统计信息。
--shortstat |只显示 --stat 中最后的行数修改添加移除统计。
--name-only |仅在提交信息后显示已修改的文件清单。
--name-status |显示新增、修改、删除的文件清单。
--abbrev-commit |仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。
--relative-date |使用较短的相对时间显示（比如，“2 weeks ago”）。
--graph| 显示 ASCII 图形表示的分支合并历史。
--pretty |使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和format（后跟指定格式）

限制输出长度

选项|说明
---|---
-(n)| 仅显示最近的 n 条提交
--since, --after |仅显示指定时间之后的提交。
--until, --before |仅显示指定时间之前的提交。
--author |仅显示指定作者相关的提交。
--committer |仅显示指定提交者相关的提交。
--grep |仅显示含指定关键字的提交
-S |仅显示添加或移除了某个关键字的提交

## 4 撤消操作

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 --amend 选
项的提交命令尝试重新提交：


```python
$ git commit --amend
```


      File "<ipython-input-6-aa995e1aa6b8>", line 1
        $ git commit --amend
        ^
    SyntaxError: invalid syntax
    


### 取消暂存的文件

git reset HEAD <file>... 来取消暂存

### 撤消对文件的修改

git checkout -- [file]

###### 对那个文件做的任何修改都会消失 - 你只是拷贝了另一个文件来覆盖它。 除非你确实清楚不想要那个文件了，否则不要使用这个命令。

## 5 远程仓库的使用

为了能在任意 Git 项目上协作，你需要知道如何管理自己的远程仓库。 远程仓库是指托管在因特网或其他网络中
的你的项目的版本库。 你可以有好几个远程仓库，通常有些仓库对你只读，有些则可以读写。 与他人协作涉及
管理远程仓库以及根据需要推送或拉取数据。 管理远程仓库包括了解如何添加远程仓库、移除无效的远程仓
库、管理不同的远程分支并定义它们是否被跟踪等等。

### 5.1 查看远程仓库

如果想查看你已经配置的远程仓库服务器，可以运行 git remote 命令。 它会列出你指定的每一个远程服务器
的简写。 如果你已经克隆了自己的仓库，那么至少应该能看到 origin - 这是 Git 给你克隆的仓库服务器的默认名
字：


```python
$ git clone https://github.com/schacon/ticgit
Cloning into 'ticgit'...
remote: Reusing existing pack: 1857, done.
remote: Total 1857 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (1857/1857), 374.35 KiB | 268.00 KiB/s, done.
Resolving deltas: 100% (772/772), done.
Checking connectivity... done.
$ cd ticgit
$ git remote
origin
```

你也可以指定选项 -v，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。


```python
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
```

如果你的远程仓库不止一个，该命令会将它们全部列出。 例如，与几个协作者合作的，拥有多个远程仓库的仓
库看起来像下面这样：


```python
$ cd grit
$ git remote -v
bakkdoor https://github.com/bakkdoor/grit (fetch)
bakkdoor https://github.com/bakkdoor/grit (push)
cho45 https://github.com/cho45/grit (fetch)
cho45 https://github.com/cho45/grit (push)
defunkt https://github.com/defunkt/grit (fetch)
defunkt https://github.com/defunkt/grit (push)
koke git://github.com/koke/grit.git (fetch)
koke git://github.com/koke/grit.git (push)
origin git@github.com:mojombo/grit.git (fetch)
origin git@github.com:mojombo/grit.git (push)
```

### 5.2 添加远程仓库

运行 git remote add <shortname> <url> 添加一个新的远程 Git 仓库，同时指定一个你可以轻松引用的简
写：


```python
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin https://github.com/schacon/ticgit (fetch)
origin https://github.com/schacon/ticgit (push)
pb https://github.com/paulboone/ticgit (fetch)
pb https://github.com/paulboone/ticgit (push)
```

### 5.3 从远程仓库中抓取与拉取


```python
$ git fetch [remote-name]
```

这个命令会访问远程仓库，从中拉取所有你还没有的数据。 执行完成后，你将会拥有那个远程仓库中所有分支
的引用，可以随时合并或查看。

如果你使用 clone 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写。 所以，git fetch origin 会抓取克隆（或上一次抓取）后新推送的所有工作。 必须注意 git fetch 命令会将
数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。

运行 git pull 通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

### 5.4 推送到远程仓库

当你想分享你的项目时，必须将其推送到上游。 这个命令很简单： git push [remote-name] [branchname]

当你想要将 master 分支推送到 origin 服务器时（再次说明，克隆时通常会自动帮你设置好那两个
名字），那么运行这个命令就可以将你所做的备份到服务器：


```python
$ git push origin master
```

### 5.5 查看远程仓库

如果想要查看某一个远程仓库的更多信息，可以使用 git remote show [remote-name] 命令。

### 5.6 远程仓库的移除与重命名

如果想要重命名引用的名字可以运行 git remote rename 去修改一个远程仓库的简写名。 例如，想要将 pb重命名为 paul，可以用 git remote rename 这样做：


```python
$ git remote rename pb paul
$ git remote
origin
paul
```


      File "<ipython-input-8-1c554a714100>", line 1
        $ git remote rename pb paul
        ^
    SyntaxError: invalid syntax
    


值得注意的是这同样也会修改你的远程分支名字。 那些过去引用 pb/master 的现在会引用 paul/master。

## 6 打标签

Git 可以给历史中的某一个提交打上标签，以示重要。 比较有代表性的是人
们会使用这个功能来标记发布结点（v1.0 等等）

### 6.1 列出标签

在 Git 中列出已有的标签是非常简单直观的。 只需要输入 git tag

你也可以使用特定的模式查找标签。 例如，Git 自身的源代码仓库包含标签的数量超过 500 个。 如果只对 1.8.5
系列感兴趣，可以运行：


```python
$ git tag -l 'v1.8.5*'
```

### 6.2 创建标签

Git 使用两种主要类型的标签：轻量标签（lightweight）与附注标签（annotated）

一个轻量标签很像一个不会改变的分支 - 它只是一个特定提交的引用。

### 6.3 附注标签

在 Git 中创建一个附注标签是很简单的。 最简单的方式是当你在运行 tag 命令时指定 -a 选项：


```python
$ git tag -a v1.4 -m 'my version 1.4'
$ git tag
```

通过使用 git show 命令可以看到标签信息与对应的提交信息：


```python
$ git show v1.4
```

### 6.4 轻量标签

创建轻量标签，不需要使用 -a、-s 或 -m 选项，只需要提供标签名字：


```python
$ git tag v1.4-lw
```

### 6.5 后期打标签

### 6.6 删除标签

要删除掉你本地仓库上的标签，可以使用命令 git tag -d <tagname>

上述命令并不会从任何远程仓库中移除这个标签，你必须使用 git push <remote>
:refs/tags/<tagname> 来更新你的远程仓库

### 6.7 检出标签

如果你想查看某个标签所指向的文件版本，可以使用 git checkout 命令
