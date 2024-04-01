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



