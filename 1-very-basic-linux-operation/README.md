# 第一讲 非常基础的命令行操作

前置知识：无

Copyright 2025 OhanaTyan@GitHub, wold9168@GitHub(Shinonome Tera tanuki)

本讲的主要内容是基础的 Linux 命令行操作。主要涵盖 Linux 的文件、目录操作，以及其他方面的一些常用指令。

对于使用 Windows 操作系统的同学，请阅读第零讲的内容，以获取一个可用的 Linux 终端环境。同时，这些知识在几乎任何的 UNIX 系统上都是通用的，值得任何未来在学习科研生活中接触和使用 UNIX-like 环境的同学进行学习。

我们假设您正在使用 Bash Shell 学习这一章内容。第零讲的懒人镜像同样使用了 Bash Shell 。

有关 Bash Shell 的更多信息，请参阅[Bash的维基百科条目](https://zh.wikipedia.org/wiki/Bash)。

**_本讲的讲义在重写以后加入了很多现场宣讲（2025年6月15日宣讲）中没有的内容，补充了大量的知识背景（不过也可能略显啰唆）。值得一读！_**

---

## 题外话在前：`man`命令的使用和系统手册

使用`man`指令可以阅读系统手册。如您英文水平过关，我们十分推荐通过阅读系统手册的方式学习 Linux 的各指令工具。

懒人镜像中并不默认提供`man`指令，你可以通过`apt update && apt install -y man less`来安装这一工具（以及`less`工具，该工具会改善man的阅读体验）。

`man`指令的最基本用法是：

```bash
man [man options] [[section] page ...] ...
```

- `man options`：`man`指令的参数。视情况使用。
- `section`：指定希望阅读的系统手册的具体章节。比如`man 1 man`就是查看系统手册第一章中`man`指令的条目。在不清楚指令在系统手册具体第几章的时候完全可以不输入。（`man man`就可以查看到`man`指令的条目了）。`man`指令的条目通常被表示为`条目名(章节名)`，比如`man(1)`同样意味着系统手册第一章中`man`指令的条目。
- `page`：输入你希望查看的系统手册的条目。条目一般以对应指令命名，也有以接口命名的。比如`man mmap`用于查看标准C语言库中提供的`mmap()`函数的系统手册。

提示：

- 打开系统手册后，按方向键浏览系统手册，按`q`键退出系统手册，按`h`键获取更多信息。
- 如若不安装`less`，那么`man`会使用`more`作为`man`的默认阅读器。该工具不支持向上翻页。

延伸了解：

- `info`：在终端中显示 Info 文件。Info 文件是一些 Linux 软件所使用的文档系统。`info`指令可以起到与`man`指令类似的作用。`info`指令一般为 Linux 发行版自带。
- `tldr`：tldr 是 too long to read 的缩写，意为“太长不看”（我们接下来也会用这个缩写去标识一些章节中高度提炼的内容）。`tldr`是受此缩写启发的一个速查表程序，快速列出一个命令行指令的常用功能，方便第一次使用该指令的用户快速上手或回忆。访问其[仓库](https://github.com/tldr-pages/tldr)获取更多信息。
- `navi`：一个可交互式的命令行速查表程序，允许用户快速检索常用指令。用户可以载入自己的速查表。
- `--help`、`-h`、`-?`：大部分的命令行程序都支持类似的参数，以显示一个基本的帮助信息。比如`man --help`会显示`man`指令的帮助信息。

## `ls`：查看目录

第一个需要介绍的指令是`ls`。`ls`是`list directory contents`的缩写，意为“列出目录的内容”。Linux 的许多指令的命名规则都是如此。类似的指令还有：

| 指令    | 对应英文全称                            | 用途               |
| ------- | --------------------------------------- | ------------------ |
| `pwd`   | print name of current/working directory | 打印当前目录的地址 |
| `cd`    | change the current directory            | 切换当前目录       |
| `mkdir` | make directories                        | 新建目录           |
| `rmdir` | remove empty directories                | 删除空目录         |

`ls`的基本用法如下：

```bash
ls # 列出当前目录的内容
ls folder # 列出当前目录下 folder 文件夹中的内容
ls /bin # 列出 /usr 这一文件夹中的内容
ls /bin/echo # 列出 /bin/echo 这一文件

ls -a # 列出当前目录下的全部内容，即追加列出以.（半角点号）开头的文件。以.开头的文件或文件夹在 UNIX 系统中通常被认为是隐藏的。
ls -l /usr/bin # 以所谓“长列表格式”列出 /usr/bin 目录下的内容。长列表格式包含了目录中每个文件的更多相关信息。我们会在稍后介绍长列表格式的阅读方式。
ls -lh /usr/bin # -h 参数是在打印文件大小的时候将文件大小转换成人类可读的容量大小。我们已经可以注意到这行指令输出中的一列比起上一行指令的输出多出了K、M等字样，该列代表对应文件的大小。这里的K指的是KB，M是MB。
ls -al # 结合 -a 和 -l 的效果。短横线加单个字母的形式被称为短参数，也被称为“UNIX风格的参数”。
# 短参数允许你在单个短横线后面进行拼接，像是 -a 和 -l 被结合成了 -al （ -la 也不是不行）；短参数也可以拆开使用，像下一行演示的这样。
ls -a -l
```

### 阅读长列表格式的`ls`指令输出（选学）

以下给出了两个`ls -l`指令的输出：

```bash
root@ee8e51e37746:/# ls -l /dev
total 0
crw--w----. 1 root tty  136, 0 Jun 24 18:32 console
lrwxrwxrwx. 1 root root     11 Jun 24 18:32 core -> /proc/kcore
lrwxrwxrwx. 1 root root     13 Jun 24 18:32 fd -> /proc/self/fd
crw-rw-rw-. 1 root root   1, 7 Jun 24 18:32 full
drwxrwxrwt. 2 root root     40 Jun 24 18:32 mqueue
crw-rw-rw-. 1 root root   1, 3 Jun 24 18:32 null
lrwxrwxrwx. 1 root root      8 Jun 24 18:32 ptmx -> pts/ptmx
drwxr-xr-x. 2 root root      0 Jun 24 18:32 pts
crw-rw-rw-. 1 root root   1, 8 Jun 24 18:32 random
drwxrwxrwt. 2 root root     40 Jun 24 18:32 shm
lrwxrwxrwx. 1 root root     15 Jun 24 18:32 stderr -> /proc/self/fd/2
lrwxrwxrwx. 1 root root     15 Jun 24 18:32 stdin -> /proc/self/fd/0
lrwxrwxrwx. 1 root root     15 Jun 24 18:32 stdout -> /proc/self/fd/1
crw-rw-rw-. 1 root root   5, 0 Jun 24 18:42 tty
crw-rw-rw-. 1 root root   1, 9 Jun 24 18:32 urandom
crw-rw-rw-. 1 root root   1, 5 Jun 24 18:32 zero
root@ee8e51e37746:/# ls -l /usr/lib
total 4
drwxr-xr-x. 1 root root   98 Jun 10 00:00 apt
drwxr-xr-x. 1 root root   30 Jun 24 18:32 binfmt.d
drwxr-xr-x. 1 root root   14 May 25  2023 dpkg
drwxr-xr-x. 1 root root   26 Jun 24 18:35 groff
drwxr-xr-x. 1 root root   40 Jun 10 00:00 init
drwxr-xr-x. 1 root root   12 Mar  6 22:46 locale
drwxr-xr-x. 1 root root   60 Jun 10 00:00 lsb
drwxr-xr-x. 1 root root  124 Jun 24 18:35 man-db
drwxr-xr-x. 1 root root   16 Jan 20  2024 mime
-rw-r--r--. 1 root root  267 May  9 14:50 os-release
drwxr-xr-x. 1 root root   26 Jun 24 18:32 python3
drwxr-xr-x. 1 root root 4108 Jun 24 18:32 python3.11
drwxr-xr-x. 1 root root   70 Jun 24 18:32 ssl
drwxr-xr-x. 1 root root   12 Sep 21  2023 systemd
drwxr-xr-x. 1 root root   28 May  7  2023 terminfo
drwxr-xr-x. 1 root root   22 Jun 24 18:35 tmpfiles.d
drwxr-xr-x. 1 root root   36 Jun 10 00:00 udev
drwxr-xr-x. 1 root root   24 Jun 24 18:32 valgrind
drwxr-xr-x. 1 root root  200 Jun 24 18:35 x86_64-linux-gnu
```

第一列形同`drwxr-xr-x.`的部分描述了该项的类型和权限。Linux 权限管理的部分已经超出了本讲的话题。该列中的`d`表明该项是一个目录，`c`说明这是一个字符型设备，`b`说明这是一个块设备，`-`说明这是一个文件，`l`说明这是一个符号链接（可以类比成 Windows 里的快捷方式）。

第二列是该项的硬链接总数。硬链接和符号链接（也叫软链接）的部分我们日后会介绍。

第三列和第四列分裂标记了该项的文件属主和文件属组。他们对应这个文件的所有者和对这个文件具有权限的用户组。上面所有的项，其属主都是`root`。这个用户是 Linux 系统里的根用户，具有最高权限。而属组中的`root`则是与`root`用户同名的用户组。用户与组的部分同样超出了本讲的话题，阅读[ArchWiki这篇绝妙的文章](https://wiki.archlinuxcn.org/wiki/用户和用户组)以获取更多信息。

第五列就是文件的大小，单位是字节（Byte）。我们已经提到过，使用`-h`参数可以使得这一列数字的单位向上换算为KB、MB这些更容易阅读的单位。

第六列、第七列、第八列形同`Jun 24 18:32`的部分是文件的上次修改时间。我们稍晚会提到一个名为`stat`的指令，该指令能查询一个文件的最后访问时间、上次修改时间、最后元数据变更时间和文件创建时间，以及其他一些有用的信息。

第九列显然就是该项的名字。

## `cd`和`pwd`：切换目录和显示当前所在目录

我们前面提到过，`ls`是显示当前目录的内容。`cd`指令就是被用于切换当前目录的。

```bash
cd # 切换到家目录。我们稍后会在“Linux文件系统速览”一节中提及家目录的概念。

cd /usr # 切换到 /usr 目录
ls # 显示当前目录的内容。当前是 /usr 目录，所以这条指令的效果和 ls /usr 是一样的。
```

如何判断我们当前所在目录呢？我们可以用`pwd`指令。`pwd`指令输出用户当前所在目录。

```bash
root@ee8e51e37746:/usr# pwd
/usr
```

当前目录有时候也称为工作目录。

### 观察我们的命令行：Command prompt

以此为例：

```bash
root@ee8e51e37746:/usr# pwd
/usr
```

我们发现，我们输入指令的地方是一个井号和一个空格（`# `）之后开始的。在这个`# `之前，还有一段信息`root@ee8e51e37746:/usr`。这整段内容我们称之为 Command prompt，也称为“命令提示符”。Windows 中就有一个名为`cmd`的终端程序，其中文名也是“命令提示符”。

命令提示符通常以`#`、`%`、`$`之类的字符收尾，并且附带有一些有用的信息。

`root@ee8e51e37746:/usr`中包含的信息如下：

- `root`：当前登录的用户名
- `ee8e51e37746`：当前的设备名。由于我们的懒人环境是在容器中构建的，所以这个设备名是一串随机的字符。
- `/usr`：当前所处目录。当当前所处目录过长的时候，一些 Shell 应用可能以缩略形式显示这个路径。`pwd`可以打印当前所处目录的完整路径。

## 题外话：`Tab`键的妙用

`Tab`（⇥）键位于键盘`CapsLock`/`Caps`键的上方、`Q`键的左边，名为制表符，一般用于对其输出。在命令行中，`Tab`键可以用来智能补全。

在你的终端中输入`cd /us`，然后按下`tab`键看看？

终端中的输入应该变成了`cd /usr/`，对吧！

`Tab`键可以自动补全终端中未输入完毕的内容，帮助用户减少输入的麻烦，同时也可以提高输入的正确率。

一些 Shell 应用同样支持通过`Tab`键补全参数。

```bash
$ tar⇥
A  -- append to an archive
c  -- create a new archive
t  -- list archive contents
u  -- update archive
x  -- extract files from an archive
v  -- verbose output
f  -- specify archive file or device
```

## Linux 文件系统速览

在进一步了解其他的指令以前，我们需要先学习一点有关于 Linux 文件系统的知识。

这一部分内容可以先跳过，阅读了后面的内容再回看。

### 根目录

与 Windows 不同，Linux 中没有C盘、D盘这样的概念。Linux 中文件被组织成一个树状结构，最顶级的目录是根目录`/`。

所有的文件都可以在`/`以及其子文件夹下找到。

### 相对路径和绝对路径

绝对路径就是从根目录开始完整地写出的路径，比如`/usr/bin/ls`。

相对路径就是不是从根目录完整地写出的路径。比如`./bin`，就是当前目录下的`bin`文件夹（或者文件）。

在 Linux 中，单个点号`.`表示当前目录，两个点号`..`表示上一级目录。`ls -a`对此亦有显示。

比如`cd ..`，这个指令的做法就是切换到上一级目录。

`.`和`..`的使用很自由。像是`ls /usr/../usr/bin/ls`这样的写法也是OK的。`/usr/..`就是根目录`/`吗。

### 家目录

前面我们提到，不带任何参数的`cd`可以切换到家目录。

家目录一般是在`/home`文件夹下的一系列文件夹，可以类比为 Windows 中的用户文件夹，像是`C:\Windows\Users\foo`。一些用户没有家目录，在创建新用户的时候也可以指定新用户的家目录为任何一个其他目录。

`root`用户比较特殊，`root`用户的家目录是`/root`，所以`root`用户输入`cd`的时候是直接跳转到`/root`目录的。

```bash
root@ee8e51e37746:/# cd
root@ee8e51e37746:~# pwd
/root
```

波浪号`~`用于表示用户的家目录。`cd ~`和`cd`是等价的。

家目录用于给用户存放用户自己的文件。普通用户应该尽量避免操作家目录之外的文件。

### 约定俗成：FHS（Filesystem Hierarchy Standard，文件系统层次结构标准）

这一部分内容是选学。

FHS 是 Linux 的文件系统层次结构标准。这是一个非强制的标准。多数 Linux 发行版遵从 FHS 标准并且声明其自身政策（文件分布的方案）以维护 FHS 的要求。对于普通用户，我们只需要了解FHS非常有限的一些内容就行。

| 目录         | 描述                                                                                                              |
| ------------ | ----------------------------------------------------------------------------------------------------------------- |
| `/`          | 根目录                                                                                                            |
| `/bin`       | 需要在单用户模式下可以的必要命令的可执行文件；面向所有用户，例如`cat`、`ls`、`cp`                                 |
| `/sbin`      | 必要的系统级的可执行文件；面向管理员用户（比如`root`。                                                            |
| `/lib`       | `/bin`和`/sbin`中可执行文件所需要的库库                                                                           |
| `/tmp`       | 临时文件。系统重启的时候通常会清空这个文件夹。你可以在这个文件夹里瞎搞                                            |
| `/etc`       | 系统范围内的配置文件。FHS 限制`/etc`存放静态配置文件，不能包含二进制文件。可以类比为 Windows 的注册表             |
| `/usr`       | UNIX系统资源（Unix System Resources），包含大部分的（多）用户工具和应用程序                                       |
| `/usr/bin`   | 非必要命令可执行文件（单用户模式下非必需）；面向所有用户。一些 Linux 发行版喜欢把`/bin`置为`/usr/bin`的符号链接。 |
| `/usr/sbin`  | 非必要系统级的可执行文件；面向管理员用户。一些 Linux 发行版喜欢把`/sbin`置为`/usr/sbin`的符号链接。               |
| `/usr/lib`   | `/usr/bin`和`/usr/sbin`中可执行文件所需要的库。一些 Linux 发行版喜欢把`/lib`置为`/usr/lib`的符号链接。            |
| `/usr/local` | 本地数据的第三级层次结构，特定于本机。通常有类似于`/usr`中的结构，有`bin`、`lib`等文件夹。                        |

#### 扩展了解：`/bin`、`/usr/bin`和`/usr/local/bin`

这里可以提一个许多老 Linux 用户都了解有限的设计，就是`/bin`、`/usr/bin`和`/usr/local/bin`这三个目录的区别。`bin`是二进制的意思，在 FHS 中，`bin`目录专门用于存储普通用户也可以使用的可执行文件（这些可执行文件都是二进制（binary）文件，`bin`目录遂得名）。我们已经接`/bin`、`/usr/bin`和`/usr/local/bin`触过里面的文件了，像是`ls`。

`/bin`目录里一般存放系统必须的可执行文件。这些可执行文件在系统救援等场景下是必须的。在其他文件系统没有挂载入系统的时候，`/bin`下的工具仍需要保持可用（像是单用户模式）。在该模式下，系统仅启动最基础的服务，并且通常只允许`root`用户登录。

`/usr/bin`中的内容就稍微会丰富一点，一般是`/usr`的超集，也有一些发行版直接规定`/bin`就是`/usr/bin`的符号链接——使得这俩文件夹下的内容是一样的。系统包管理器（比如`apt`，我们之后会介绍）安装的新软件一般会安装在这里。

`/usr/local/bin`中的目录就是所谓“特定于本机”的内容，从`local`的命名上也可见一斑。比如系统管理员自己搜罗来的二进制文件，或者系统管理员从源码安装的软件，基本都会被存放在这个目录下。一般而言，使用源码安装软件并且希望这个软件对所有用户可用，那么软件就安装到这个目录。许多软件通过源码安装时，会要求用户运行其`make install`指令，这一指令会执行这一软件自定义的安装操作，通常此安装操作会将这一软件安装到`/usr/local/bin`目录而非`/usr/bin`目录，以免和系统的包管理器发生冲突。

其实还有一个`~/.local/bin`目录，一些发行版不会主动创建这个目录，甚至不会把这个目录添加到`PATH`环境变量中（我们以后会介绍`PATH`环境变量）。约定俗成地，用户自己安装并且不希望其他用户使用的软件会被放入到这个目录中。

顺带一提，软件除了其可执行文件及其配置文件以外还有许多其他的文件，比如软件图标、软件的`man`文档之类的东西。这些东西一般放在软件所在对应`bin`目录的`../share`目录下（`bin`目录同级的`share`目录下）。`share`目录下的东西就五花八门许多。

### 约定俗成：以`/`结尾是目录

约定俗成的，以`/`结尾表示目录。像是我们前面展示`Tab`键的用法的时候，`cd /us`被补全成`cd /usr/`。这是为了在交流时清晰区分文件和目录。

Linux 中无法在同一路径下存在同名的目录和文件。所以我们日常使用的时候不以`/`结尾也不会造成 Linux 的困惑。和别人交流的时候注意一点就好。

### 扩展了解：如何在路径中表示空格？转义字符和引号

我们想到，空格在命令行中是一种用于间隔的符号。像是`ls`指令，`ls /usr /lib`就会同时列出`/usr`和`/lib`目录下的内容。

那假如我们有一个文件夹，其路径为`/usr /lib`怎么办？这个`/usr `是一个命名以空格结尾的文件夹。

答案是使用转义字符。转义字符中`\ `（一个反斜杠加一个空格）被用于表示空格。`cd /usr\ /lib`就可以轻松解决这一问题。

再就是可以使用引号。引号中的空格会被保留，不作为参数之间的间隔符。`cd "/usr /lib"`和`cd '/usr /lib'`也是可行的。单引号和双引号有一些微妙的区别，这超出了我们的主题。

在路径中使用引号怎么办呢？还是通过转义字符。像是一个文件夹的路径为`/usr"/lib`，就可以通过`cd /usr\"/lib`来切换到这一文件夹。

阅读[转义字符的维基百科页面](https://zh.wikipedia.org/zh-cn/转义字符)获取更多信息。

## 重命名（移动）`mv`、复制`cp`、删除`rm`

```bash
touch /tmp/foo # 创建文件 /tmp/foo，touch 这一指令下文会进一步介绍
mv /tmp/foo /tmp/bar # 将文件 /tmp/foo 移动为 /tmp/bar，也可以认为这就是对 /tmp/foo 进行重命名
cp /tmp/bar /tmp/bar2 # 文件 /tmp/bar2 和文件 /tmp/bar 的内容是一样的。使用 cp 进行文件复制并不会访问系统剪贴板
rm /tmp/bar # 删除了 /tmp/bar 这个文件
```

### 题外话：Linux 的剪贴板

TL;DR: Linux 不自带剪贴板。`mv`和`cp`并不会改变 Linux 的剪贴板。

剪贴板的实现是用户空间主导的。主要由用户空间的工具（如X11/Wayland协议、桌面环境或终端模拟器）实现，而非内核直接提供。

参见[FreeDesktop.org 的这篇文章](https://specifications.freedesktop.org/clipboard-spec/latest/)，由于历史原因，X 客户端并没有一种一以贯之的方式去运作剪贴板。这使得一些用户认为 X 甚至没有剪贴板。但这一说法并不准确。X 中其实有剪贴板，并在ICCCM中进行了标准化。然而许多应用程序并不遵循这种标准。

参见[ArchWiki 的剪贴板词条]：

> 在X10（1985），引入了剪切缓冲区。这些是有限的缓冲区，用于存储任意文本，并被大多数应用程序使用。然而，它们效率低下，且实现方式各异，因此引入了选择功能。剪切缓冲区现在已经被长期弃用，尽管一些应用程序（如xterm）可能仍然有对它们的遗留支持，但不建议使用它们。

笔者个人不是 Linux 桌面开发的专家。笔者对 Linux 剪贴板的认识也有限。一般而言，认为 Linux 就是不带剪贴板的就行。然而 Linux 可以通过一些社区开发的软件去实现剪贴板的存在——也就是所谓“自己装一个剪贴板”。这些剪贴板软件彼此之间应该有某种通信行为，使得他们彼此兼容。大部分情况下，用户只需要安装自己桌面对应的剪贴板管理器就行。`KDE`用户装`Kilpper`，`GNOME`用户装`GPaste`，这就够用了。

总之内核应该是没有实现剪贴板这一功能的。

因而`mv`和`cp`，虽经常被类比为 Windows 的“剪贴”（cut）和“复制并粘贴”（copy & paste），但其并不会改变 Linux 的剪贴板。这些基础工具自其设计的时候应该就没有考虑过和各种剪贴板的 integration。

### 题外话：不要执行`rm -rf /`，以及多用回收站

TL;DR: 不要执行`rm -rf /`，这是一个梗，老用户经常拿来逗人玩儿。平时用设备的时候多用`trash`少用`rm -r`。

`rm -rf /`是 Linux 经久不衰的梗。这行指令会删除根目录下的所有内容，其效果相当于在 Windows 中格式化C盘。经常被老用户用于调侃新用户，也不乏新用户由于对`rm`指令缺乏了解，真的执行了这一行指令。

`-r`参数表明这是递归删除（我们会在之后介绍这个参数）；`-f`参数是强制执行的意思。`rm -rf`会不带任何警告地递归删除一个目录下的全部内容，强大且易用。

在任何条件任何环境下都应该审慎使用`-rf`参数！如若真的对`-r`参数有需求，那也尽量不要加`-f`参数。

普通用户在没有进行系统维护的情况下，若安装了`trash-cli`，可以优先考虑使用`trash`指令代替`rm`指令。这一指令的用法比`rm`更简单，并且提供了一个好用的回收站，以防用户的误操作。阅读`trash(1)`获取更多信息。

## 创建目录`mkdir`和删除目录`rmdir`，以及使用`rm`删除目录

```bash
mkdir /tmp/dir_a/ # 在 /tmp/ 下，创建目录 /tmp/dir_a/
mkdir /tmp/dir_b/dir_c/ # 尝试在 /tmp/dir_b/ 下创建 dir_c/ ，但应该会失败。因为我们此时还没有创建 /tmp/dir_b/
mkdir -p /tmp/dir_b/dir_c/ # 使用 -p 参数，可以使得在创建目录时，如若父目录不存在，则创建之。在这里，-p 参数使得 /tmp/dir_b/ 被自动创建


rmdir /tmp/dir_a/ # 删除了目录 /tmp/dir_a/
rmdir /tmp/dir_b/ # 执行失败，因为`rmdir`只能删除空目录，而 /tmp/dir_b 非空——其下还有一个目录 dir_c
rmdir /tmp/dir_b/dir_c/ # 删除了目录 /tmp/dir_b/dir_c/

mkdir -p /tmp/dir_b/dir_c/dir_d/ # 创建目录 /tmp/dir_b/dir_c/dir_d/
rm /tmp/dir_b/dir_c/dir_d/ # 执行失败，提示 /tmp/dir_b/dir_c/dir_d/ 是一个目录。因为不带参数的 rm 无法删除任何目录。
rm -r /tmp/dir_b/dir_c/ # 删除了目录 /tmp/dir_b/dir_c/，同时也“递归”删除了该目录下所有的内容。-r 参数允许 rm 删除目录以及目录下的任何内容。
```

## 文件大小`du`和设备大小`df`

```bash
cd /usr
du # 列出当前目录下所有文件夹的大小，单位是字节
du -a # 列出当前目录下所有文件和文件夹的大小
du -d2 # 列出当前目录下所有文件夹及其向下两级文件夹的大小
du -h # 在打印文件大小的时候将文件大小转换成人类可读的容量大小，和 ls -h 类似
du /tmp/foo # 列出 /tmp/foo 目录下所有文件夹的大小

df # 列出当前设备所有文件系统的空间使用情况，单位是字节。懒人环境中，这个指令的输出可能比较复杂，这是正常的。
df -h # 在打印文件大小的时候将文件大小转换成人类可读的容量大小
```

以下讲解设备大小`df`的输出格式：

```bash
❯ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3       742G  651G   89G  89% /
devtmpfs        4.0M     0  4.0M   0% /dev
tmpfs           119G  447M  118G   1% /dev/shm
tmpfs            48G  3.4M   48G   1% /run
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-network-generator.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-udev-load-credentials.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-sysctl.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-tmpfiles-setup-dev-early.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-tmpfiles-setup-dev.service
/dev/sda3       742G  651G   89G  89% /home
tmpfs           119G   94M  118G   1% /tmp
/dev/loop0      105M  105M     0 100% /var/lib/snapd/snap/core/17200
/dev/nvme0n1p1  477G  362G  115G  76% /mnt/nvme0n1p1
/dev/loop1       12M   12M     0 100% /var/lib/snapd/snap/unixbench/26
/dev/loop2       45M   45M     0 100% /var/lib/snapd/snap/snapd/23545
/dev/sda2       3.9G  489M  3.2G  14% /boot
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-vconsole-setup.service
tank             11T  3.4T  7.5T  32% /tank
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-tmpfiles-setup.service
tmpfs           1.0M     0  1.0M   0% /run/credentials/systemd-resolved.service
tmpfs            24G   11M   24G   1% /run/user/1000
```

- `Filesystem`：文件系统对应的设备（像是`/dev/sda2`）或者文件系统的类型。
- `Size`：文件系统的大小
- `Used`：已经使用的空间大小
- `Avail`：未使用的空间大小（可用的空间大小）
- `Use%`：已用的空间大小占总空间的大小
- `Mounted on`：文件系统被挂载在系统中的路径

挂载的概念比较复杂，超出了本讲的主题。

## 查看文件内容：`cat`

`cat`用于查看文件的内容，并将文件的内容打印到屏幕上。

如果直接在终端中输入`cat`，终端会变成“复读机”，在你回车后直接输出你输入的内容。此时且按`Ctrl-C`退出`cat`。

以下以`/etc/os-release`文件为例。该文件被包含在一些 Linux 发行版中，被用来区分发行版以及发行版版本。

```bash
root@ee8e51e37746:/etc# cat os-release
PRETTY_NAME="Debian GNU/Linux 12 (bookworm)"
NAME="Debian GNU/Linux"
VERSION_ID="12"
VERSION="12 (bookworm)"
VERSION_CODENAME=bookworm
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

## 比较文件差异：`diff`

`diff`指令用于比较文件差异。

```bash
root@ee8e51e37746:/tmp# cat a
1111
222
root@ee8e51e37746:/tmp# cat b
1111
33333
44
root@ee8e51e37746:/tmp# diff a b
2c2,3
< 222
---
> 33333
> 44
```

`<`表示左边的文件（`/tmp/a`）有内容`222`，而`>`表示右边的文件（`/tmp/b`）有内容`33333`和`44`。

`2c2,3`由三个部分组成：

- 前面的`2`，表示`/tmp/a`的第2行有变化
- 中间的`c`表示变动的模式是内容改变（change），其他模式还有“增加”（a，代表 addition）和“删除”（d，代表 deletion）
- 后面的`2,3`，表示变动后变成`/tmp/b`的第2行和第3行。

`diff`是版本管理的基石之一。阅读[阮一峰的这篇文章](https://ruanyifeng.com/blog/2012/08/how_to_read_diff.html)了解更多信息！

## 懒人专用的一站式文件管理工具：`mc`

TL;DR: 即便前面的知识都没记住都没关系，这里有轮椅工具。

`mc`是一个给 UNIX-like 可视化的终端。其全称为 Midnight Commander，是一种传统（双窗格）文件管理器，支持标准文件操作、虚拟文件系统、外部命令面板化以及用户菜单。

输入`mc`，出现如下界面：

![`mc`的屏幕截图](./imgs/mc-screenshot.png)

该界面支持鼠标直接操作。因而我们可以直接用鼠标与这一界面进行交互。

屏幕的上方被分为了两部分，称为窗格。打开应用以后，这两个窗格会显示你输入`mc`指令时候的工作目录的内容。按`tab`键在窗格之间进行切换。

直接进行键盘输入，键盘输出会出现在窗格下方的中断处。尝试输入`cd`指令看看？可以发现`cd`指令切换工作目录的时候会连同目前使用的窗格的目录一同切换。使用`tab`键在窗格间切换的时候，也会切换下方窗格的工作目录！

中断下方用数字标识了十个功能，分别可以使用`F1`到`F10`键触发，也可以用鼠标点击。

阅读[ArchWiki的这篇文章](https://wiki.archlinuxcn.org/wiki/Midnight_Commander)获取更多信息！

## 扩展了解：其他命令

### 创建文件和更新时间戳：`touch`

`touch`指令可以被用来创建文件或者更新一个文件的访问时间和修改时间。

```bash
root@ee8e51e37746:/tmp# ls -al # 此时可以看见 /tmp 文件夹为空
total 0
drwxrwxrwt. 1 root root  0 Jun 25 14:27 .
drwxr-xr-x. 1 root root 38 Jun 24 18:32 ..
root@ee8e51e37746:/tmp# touch a # 创建文件 /tmp/a
root@ee8e51e37746:/tmp# ls -al # 此时可以看见 /tmp 文件夹中出现了文件 a
total 0
drwxrwxrwt. 1 root root  2 Jun 25 14:28 .
drwxr-xr-x. 1 root root 38 Jun 24 18:32 ..
-rw-r--r--. 1 root root  0 Jun 25 14:28 a
root@ee8e51e37746:/tmp# touch a # 对一个已经存在的文件使用的时候，touch 会更新这个文件的访问时间和修改时间
root@ee8e51e37746:/tmp# ls -al # 观察到文件 /tmp/a 的上次修改时间被修改为 Jun 25 14:29。
total 0
drwxrwxrwt. 1 root root  2 Jun 25 14:28 .
drwxr-xr-x. 1 root root 38 Jun 24 18:32 ..
-rw-r--r--. 1 root root  0 Jun 25 14:29 a
```

### 输出内容：`echo`

```bash
root@ee8e51e37746:/tmp# echo 123
123
```

`echo`指令输出其后跟着的内容。

与`cat`指令不同：`cat`指令是读取文件系统中的文件；`echo`是从终端中读取内容。

### “花里胡哨地”查看文件内容：`head`、`tail`、`more`和`less`

```bash
root@ee8e51e37746:/tmp# echo "1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> 10
> 11
> 12" > /tmp/a
# 引号框定的内容是可以跨行输入的。这里使用了名为“重定向”的技术将 echo 指令的输出输入到 /tmp/a 文件中。
# 我们会在之后的课程中讲解重定向这一技术。
root@ee8e51e37746:/tmp# cat a
1
2
3
4
5
6
7
8
9
10
11
12

# head 显示文件的前10行
root@ee8e51e37746:/tmp# head a
1
2
3
4
5
6
7
8
9
10
# -n 用于指示显示文件的前几行
root@ee8e51e37746:/tmp# head -n 5 a
1
2
3
4
5
# tail 显示文件的后10行
root@ee8e51e37746:/tmp# tail a
3
4
5
6
7
8
9
10
11
12
# -n 的用法和head是一样的
root@ee8e51e37746:/tmp# tail -n 4 a
9
10
11
12

root@ee8e51e37746:/tmp# more a # 打开了一个文本查看器，只能向下翻阅。

root@ee8e51e37746:/tmp# less a # 打开了一个文本查看器，可以向上翻阅，也可以向下翻阅。实际工作中，more 几乎完全被 less 替代。
```

### 查看历史指令：`history`

输入`history`指令可以快速查阅自己输入过的指令。

```bash
root@ee8e51e37746:/tmp# history
    1  ls
    2  ls
    3  ls /usr
    4  ls /bin
    5  ls /bin/echo
    6  ls -a
    7  man ls
# 下略
```

其实通过上下方向键也可以快速翻阅命令行中的指令历史。

按`↑`键可以切换到终端上一条输入的指令，按`↓`则是下一条。

`history`与管道以及`grep`指令相搭配可以产生十分优雅的结果。管道和`grep`指令我们之后的宣讲中会介绍到。这里先给出一个示例用法。

```bash
root@ee8e51e37746:/tmp# history | grep "man" # 查找所有含有 "man" 字样的历史指令，"man" 可以被替换为其他字段
    7  man ls
    9  apt update && apt install -y man
   10  man ls
   11  man ls
   12  man ls
   13  man --version
   14  man ls
   15  man man
   19  man man
   20  man ls
   24  man ls
   47  man ls
   50  man time
   53  man stat
   70  man usr
  107  man xclip
  108  man wlr-data-control
  109  man mv
  113  man mkdir
  115  man rmdir
  178  man diff
  184  man mc
  188  man touch
  247  history | history "man"
  248  history | grep "man"
```

### 查看系统当前状况：`top`

`top`是`procps`软件包的一部分。使用指令`apt update && apt install -y procps`来安装这一软件包。

`procps`工具的数据来源主要是 Linux 的`/proc`虚拟文件系统，它提供了内核和进程的实时信息：

- `/proc/[PID]/`：每个进程的详细信息（如 status 和 cmdline）。
- `/proc/meminfo`：内存使用情况（`free`命令读取此文件以释放内存）。
- `/proc/stat`：CPU 统计信息（`top`和`vmstat`依赖此数据）。

`top`指令可以被用来监控系统状态。其中显示了系统运行时间、平均负载、当前进程数量、CPU使用情况等信息。

`top`指令的输出格式太复杂，建议同学自行学习了解。（提示：大模型可以很好地回答相关问题）

`top`指令的可读性太差，因此后来也有人开发出了`top`的改进版本。像是`htop`和`btop`。

### 清屏：`clear`

`clear`指令被用于清空当前终端屏幕中的内容。

快捷键`Ctrl-L`的效果是一样的。

### 测量指令运行时间：`time`

`time`指令被用于测量指令运行时间，用法是`time <你需要测时的指令>`

```bash
root@ee8e51e37746:/usr# time du -d1
38744   ./bin
0       ./games
0       ./include
162092  ./lib
4       ./lib64
140     ./libexec
4       ./local
6808    ./sbin
155488  ./share
0       ./src
363280  .

real    0m0.087s
user    0m0.015s
sys     0m0.070s
```

其中`real`是该指令运行的实际总耗时，`user`是用户态耗费的 CPU 时间，`sys`是内核态耗费的 CPU 时间。

有关于用户态和内核态的概念，计算机系的同学未来会在《操作系统》的课程中学习到。其他专业的同学暂时不必作理解。更多信息可以阅读[维基百科的核心态条目](https://zh.wikipedia.org/zh-cn/核心态)和[分级保护域](https://zh.wikipedia.org/zh-cn/分级保护域)条目获取更多信息。

## 扩展了解：命令行快捷键的表达方式

由于历史的和文化的原因，命令行软件的快捷键有一些特殊的表达方式。

### Windows 键的称呼

Windows 键是在计算机键盘左下角 Ctrl 和 Alt 键之间的按键，图案是 Microsoft Windows 的视窗徽标。这一按键在 MacOS 下称为 Command 键，在 Linux 下则称为 Super 键。

历史上在 SAIL（Stanford Artificial Intelligence Lab）键盘上得到广泛应用的 Meta 键在现在通常被 Windows 键和 Alt 键替代，即有一些软件标记的 Meta 键可能是 Windows 键，也可能是 Alt 键。具体情况应该视软件实际讨论。（像是 KDE 认定 Meta 键就是 Windows 键，而 Emacs 则将 Alt 键（或者 Esc 键）作为 Meta 键使用。）

阅读[英文维基百科的 Meta key 词条](https://en.wikipedia.org/wiki/Meta_key)获取更多信息。

### Emacs 的快捷键表示

Emacs，是一个具有可扩展性的文本编辑器，历史悠久，使用者以程序员和其他以技术工作为主的计算机用户为主。

Emacs对快捷键的表达方式深刻地影响了一部分命令行用户的快捷键表达方式。在阅读一些互联网上的材料的时候，我们可能会碰见这种表达方式。

其中`C-<chr>`表示按住 Ctrl 键然后按对应的字符。像是`C-c`等价于`Ctrl-C`。

`M-<chr>`则是表示按住 Alt 键再按对应的字符。（在 Emacs 中，实际你也可以用按下并释放 Esc 键的操作来代替按住 Alt 键的操作。不过在 Shell 相关的资料中，`M-<chr>`就是表示用 Alt 键修饰的快捷键。）

组合起来，我们也可以得到`C-M-c`——这个快捷键就是`Ctrl-Alt-C`。

## 扩展了解：参数风格

参数有三种风格。以下以使用`tar`指令解压`./foo.tar`文件的指令为例：

```bash
tar -xf ./foo.tar # Unix 风格的参数，由一个短横线和一个字母组成。字母可以拼接。也被称为短参数。
tar --extract --file ./foo.tar # GNU 风格的参数，由两个长横线和一个单词组成。这里的单词不可以拼接。也被称为长参数。短参数通常是依照长参数（的首字母或者发音）制定的，像是 -f 就是 --file 的首字母，-x 取 --extract 的发音。
tar xf ./foo.tar # BSD 风格的参数，前面不加短横线。有的人喜欢管这种参数叫“动作”。许多 Python 程序喜欢用这种参数。
```

`tar`指令的相关内容超出了本讲的主题。

## 扩展了解：命令行、Shell、终端、终端模拟器的概念辨析

命令行是用户通过文本指令与计算机交互的方式。广义上指所有通过 Shell 输入的命令，狭义上指命令行界面（CLI, Command Line Interface）。

Shell 是一个命令行解释器，负责解析用户输入的命令，与操作系统内核交互。我们可以理解为，Shell 是我们和操作系统交互的门户和媒介。

终端在历史上是用户和计算机进行文本交互的输入输出设备。历史上是物理设备，像是电传打字机 tty。现在由软件模拟，模拟终端的软件称为终端模拟器。

对于实际中的命令行环境。Shell 意味着终端模拟器中运行的解释器，而终端模拟器则是 Shell 的“前端”，决定了 Shell 向用户展现的图形化界面。换言之，指令的格式、语法受到 Shell 的影响，我们在命令行中输入的指令也直接由 Shell 解释和执行。终端模拟器则允许我们更换 Shell 的主题，甚至提供一些特殊特性使得我们可以通过 Shell 输出图片、切换终端前景色后景色等。

Shell 的典例有 **CMD**、**PowerShell**、Bourne shell（**sh**）、Bourne-Again shell（**bash**）、C shell（csh）、Z shell（zsh）、friendly interactive shell（fish）。懒人环境里使用的是`bash`。读者也可以根据自己的需求安装并修改容器命令行的启动指令以更换默认的 Shell。

终端模拟器的典例有 **Windows Terminal**、cmder、xterm、**Xfce4 Terminal**、**Konsole\*\***GNOME Terminal\*\*、Guake、Yakuake、Terminator、WezTerm等。

## 扩展了解：

“一切皆文件”（Everything is a file）是 Unix/Linux 系统设计的核心哲学之一，它表示系统中的几乎所有资源（包括硬件设备、进程、网络连接等）都通过文件系统的抽象接口来访问和管理。这种设计极大地简化了系统操作，提供了一致的交互方式。

我们前面介绍了，`/proc/meminfo`文件中存放的是有关于内存使用情况的信息。

尝试用`cat`指令去输出这个文件中的内容？

```bash
root@ee8e51e37746:/# cat /proc/meminfo
MemTotal:       247546948 kB
MemFree:        156441380 kB
MemAvailable:   224587064 kB
# 下略
```

这个文件中的内容是被操作系统实时更新的，反映了当前设备的内存使用情况。我们可以将系统提供的这个查看内存使用情况的接口当成普通文件来访问和管理，可以把它当作普通文件复制它的内容，也可以直接用将文件输出到终端中的`cat`去查看其中的内容。

Linux 中与硬件设备、套接字等对象的交互也是通过类似的文件操作进行的。通过文件抽象，用户可以用简单的命令行工具完成复杂的系统管理任务。

## 顺带一提：什么是 Linux 发行版？什么又是 UNIX-like？

详见[附录一：Linux 发行版和 Unix-like](./appendix-1-linux-distro-and-unix-like.md)

## 延伸阅读

- [Command prompt的维基百科条目](https://en.wikipedia.org/wiki/Command-line_interface#Command_prompt)
- [文件系统层次结构标准 2.3版本 的原始页面](https://refspecs.linuxfoundation.org/FHS_2.3/fhs-2.3.html)
- [文件系统层次结构标准的维基百科条目](https://zh.wikipedia.org/wiki/文件系统层次结构标准)
- [Midnight Commander 的官网](https://midnight-commander.org/)
