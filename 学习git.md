# 一次代码提交引发的惨案

虽然git用了好些年了，但一直是做网盘用。但使用的时候还是出现问题，而且还不能随意使用。所以重新加有体系的重新学一遍。

## 什么是git

当网盘用也没啥毛病。前身是版本控制器VCS（Version Control System）。后来发展成集中化的版本控制系统CVCS(Centralized Version Control System)。再往后就是诸如git这一类的分布式版本控制系统DVCS(Distributed Version Control System).

## git 原理

本质上是在中央仓库得到初始版本，然后在本地进行修改提交。储存每一次修改的内容。这其中又有各个版本存储的问题，不可能全部存储每次的版本内容，所以是存储每次修改的部分。

剩下的内容都是基于这个原理而发展出来。

## git 基础

虽然用的时候都是跟远程仓库有关，但这个主要的操作都是在本地，哪怕是远程仓库的各个分支的分支出现问题，也是拉取到本地进行修改合并并提交到远程仓库之中的主要分支。所以先看本地的命令。

### git的基本状态

三种状态

1. 已提交(committed)
2. 已修改(modified)
3. 已暂存(staged)

三个工作区域

1. git 仓库
2. 工作目录
3. 暂存区

这里放弃远程仓库的概念。因为远程仓库也只是将本地仓库推送至云服务器之中。 

三个流程

1. 在工作目录中修改文件
2. 暂存文件，将文件的快找放入到暂存区域
3. 提交更新，找到暂存区域的文件，将快找永久存储到git仓库目录

