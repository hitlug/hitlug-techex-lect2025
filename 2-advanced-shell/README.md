# 第二讲 命令行高级操作和 Shell 脚本编程

前置知识：无。有操作系统基础、编程语言基础者为佳。第一讲已学有帮助。

Copyright 2025 wold9168@GitHub(Shinonome Tera tanuki)

本讲主要涵盖 Shell 必知必会的高级知识（这里的高级是相对于直接使用 Shell 指令这种基础操作而言的）。

一些例如管道、文件描述符等技巧与系统内核有关，而 Shell 脚本编程的快速掌握又需要读者有编程语言的基础。因而本讲内容上完全需要单开一讲。

---

## 命令行高级操作

### 前置知识：Linux 文件描述符

文件描述符 (fd) 是一个非负整数。当进程打开一个**文件**、**管道**、**网络套接字**或**其他 I/O 资源**时，由 Linux 内核创建并返回给进程。

每个进程都维护有自己的文件描述符列表，并用一个非负整数去索引。进程可以通过文件描述符对一个文件进行读写操作。

进程启动的时候默认创建三个文件描述符：

- `0`：标准输入
- `1`：标准输出
- `2`：标准错误

以 Shell 进程为例，我们的输入即为该 Shell 进程的标准输入。

### 重定向

终端模拟器显示了 Shell 进程的标准输出。

用户可以通过重定向，将 Shell 进程的输出重定向到文件。这是重定向比较基础的一个应用。

```bash
echo "Hello, World!" > ~/hello.txt # 输出一行 Hello, World! 到家目录下。
echo "123123" >> ~/hello.txt # >> 是追加。单个 > 会覆盖文件。
echo "Another Hello" > ~/hello.txt # 输出一行 Hello, World! 到家目录下。注意，上一行指令的输出会被覆盖。
cat < ~/hello.txt # 从文件获取输入也是可行的
```

重定向顾名思义，就是把文件描述符**重新定向**到另一个文件描述符。

我们来阐述上例中重定向做了什么。

```bash
echo "Hello, World!" > ~/hello.txt # echo 指令的标准输出被重定向至 ~/hello.txt 的文件描述符。如若文件不存在，则创建之；否则，则覆盖之。
echo "123123" >> ~/hello.txt # echo 指令的标准输出被重定向至 ~/hello.txt 的文件描述符。如若文件不存在，则创建之；否则，则将 echo 指令的标准输出追加至 ~/hello.txt 文件尾。
echo "Another Hello" > ~/hello.txt # echo 指令的标准输出被重定向至 ~/hello.txt 的文件描述符。如若文件不存在，则创建之；否则，则覆盖之。
cat < ~/hello.txt # 从文件获取输入也是可行的。将文件的内容作为 cat 指令的标准输入。
```

从文件描述符的角度，`echo`指令的本质是将命令行的参数搬运到标准输出这一文件描述符，`cat`则是将文件中的内容搬运到标准输出。通过重定向，本应输出到标准输出的内容被输出到其他文件（描述符），本应从标准输入中读取的输入却从文件中读取。重定向为命令行指令的输入输出提供了一座与文件系统，以及其他一切存在文件描述符的软件系统的桥梁。Linux 是秉持“一切皆文件”理念的操作系统，文件描述符可以在系统管理中起巨大作用，举例如下：

```bash
echo 1 > /sys/bus/pci/devices/<PCI Address>/remove # 这行指令实现的功能是——禁用一个特定 PCI 地址的 PCI 设备。
```

字符串`1`被 echo 指令输出，而 echo 指令的输出又被重定向到`/sys/bus/pci/devices/<PCI Address>/remove`这一文件描述符。正是这一操作，使得 Linux 内核收到移除特定 PCI 设备的信息，进而实现了禁用一个特定 PCI 地址的 PCI 设备的功能。

参阅 bash(1) 的 REDIRECTION 一节以获取更多信息。

顺带一提，网络套接字也是一种文件描述符。这意味着，重定向也可以被用于向网络发送数据。

#### tty 指令和 gdb 的 tty 重定向

`tty`指令的作用是，打印连接到当前终端的标准输入的文件。

比如，打开两个终端。其中一个输入`tty`，假如其输出为`/dev/pts/0`；则另一个终端输入`echo "example" > /dev/pts/0`。我们会发现，输入`tty`的那个终端，其界面中出现`example`字样。这再次验证了《重定向》一节所言。

`gdb`中也有一个`tty`指令，用于重定向`gdb`所调试程序的标准输出到其他终端中。其用法为：`tty <连接到其他终端的标准输入的文件>`

使用这一方法可以避免`gdb`的 TUI 被`gdb`所调试程序的标准输出搞乱。

### Shell 管道

Shell 管道干的活与重定向相近。（Linux 的管道是一个大机制，Shell 管道是这一机制的一个经典应用。）不同的是，管道是将数据从指令搬运到指令；重定向是将数据从指令搬运到文件，或将数据从文件搬运到指令。

