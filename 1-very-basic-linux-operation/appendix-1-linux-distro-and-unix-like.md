# 附录一：Linux 发行版和 Unix-like

本节内容涉及在技术界富有争议的话题，并且较大地偏离了本讲的主题，因而写在附录中。

## Linux 发行版和 Unix-like 的概念

Linux 发行版是基于 Linux 内核的完整操作系统。它一般包括 Linux 内核、GNU 工具链、软件包管理系统、桌面环境、预装应用、系统服务，以及将这些内容组织在一起的设计理念、政策。Windows 就是一个完整的操作系统，其内核名为 Windows NT；MacOS 也是一个完整的操作系统，其内核为 XNU（X is Not UNIX）。Linux 发行版的英文为 Linux Distribution，为称呼方便，也简称为 Linux Distro 或 Distro。

Android 操作系统也使用了 Linux 内核，我们说不同的手机厂商他们都是基于 Android 开发了自己的手机系统，而这些手机系统的内核都是 Linux。那么可以认为这里不同的手机系统其实就是不同的 Linux 发行版。（请注意：在交流情境中一般不认为 Android 各厂商所谓的系统是 Linux 发行版。狭义的 Linux 发行版不包括移动端的操作系统。即便依照定义而言，Android 上的操作系统的确可以归为 Linux 发行版。）

Unix-like 则是遵循 UNIX 设计哲学的系统（包括 Linux、BSD、MacOS 等）。也叫“类 UNIX 操作系统”

可以认为：

- Linux 内核：操作系统的核心，管理硬件资源。
- Linux 发行版：内核 + 配套软件，提供完整可用的系统。

## 常见的 Linux 发行版

我们常见的 Linux 发行版包括但不限于如下的几款（排名为字母序）：