![image-20240401101059899](https://raw.githubusercontent.com/Mirror18/imgage/img/note/202404011011026.png)

### 一次简单的运行git

1. git init
2. git config user.name ""
3. git add .
4. git commit -m ""

下面详细解释

要使用 git ，就要先明白 git 需要什么样的环境。电脑使用微信登陆还需要 微信账号呢。

那对于 git 在本地运行是需要什么呢，就一个，账号。可以随意设置。至于远程仓库中需要的 key 和 云端信息之后再看。现在仅需设置一个用户账号。

因为是命令行操作，所以这里需要明白点其他的东西。例如这个用户信息是放到哪的。不同地方的信息使用的参数不一样。

#### config 信息设置

1. 当时安装文件放的文件夹
2. 用户名下的./gitconfig文件下
3. 所使用的仓库的/.git/config路径下

对比下nux 系统下的三种状态位置

1. /etc/gitconifg
2. ~/.gitconfig
3. 项目文件夹/.git/config

如果有点 nux 基础，就很容易看明白这是这三个目录的优先级

所以也就可以看接下来的命令了

```shell
git config --global user.name "mirror cat"
git config --global user.email "mirror@example.com"
```

这个是设置的全局，就是放到用户名下的config，就第二个状态的位置。

```shell
git config user.name "cat mirror"
git config user.email "cat@emaple.com"
```

这个就是设置到所在项目的config下。



```shell
git config --list
```

列出所有的配置信息

也可以使用键值对

```shell
git config user.name
```

获取到单独的键值信息

---

附加一个不怎么常用的设置。

```shell
git config --global core.editor vim
```

这个是设置文本编辑器。虽然默认是vim。还有一个是emacs。

#### git init 详解

如何让一个文件夹可以被识别到是一个git 工作目录

可以使用 git init 进行初始化。然后配置基于这个项目的config 设置。

当然也可以使用这个命令

```shell
git init [project-name]
```

在当前文件夹下创建一个新的文件夹，并且包含配置项目，虽然，没什么用。

或者说从远程仓库里拉一个玩。里面包含了init的初始信息。

```shell
git clone https or ssh
```

或者

```shell
git clone https or ssh  本地仓库名
```

#### git add 详细

add 这个命令参数有很多种解释。例如添加远程仓库，或者将文件添加到暂存区，或者说将一个文件的状态改为已跟踪。

在这个的流程中。add 有两种解释。

1. 如果是新创建的文件，使用 add 是变成已跟踪，纳入到git 版本控制中
2. 如果说是修改过后的文件，使用之后就是，暂存。可以跟随commit 进行提交。

---

这里插入一个关于前面的解释吧。就三种状态的解释。

已提交，已修改，已暂存。

先 git init，进行初始化之后

这样去实操。

1. 先创建一个 README.md文件，里面写好内容，使用git add README.md 纳入git 控制，然后使用 git commit -m "init"进行提交。

2. 在创建一个 a.md 文件，里面写好内容。使用 git add a.md 被跟踪

3. 在创建一个 b.md 文件。写好内容

4. 这个时候使用 git status  查看当前状态。

   1. ​	![image-20240401113709645](https://raw.githubusercontent.com/Mirror18/imgage/img/note/202404011137709.png)可以很清楚的看出来未跟踪，已暂存。因为命令显示使用那个命令可以退出暂存状态
   2. 已修改。 使用 echo "aaa" >> a.md，但没有git add a.md![image-20240401113949878](https://raw.githubusercontent.com/Mirror18/imgage/img/note/202404011139924.png)然后在看 git status 所表达的属性。显示的就是暂存这上一次的版本，还没有提交。还显示一个a.md已经修改
   3. 如果git add a.md 进行添加到缓存区之后。![image-20240401114655124](https://raw.githubusercontent.com/Mirror18/imgage/img/note/202404011146161.png)就从已修改中去掉。
   4. 使用git commit -m "a.md"。就会发现从git status 中去掉，不显示。

   通过上述的一番操作，就可以明显看出，已暂存，已提交，已修改。都是发生在什么时候。add -> commit -> vim a.md

   1. 已暂存：对当前已经修改的文件做标记，包含到下次提交的快照中
   2. 已提交：表示数据保存到本地数据库中
   3. 已修改：修改文件，但没有保存到数据库中

   三种状态既然了解了，那么自然，三个工作区域就很好理解。

   1. 工作目录，把某个分支，或者说某个版本，独立提取出来的内容
   2. git 仓库，保存项目的元数据，对象数据库的地方
   3. 暂存区，保存了下次要提交的内容

   工作流程

   1. 在工作目录中修改文件
   2. 暂存文件，文件快照放到暂存区域
   3. 提交更新，将暂存区的文件永久存储到git仓库目录

   ---

   一个插曲

   通过上述，可以明白文件有三个版本。工作目录中的版本，暂存文件中的版本。git 仓库中的版本。

   所以这三个版本之中修改了什么内容我该怎么看呢

   1. 已修改版本跟暂存区的对比，通过 git diff 查看
   2. 暂存区的和git 仓库的对比 ，通过 git diff --cached 查看

#### git commit 详解

这个就是提交更新、只不过有几个参数需要注意

1. git commit 会进入到文本编辑中，必须要写提交信息，养成好习惯
2. git commit -m "xxx"则是直接提交，不进入文本编辑。提交信息提供了
3. git commit -a -m "xxx"则是把所有跟踪的，已修改的，进行提交。少了一步git add
4. git commit --amend 会将暂存区中的文件再次重新提交

### 移除文件

文件被跟踪状态，直接进行删除，在状态中会显示被删除，没有提交到暂存区

也就是说这时候可以用 git restore 进行恢复

也可以用  git rm  或者 git add 给删除掉 

根据上述的git diff 命令，其实能发现git rm 是可以有两种状态

1. 删除工作目录和暂存区的文件,git rm
2. 删除暂存区中的文件, git rm --cached

第一种对应全删除，除了git 仓库中还留有数据，但随着下次提交就去掉了

第二种缓存区删除，文件仍留在本地，但不纳入版本管理。这种针对添加多了文件到仓库中

### 恢复文件

这样做的原因是为了可以恢复，第一种情况下使用 git restore --staged 先恢复到暂存区，再使用git restore 恢复到工作目录中

第二种情况跟上述的一样。不过主要就是纳入管理。

### 移动文件

这是个重命名操作。

git mv a.md b.md

上述命令实际上是个缩减

实际执行的是

```shell
mv a.md b.md
git rm a.md
git add b.md
```



### 提交历史

git log

查看日志是一个良好的行为。在git 中 日志就是提交日志。之前的ommit -m "xxx"提交的信息就在这里可以看到。

同时有几个参数挺好用

`git log -p -2`显示最近两次的提交的差异

`git log --stat` 提交的简要统计信息

`git log --pretty=`这个有很多种输出日志的方法



### 回退版本

通过使用，可以明白 commit 提交的内容是最终版本，但是修改出错了，想返回之前的状态，不用删了再重新写。git 把各个版本的信息都记录了下来。

使用 git reset 就可返回之前提交的内容

返回上一个版本 git reset HEAD^

通过使用git log 得到每个的版本信息。然后使用 git reset 0562e就可返回特定版本

也可以使用 git reset HEAD^3返回上上上次版本

--soft 参数是不删除的信息提交

--hard 是删除之前的所有信息提交

##  git 分支

这个才是git 的用处所在

使用git branch branchname创建分支

使用 git checkout branchname 切换分支

或者使用 git checkout -b branchname 创建并切换分支

使用 git branch -d branchname 删除分支

使用 git merge branchname 合并分支

如果这里有冲突会提示在那个文件有冲突，并且冲突内容是什么。修改对应的文件解决完冲突后。使用git add 然后提交，就可以完成了。

## 远程仓库

### 添加远程仓库

使用 git remote add remote_name  remote_url进行添加远程仓库

之后用git remote -v查看远程仓库信息

### 更新本地远程仓库

上述是添加远程仓库

但是如果远程仓库更新了呢

使用 git fetch remote_name 拉取更新内容

再使用 git merge remote_name/remote_fetch合并到当前分支

但是远程仓库的内容毕竟是多个分支的，那么查看远程仓库各个分支下的内容，就可以使用git chekcout remote_name/remote_fetch 进行切换

### 推送到远程仓库

git push remote_name local_fetch 将本地这个分支推送到远程仓库

### 查看远程仓库

git remote show remote_name展示远程仓库下的信息

### 删除远程仓库和重命名远程仓库

重命名 git remote rename remote_name1 remote_name2

删除 git remote rm remote_name

## 远程分支

既然有了远程仓库，可以得到仓库的信息。那么推送的时候怎么设置一个本地分支和远程分支的绑定呢。

先看怎么推送到特定分支上

### git push

git push remote_name local_fetch:remote_fetch

之后就建立了默认链接，将本地分支和远程分支绑定起来。或者说省略，直接git push remote_name local_fetch 会自动绑定到默认分支。之后就可以用git push 来简化推送

版本有差异，可以使用 git push --force remote_name/remote_fetch 来进行强制推送

#### 删除远程分支

git push remote_name --delete remote_fetch

### git pull

git pull remote_name remote_fetch:local_fetch

或者说拉取到当前分支git pull remote_name remote_fetch



### 跟踪远程分支

git checkout -b local_branch rempte_name/remote_branch

在这个新创建的分支上就可以拉取和查看远程分支