Shell 管道通过管道符`|`进行。管道符`|`的作用是，将前一个指令的标准输出传送给后一个指令的标准输入。

```bash
ls | wc -l # 统计当前目录下文件个数
cat system.log | grep "error" # 搜索 system.log 文件中出现 "error" 字段的行
cat data.txt | sort | uniq # 输出 data.txt 文件的内容，并排序和去重
```

### xargs

`tty`指令只暴露出终端程序的标准输出给我们操作。那么有没有办法可以使我们操作终端程序的标准输入呢？——答案是`xargs`指令

`xargs`指令从标准输入读入输入，作为其后指令的参数。

一般的用法是结合管道，将前一个指令的标准输出传送给`xargs`，作为`xargs`的标准输入，进而作为再后一个指令的参数。

举例如下：

```bash
echo file1 file2 file3 | xargs touch # 等价于 touch file1 file2 file3
echo file1 file2 file3 | xargs -i touch {} --no-create # 等价于 touch "file1 file2 file3" --no-create，-i 参数指定标准输入读入内容的占位符为 {}
echo file1 file2 file3 | xargs -I _ touch _ --no-create # 等价于 touch "file1 file2 file3" --no-create，-I 参数用于指定自定义占位符
```

### 小结

![./imgs/img_shell.drawio.png](./imgs/img_shell.drawio.png)

### 扩展了解：`bash`快捷键

`bash`有许多快捷键。以下介绍其默认的 Emacs 风格快捷键方案。

| 快捷键       | 功用                                                                                                 |
| ------------ | ---------------------------------------------------------------------------------------------------- |
| Ctrl+C       | 发送中断，终止终端当前命令；也可用作保存当前终端缓冲区输入于终端历史，以便修改。                     |
| Ctrl+D       | 基本用法是用 EOF 强行终止终端当前命令，与 Ctrl-C 原理不同。zsh中，若终端缓冲区有输入，则作提示之用。 |
| Ctrl+Z       | 挂起当前命令而不终止。                                                                               |
| Ctrl+L       | 清屏。                                                                                               |
| Ctrl+R       | 进入搜索模式。按下后，输入搜索内容。而后再按，向前搜索终端历史。                                     |
| Crtl+J       | 执行当前搜索结果，相当于回车，并结束搜索模式。                                                       |
| Crtl+G       | 置当前搜索结果入终端，并结束搜索模式。                                                               |
| Ctrl+P       | 终端历史中，当前指令的前一条指令。终端缓冲区有输入时，与上方向键行为不同。                           |
| Ctrl+N       | 终端历史中，当前指令的后一条指令。终端缓冲区有输入时，与下方向键行为不同。                           |
| Ctrl+A       | 置光标于行首。                                                                                       |
| Ctrl+E       | 置光标于行尾。                                                                                       |
| Ctrl+W       | 自光标处删至行首。                                                                                   |
| Ctrl+K       | 自光标处删至行尾。                                                                                   |
| Ctrl+←/Alt+B | 向后一个单词。                                                                                       |
| Ctrl+→/Alt+F | 向前一个单词。                                                                                       |

## Shell 编程

### 变量

变量是用于存储数据值的名称。

```bash
example_var="123" # 定义变量
echo $example_var # 使用变量
echo ${example_var} # 使用变量，花括号可选
echo ${undefined_var} # 未定义变量，默认为空，而无报错！这个很重要，属于常有之错中极有危害性的！
readonly ro_var="123" # 只读变量
unset ro_var # 删除变量
my_array=(1 2 3 4) # 数组
echo $0 $1 $2
```

#### 环境变量

```bash
VAR=1
export VAR # 分享变量给子进程，称作环境变量
export another_var=1
```

系统环境变量一般定义在`/etc/profile`和`/etc/profile.d/`下。

用户环境变量一般定义在`~/.bashrc`（或其他 rc 文件下）和`~/.profile`（或其他 profile 文件下）下。

（如 zsh，其 rc 文件为`~/.zshrc`，其 profile 文件为`~/.zprofile`。不同 shell 所用文件不同。）

rc 文件在相应 shell 启动时应用。

#### 特殊变量

- `$0`表示脚本的名称。
- `$1`、`$2`等表示脚本的参数。
- `$#`表示传递给脚本的参数数量。
- `$?`表示上一个命令的退出状态等。

#### 引号之分：`""`和`''`

```bash
root@ee8e51e37746:/# echo "123 \\ \$ 456" # 双引号中发生转义
123 \ $ 456
root@ee8e51e37746:/# echo '123 \\ \$ 456' # 单引号中不发生转义
123 \\ \$ 456
```

### 判断