- Aipine Linux：Alpine Linux 是一个由社区开发的基于 musl 和 BusyBox 的 Linux 操作系统。其以安全为理念，面向 x86 路由器、防火墙、虚拟专用网、IP电话盒及服务器。因其轻量化，也被广泛应用于容器技术中。
- Arch Linux：Arch Linux 是一个轻量的、灵活的 Linux 发行版，遵循 K.I.S.S.（在设计当中应当注重简约，“Keep It Simple, Stupid”）原则。Arch Linux 以滚动更新和“简洁”（Arch Linux 将简洁定义为：避免任何不必要的添加、修改和复杂化的增加）著称。它的初始安装仅提供命令行环境，一般被认为适合高级用户。
- AlmaLinux：自由且开放源代码的 Linux 发行版。被认为是 CentOS 的后继者之一。
- CentOS：来自于 RHEL 依照开放源代码规定发布的源代码所编译而成。以稳定可靠和与 RHEL 的兼容性良好著称。（事实上两者的不同，在于 CentOS 不包含封闭源代码软件。CentOS 对上游代码的主要修改是为了移除不能自由使用的商标。)Linux 教程《鸟哥的 Linux 私房菜》以 CentOS 7 为教学环境。2020 年 12 月 8 日，红帽公司宣布将停止开发 CentOS。目前，CentOS 的上游版本滚动更新版分支 CentOS Stream 开发仍在继续。
- CentOS Stream：CentOS 的上游版本滚动更新版分支。采用滚动更新策略。
- ChromeOS：由 Google 设计基于 ChromiumOS 的操作系统，并使用 Google Chrome 浏览器作为其主要用户界面。最初设置在上网本上使用，之后推展到笔记本（Chromebook）和台式机（Chromebox）上。
- ChromiumOS：是 ChromeOS 的开放源代码开发版本。试图能够为绝大多数长时间浏览万维网的用户提供一个快速、方便且安全的操作系统。
- Debian：完全由自由软件组成的类 UNIX 操作系统。在 Linux 专家和商业环境中流行。以稳定可靠和更新缓慢著称。
- Deepin：深度操作系统，亦称为 deepin，原名 Hiweed Linux 及 Linux deepin。由 武汉深之度科技有限公司 基于 Debian 开发。是较为出名的国产操作系统。
- elementary OS：基于 Ubuntu 的桌面 Linux 发行版，采用其独有的注重简洁、美观和易用性的桌面环境 Pantheon。深度集成了一系列 elementary OS 提供的应用程序。以简洁和直观为核心设计理念，许多用户认为其类似 macOS 的风格可以帮助新用户快速上手。
- Fedora：Fedora 由 Fedora 项目社群开发、Red Hat 赞助，目标是建立一套新颖、多功能并且自由（开放源代码）的操作系统。作为 RHEL 的上游存在（Red Hat 新技术在此被测试，稳定后被并入 RHEL）。
- Gentoo：基于 Portage 包管理系统，拥有几乎无限制的适应性特性。软件包从源代码构建。Gentoo Linux 的每一部分都可以在最终用户的系统上通过源码重新编译构造，甚至包括最基本的系统库和编译器自身。在 Linux 专家间流行。
- Kali Linux：基于 Debian 的 Linux 发行版，设计用于数字鉴识和渗透测试。由 Offensive Security 公司维护和资助。以其在网络安全、信息安全领域的专业性著称。
- KDE neon：由 KDE 社区直接开发，以最新的 Ubuntu 长期支持版为基础。被认为是有 KDE Plasma 桌面的第一方支持。
- Kubuntu：采用 KDE Plasma 桌面为其默认桌面环境发行版，和 Ubuntu 采用同样的底层系统和软件库。事实上，Kubuntu 和 Ubuntu 没有太大的差异，只是默认桌面系统采用 KDE，而非 GNOME 或 Unity。
- Linux From Scratch：《Linux From Scratch》（LFS）是杰勒德·比克曼斯等人编写的安装 Linux 的教科书，描述了从源代码编译 Linux 系统的方法。LFS 被用于描述通过同名书籍安装得到的 Linux 操作系统整体。LFS 的安装和维护难度较大，通常被认为是 Linux 专家的选择。
- Linux Mint：一个基于 Ubuntu 的发行版。宗旨是提供一个免费开源、现代、优雅、功能强大却也易于使用的操作系统。以易于使用和开箱即用著称。
- Loongnix：2015 年 8 月 18 日龙芯中科在产品发布会上发布的基于 Fedora 21 版本的新 Linux 操作系统。被认为是 LoongArch 架构的第一方操作系统。
- Manjaro：Manjaro Linux（或简称 Manjaro）是基于 Arch Linux 的 Linux 发行版。基于 Arch Linux，但拥有自己独立的软件仓库。目标是让强大的 Arch 更方便用户使用。
- NixOS：一个基于 Nix 的 Linux 发行版。系统的所有组件（包括内核、已安装的包和系统配置文件）都是由 Nix 从 Nix 表达式构建的。以所谓“可复现性”闻名。（Nix 要保证输入和输出是对应的，因此相同的配置文件可以得到一个相同的系统。）
- openSUSE：前身为 SUSE Linux 和 SuSE Linux Professional，是一个 Linux 发行版计划，由 SUSE Linux GmBH 与其他公司赞助。因为其徽标是一只蜥蜴，因而也被亲切称呼为“大蜥蜴”。它的开发重心是为软件开发者和系统管理者创造适用的开放源代码的工具，并提供易于使用的桌面环境和功能丰富的服务器环境。同样因易于上手及其专业性久负盛名。
- Puppy Linux：一个轻量级的 Linux 发行版。专注于易用性和尽量减少存储器使用量，适合在老设备上使用。
- Raspberry Pi OS：原为 Raspbian，基于 Debian 开发的操作系统，专为树莓派设备开发。从 2015 年起，树莓派基金会正式将其作为树莓派的官方操作系统。
- Red Hat Enterprise Linux：一个由 Red Hat 开发的商业市场导向的 Linux 发行版。Red Hat Enterprise Linux 的非官方简称是 RHEL。以稳定可靠和文档丰富著称，但是需要商用许可证。
- Rocky Linux：旨在成为一个使用 RHEL 操作系统源代码的完整的下游二进制兼容版本。被视为 CentOS 的后继者。以稳定可靠著称。
- Slackware：同样使用 K.I.S.S. 原则开发的 Linux 发行版，是最早的 Linux 发行版之一，亦是现存于世最古老的 Linux 发行版。在 Linux 专家间流行。
- SUSE Linux Enterprise：简称 SLE，是 SUSE 开发的企业级 Linux 操作系统。一般可以认为是 openSUSE 的商业版本，即便实际的情况有些不同。以稳定可靠著称。
- Ubuntu：基于 Debian 的发行版，是用户最多的 Linux 发行版之一。在中文社区异常出名，社区有海量的中文文档。在桌面操作系统、服务器、物联网设备上都得到了广泛的使用。有许多大大小小的派生版本。
- Ubuntu Kylin：基于 Ubuntu 的发行版。由中国工业和信息化部投资支持的开放源代码操作系统。以本土化和中文定制内容为特色。
- UOS：统一操作系统/统信操作系统（英文：Unity Operating System，简称 UOS）。是 2019 年由中国电子集团、武汉深之度科技有限公司、南京诚迈科技、中兴新支点在内的多家中国操作系统核心企业自愿发起并共同打造的基于 Linux 的操作系统。基于 Deepin Linux 开发，定位为商业化的操作系统，与 Deepin Linux 并行发展。
- Void Linux：一个独立的 Linux 发行版（即操作系统），以自有的 XBPS 作为软件包管理器。特色是以 Runit 作为 init 系统而非更常见的 systemd。遵循滚动更新。

我们不会在宣讲的过程中建议任何的 Linux 发行版，也不会建议你使用任何的技术！我们尽量避免向您输送任何价值观，尤其避免这种价值观可能是某种技术倾向的情况。这是我们的技术交流企划在其[白皮书](https://github.com/hitlug/hitlug-techex-whitepaper)中所承诺的。

Linux 发行版的好坏和取用在社区中是颇具争议的话题。我们建议您访问[DistroWatch](https://distrowatch.com/)获取更多对这些 Linux 发行版的技术评论。您应该根据你自己的实际需要选择适合自己的 Linux 发行版！

Linux 的发行版数量庞大！阅读[维基百科的 Linux 发行版词条](https://zh.wikipedia.org/zh-hans/Linux%E5%8F%91%E8%A1%8C%E7%89%88)以获取更多信息！该词条包含一张《[不同主要发行版开发过程的时间线](https://zh.wikipedia.org/zh-hans/Linux%E5%8F%91%E8%A1%8C%E7%89%88#/media/File:2023_Linux_Distributions_Timeline.svg)》，十分值得一看。

## 延伸阅读

- [Ubuntu 中文网的 Linux 发行版浅谈](https://wiki.ubuntu.org.cn/Linux%E5%8F%91%E8%A1%8C%E7%89%88%E6%B5%85%E8%B0%88)
