# 第零讲 环境配置和守则

Copyright 2025 wold9168@GitHub(Shinonome Tera tanuki)

---

## Linux实践中的几条金科玉律

我们避免在宣讲中向您灌输任何有关于价值观的内容。然而出于对您设备安全的考虑，我们不得不提醒您一些注意事项。

实践中的经验告诉我们“用户必须对其行为负责，因而用户必须清楚他要做什么”，因为：

1. 如果用户并不清楚其行为的后果，而要尝试进行这种行为，那么用户至少应该学会如何避免错误行动带来的严重后果。
2. 如果用户无法避免错误行动带来的严重后果，那么用户最好学会如何进行对该后果的补救。
3. 如果用户无法及时有效地补救错误行动带来的严重后果，那么用户应做好准备承担该严重后果带来的一切责任。

所以，我们最好：

1. 在行动前积极且广泛地阅读文档，无论是官方的文档还是社区的博客，亦或者是某些远古论坛、邮件列表上的讯息。
2. （尤其是对于初学者，）尽量避免在有知识上的欠缺的情况下对你的设备进行任何操作，尤其避免触碰键盘和鼠标。
3. 积累基本的知识，以在应对特殊情况时从容不迫。（这也是我们宣讲的目的之一）

## Docker的安装（Windows）

本节的内容是对Docker安装的官方教程的节选和再诠释。如您条件允许，阅读该教程的原文是更好的选择。其原文链接如下：

> https://docs.docker.com/desktop/setup/install/windows-install/

首先右键任务栏的Windows徽标（即所谓开始菜单的按钮），在右键菜单中选择带有“命令提示符”、“Powershell”、“Terminal”（终端）字样和“Administrator”（管理员）的字样的选项。执行这一步操作会打开一个用于输入命令的终端窗口。

在该终端窗口中输入如下指令并等待其执行：

```powershell
wsl --install
```

该指令执行完毕以后，窗口可以关闭。重启设备。

然后打开Docker的官网：

> https://www.docker.com/products/docker-desktop/

点击`Download Docker Desktop`，并选择`Download for Windows - AMD64`。随即浏览器会开始一个下载。您最终在您的下载目录里得到`Docker Desktop Installer.exe`。

双击运行该程序，遵照其安装向导进行安装。如无特殊情况，则不必修改任何安装选项。确认安装后，等待安装程序运行即可。

接下来，打开开始菜单中新出现的Docker Desktop。在新出现的窗口中点击“skip”跳过登录步骤。并通过和刚才一样的步骤，呼出Windows的终端窗口，在其中输入如下指令验证安装：

```powershell
docker pull debian:12
```

该指令将从Docker Hub下载`debian:12`镜像。由于国内运营商网络原因，会导致您拉取Docker Hub镜像变慢，甚至下载失败。因此，你可以使用加速器或阿里云的镜像加速服务。阿里云镜像加速服务参见如下链接：

> https://help.aliyun.com/zh/acr/user-guide/accelerate-the-pulls-of-docker-official-images

Docker Desktop修改`registry-mirrors`需要依次点击右上角的`Settings`，然后点击左边菜单列表中的`Docker Engine`，然后在该设置页面进行`registry-mirrors`的修改。可以参见：

> https://www.cnblogs.com/wenhsing/p/16471987.html

该指令执行成功，视为Docker安装成功。

## 使用懒人镜像

为了照顾完全没有计算机基础的同学，同时避免同学们在配置虚拟机、容器的时候遇到困难。我们准备了一份专用于教学的环境：

> https://github.com/wold9168/docker-debian-playground/tree/hitlug-techlect-2025-playground

该环境提供了简单的运行脚本，可以方便您进行Linux的学习。但并不适合于生产环境。

点击右上角绿色的`Code`，然后点击`Download ZIP`，然后解压得到的ZIP文件。

你可以遵照README.md中的描述选择对应的脚本，编译并启动容器，并进入容器的命令行环境。

编译并启动容器的脚本只需要执行一次，之后进入容器的命令行环境，不需要运行该脚本。

## 延伸阅读材料

1. USTCLUG的[Linux 101](https://101.lug.ustc.edu.cn/)
2. Missing Semester（[中文版讲义](https://missing-semester-cn.github.io/)）
3. 菜鸟教程的[Linux教程](https://www.runoob.com/linux/linux-tutorial.html)
4. [鸟哥的Linux私房菜](https://linux.vbird.org/)
5. GeekHour的[30分钟Shell光速入门教程](https://www.bilibili.com/video/BV17m411U7cC)
6. [命令行的艺术](https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md)