```bash
# 无 else 格式
if condition
then
    command1
    command2
    ...
    commandN
fi

if condition; then command1;command2;...;commandN; fi

# 有 else 格式
if condition
then
    command1
    command2
    ...
    commandN
else
    commandN+1
    ...
    commandM
fi

# 有 elif 格式
if condition
then
    command1
    command2
    ...
    commandN
elif another_condition
    commandN+1
    ...
    commandM
else
    commandM+1
    ...
    commandP
fi

# case 语句
case value in
pattern1)
    command1
    command2
    ...
    commandN
    ;;
pattern2)
    command1
    command2
    ...
    commandN
    ;;
esac
```

#### 条件表达式 `condition` 的语法

```
test EXPRESSION
# 或
[ EXPRESSION ]  # 方括号与 EXPRESSION 必须用空格隔开
```

此处 EXPRESSION 为条件表达式。格式如下：

| 操作符             | 描述         | 示例                         |
| ------------------ | ------------ | ---------------------------- |
| -e                 | 文件是否存在 | [ -e file.txt ]              |
| -f                 | 是普通文件   | [ -f /path/to/file ]         |
| -d                 | 是目录       | [ -d /path/to/dir ]          |
| -r                 | 可读         | [ -r file.txt ]              |
| -w                 | 可写         | [ -w file.txt ]              |
| -x                 | 可执行       | [ -x script.sh ]             |
| -s                 | 文件大小 > 0 | [ -s logfile ]               |
| -L                 | 是符号链接   | [ -L symlink ]               |
| -z STRING          | 字符串为空   | [ -z "$var" ]                |
| -n STRING          | 字符串非空   | [ -n "$var" ]                |
| STRING1 = STRING2  | 字符串相等   | [ "$var1" = "$var2" ]        |
| STRING1 != STRING2 | 字符串不等   | [ "$var1" != "$var2" ]       |
| -eq                | 等于         | [ "$a" -eq "$b" ]            |
| -ne                | 不等于       | [ "$a" -ne "$b" ]            |
| -gt                | 大于         | [ "$a" -gt "$b" ]            |
| -ge                | 大于或等于   | [ "$a" -ge "$b" ]            |
| -lt                | 小于         | [ "$a" -lt "$b" ]            |
| -le                | 小于或等于   | [ "$a" -le "$b" ]            |
| !                  | 逻辑非       | [ ! -f "$file" ]             |
| -a                 | 逻辑与       | [ "$a" -eq 1 -a "$b" -eq 2 ] |
| -o                 | 逻辑或       | [ "$a" -eq 1 -o "$b" -eq 2 ] |

#### 条件表达式的`[[ ]]`和`(( ))`

`[[ ]]`：

- 支持模式匹配：`[[ "$var" == *.txt ]]`
- 支持正则表达式：`[[ "$var" =~ ^[0-9]+$ ]]`

`(( ))`：

- 专为数值比较设计：`(( a > b ))`
- 支持更复杂的算术表达式

### 循环

#### `for`循环

```bash
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done

for var in item1 item2 ... itemN; do command1; command2… done;
```

#### `while`循环

```bash
while condition
do
    command1
    command2
    ...
    commandN
done
```

#### `continue` 和 `break`

`continue` 跳过当前循环剩下语句。

`break` 终止循环。

### 获取指令结果：`$()`和`` `command` ``

`$(command)` 是 shell 的一种命令替换机制。它允许我们将一个命令的输出作为另一个命令的参数或者赋值给变量。`` `command` `` 与其等价。

将一个命令的输出结果作为另一个命令的参数或变量值。

```bash
echo $(echo 123 456)
# 相当于
echo `echo 123 456`
# 相当于
echo 123 456
```

不同于`xargs`将标准输入（stdin）的数据转换为后续命令的参数。`$()`和`` `command` ``可以用于变量赋值。且并不涉及文件描述符。故不在上文提及。

`xargs`允许拆分多行输入。而`$()`和`` `command` ``不可。

```bash
root@ee8e51e37746:/# echo "123
> 456"|xargs -i echo 1 {}
1 123
1 456
root@ee8e51e37746:/# echo 1 `echo 123"
> 456"`
1 123 456

# > 乃双引号 " 换行后 Shell 自行添的提示符
```

### Shebang 行、`source`执行、`bash`执行和`chmod +x`

直接执行脚本需要`chmod +x`为脚本添加执行权限。或用`bash`执行。

```bash
root@ee8e51e37746:/home# echo "#!/bin/bash">>example.sh # Shebang 行，#! + 路径
root@ee8e51e37746:/home# echo "echo 123">>example.sh
root@ee8e51e37746:/home# chmod +x example.sh
root@ee8e51e37746:/home# ./example.sh # 直接执行脚本
123
root@ee8e51e37746:/home# bash ./example.sh
123
```

用`bash`执行脚本的本质是创建一当前 Shell 的子进程。脚本中的变量、函数、别名等不影响当前 Shell。

当直接执行脚本（`./example.sh`）时，Shebang 行决定了使用哪个解释器。

`source`执行是在当前 Shell 环境中执行，脚本中的变量、函数、别名等会影响当前 Shell。Shebang 行不生效。
