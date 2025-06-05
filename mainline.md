# 大纲

本文件指定本期技术宣讲的主题，作为本期宣讲各场次内容的大纲使用。

同时本文件陈列了一些需要在本期技术宣讲中不得不统一的内容和标准，以避免重复性工作和无效工作。

## 本期宣讲的主题

本期宣讲的主题是Linux基础。

在日常的科研与学习生活中，我们常常需要接触和使用Linux系统。然而，大学里的计算机课程通常更侧重于专业知识和理论的教学，像Linux这样的实用工具，往往需要学生自行探索和学习。

从求学阶段到未来的职业生涯，这些工具将与学生们朝夕相伴。深入理解理论固然重要，掌握高效的工具使用技能同样关键。本次宣讲中，我们将介绍Linux的基础操作，并讲解其上常见的任务与工作负载。

## 本期宣讲面向的听众

我们假设本次宣讲的听众：

- 受过基本的计算机技能训练，能够正确使用键盘、鼠标和互联网
- 能够结合翻译工具浏览英文页面
- 理解基本的计算机术语，并且能够自发且有效地检索并学习自己不认识的术语
- 能够使用搜索工具在互联网中有效地检索信息，并加以辨别

作为对上述要求的检验，听众可以尝试在宣讲前自行在其设备上安装Docker并且保证docker指令（或者其替代品）在他们当前的命令行中可用。

Docker Desktop在Windows上的的官方安装教程是：[Install Docker Desktop on Windows](https://docs.docker.com/desktop/setup/install/windows-install/)。

安装了Docker Desktop以后，可以在命令行中输入docker指令进行验证。

可以通过docker pull debian:trixie来验证你的安装。

我们同样撰写了一篇生动的教程来节省您检索信息的时间，详情请见[第零讲 环境配置和守则](./0-environment-and-rules/README.md)。

## 内容划分

内容划分仅适用于阐明本期宣讲应该涵盖的内容。

内容划分并不等同于场次划分，一场宣讲可以涵盖一块或多块内容。宣讲者应该对内容的介绍作删节和选择，以及提出自己的内容。这里的内容划分仅适用于理想情况。

**内容划分不暗示内容的前后顺序，宣讲者应该根据自己选择的宣讲内容自行安排内容的组织结构。**

使用全角星号“＊”标注的部分意为高级内容，适合高级用户延伸自习。宣讲者可以酌情考虑将相应内容加入宣讲。

0. 环境配置和守则
   1. 最开始的经验之谈：用户必须对其行为负责，因而用户必须清楚他要做什么
   2. 延伸阅读材料
   3. 如何选择Linux发行版？（＊）
   4. 安装Docker Desktop和懒人包的使用
   5. 使用虚拟机安装Linux（＊）
   6. 使用Docker安装Linux（＊）
      1. docker pull
      2. docker run
      3. docker exec
      4. docker cp
      5. docker stop
      6. docker rm
      7. 其他指令
         1. docker ps -a
      8. docker save和docker load：镜像存取
      9. docker export和docker import：容器存取
1. Linux概念介绍与环境安装
   1. 什么是Linux内核和Linux发行版？
   2. 在Linux上安装软件
      1. apt/yum/pacman：系统级包管理器
      2. homebrew：作为系统级包管理器的补充
      3. flatpak：用户应用程序
   3. 翻阅发行版文档和系统手册manual
      1. 如何查找发行版文档
      2. man：系统手册
2. 初识Shell与命令行文件操作
   1. 启动Shell
      1. 本地启动终端模拟器
      2. ssh：远程登录Shell服务器（＊，但是必讲）
   2. 初识Linux文件系统
      1. 与Windows目录树的区别
      2. 大小写严格
      3. 绝对路径和相对路径
      4. Filesystem Hierarchy Standard (FHS)（＊）
   3. 命令行常识
      1. 快捷键的表示
         1. ^：Ctrl
         2. Emacs表示法：C-/S-/M-
      2. TAB智能补全和方向键检索历史
      3. Shell提示符：显示当前用户和所在路径
      4. 终端和终端模拟器的区别（＊）
      5. 使用man命令翻阅手册，使用--help或者-h参数查阅帮助
      6. 使用C-C和C-D
         1. 根据终端模拟器：使用C-S-C和C-S-V代替粘贴
      7. C-Z：挂起终端（＊）
      8. 什么是环境变量？
         1. 环境变量持久化（＊）
   4. 命令行文件操作和基本指令
      1. echo：开胃菜
      2. cd、ls和tree
      3. mv和cp
      4. rm
         1. 谨慎使用rm -rf，以及其他可能不带有回收站设计的工具的删除功能
      5. mkdir和rmdir
      6. cat、more和less
      7. tail和head（＊）
      8. curl
         1. 使用curl下载文件
   5. 文本操作和编辑器
      1. vim（最）基本操作
         1. vimtutor
         2. hjkl
         3. i和ESC
         4. `:w`、`:q`和`:wq`（`:x`）
      2. nano基本操作演示
   6. 命令行功能实战：换源
   7. 一些好用的命令行工具Filesystem Hierarchy Standard (FHS)
      1. ncdu
      2. fd
3. Shell高级操作（＊）
   1. 我们在命令间传递信息的方式
      1. stdin、stdout和文件描述符
      2. xargs：从stdout到参数
      3. 重定向：重定向stdin和stdout到文件
      4. 管道：把上一个命令的stdout作为下一个命令的stdin的好办法
   2. 命令行快捷键（＊）
      1. C-L
      2. C-A/C-E
      3. C-U/C-K
      4. C-W/C-Del
      5. C-Left/C-Right
      6. C-R/C-P/C-N
      7. C-_
      8. C-Z
      9. `!$`/`!blah`
   3. 三种参数格式（＊）
      1. Unix风格的参数（-l）
      2. BSD风格的参数（list）
      3. GNU风格的长参数（--list）
   4. shell技巧
      1. 使用?和*作为通配符
      2. 使用!!调取上一条指令
   5. 包管理器的使用
   6. 进阶shell工具
      1. ln
      2. touch：创建文件和更新日期
      3. find
      4. file
      5. ps和top：系统性能监视
      6. du和df：文件夹存储和系统存储情况
      7. mount：挂载
      8. grep：筛选
      9. tar、zip、unzip和gzip：压缩和解压
      10. wget和aria2c：更好用的下载工具
      11. history：查看历史
      12. export
          1. 设置PATH变量
      13. alias：命令别名
      14. sudo
   7. Shell脚本编写（＊）
        1. 数组和变量
        2. 条件判断
        3. 循环
        4. 函数
4. 使用git进行版本管理
   1. 下载并配置git
   2. git init和git clone
   3. git add
   4. git commit
   5. git branch
   6. git checkout和git switch
   7. git reset
   8. git revert
   9. git merge
   10. git pull和git push
   11. 其他git操作
   12. GitHub CLI（＊）
