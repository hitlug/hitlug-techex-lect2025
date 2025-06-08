
## 课前引导学生安装软件
前几分钟等人到齐，引导大家下载 mobaxterm vscode 等软件，并引导安装`mingw`，~~还可以用这段时间炫一会儿技~~
	虽然powershell也能将就用，但mobaxterm相对于powershell来说，体验更加linux一些；虽然mobaxterm有些时候体验也没那么linux就是了
	需要提前说明对于一些功能，mobaxterm与powershell的操作有一些不同
## 关于课程直播与录制
并且打算上课的同时在B站开直播，以防一些突发情况
	我们现在可是在B站同时直播的，所以不要聊一些危险话题，省的把直播间封掉
同时也可以直接使用B站的录播功能
	TODO：讨论用哪个B站账号直播和录播


## 课前调查
上课之前调查一下大家有多少人学过C语言
	预期：大部分人都学过，因为听说这几届大一所有专业（除了语言类，比如英语、俄语等）都要学习C语言。所以在课程中穿插一些C语言代码，会让很多人有一种熟悉的感觉
	注意点：一些C语言的编译操作需要在安装`mingw`后才能进行，故可能会有人看到我演示使用 `gcc` 编译时，尝试自己也用命令行编译C语言代码但是发现编译不了。这时可以在课间引导大家下载安装 `mingw`
	但如果学过C语言的人太少，或者发现大多数人都“学过但忘了”，则在课上适当调整操作顺序
可以在学生到场之后开讲之前调查

## 碎碎念
课程内容很可能会变得非常枯燥
可能需要学生提前准备一个笔记本来记笔记
需要学生提前准备电脑（如果是那种阶梯教室，没有给电脑充电的位置）
这个课纲第一节的内容很可能讲不完。如果观察时间不够的话，讲不完就讲不完了。但有一项内容必须要说一些：引导在vmware上安装ubuntu虚拟机
要不要有课程群？
如果课程反响太好，可能会有人要求把第一讲重讲一遍。因为很多人第一讲没听，但如果直接听第二讲的话，又害怕自己听不懂第二讲的内容（并且他们确实很可能也听不懂）
关于讲课时的设备：到时候是使用我自己的电脑，还是使用大教室的电脑
到时候讲的时候可以考虑多来两个人作为助教，在旁边起辅助作用。有可能一个人管不过来。到时候再说


------

# 课程内容

如何打开 mobaxterm 的 bash：session>shell，选择 bash
或者在文件夹页面右键>mobaxterm打开

课程初期，尽量不用 `vim` 编辑文件，而是使用 `vscode`。通过`code .`打开当前目录
并且关闭掉 `vscode` 的 `vim` 插件

## 目录操作与文件操作
### 目录操作(1):基础内容
`ls`:展示当前目录的内容
`cd`:切换目录
`pwd`:展示当前目录的地址
`mkdir`:新建目录
`rmdir`:删除目录
补充内容：绝对地址和相对地址
引入`ls path`
引入 `echo $PATH`和`which command`操作
	所以安装完了 `mingw` 需要一步，将 `mingw` 的安装目录增加到 `PATH` 内这一步
`linux`系统认为，所有的文件都放在同一个根目录以下，即`/`中；而不是像`windows`一样，不同的文件可以放在不同的盘中
### 文件操作
`cp`: 复制文件
`mv`: 移动文件
`rm`: 删除文件
`touch`:更新时间戳，也可以生成空白文件

### 目录操作(2):更加深入的内容
`ls`: 
	`-a`显示隐藏文件
	`-l`显示详情信息
	`-h`以易读格式展示文件大小
	可以使用`-la`等方式同时传入多个参数
	引入`alias`
`cp`:
	`-r dir1 dir2`:递归复制文件
`rm`:
	`-r`:递归删除目录内容
	`-f`:强制删除文件内容
`du`:
	检查目录大小
	`-h` 
	`-d` 递归深度
	引入 `ctrl+c` 终止程序

### 文件名字匹配
`echo *.txt`:`rm *.txt`
`touch {1,2,3}.txt`

## 数据流操作
补充内容：`stdin stdout stderr`
使用简单的C语言程序来引入这些内容
如果大家C语言水平都过关，可以适当引入提问环节
引入 `< file`、`> file`、`>> file`、`|`等操作符及其用法
引导大家自学重定义 `stderr` 操作
	例如 `2>&1` 等
	我认为关于这一部分，只需要让大家知道有这么回事就可以了。如果需要重定义`stderr`操作，可以在用的时候问大模型应该怎么用就可以
	但得告诉大家有这么回事

关于为什么要引入数据流操作：主要是如果不使用`stdin`和`stdout`这两个术语，很多时候描述起来会很不方便。经验之谈

### 特殊的文件句柄
大家都知道C语言的文件操作。一般都是使用 `fopen()` 函数打开一个文件句柄，该文件句柄通常为`FILE*`类型；最终使用`fclose()`函数关闭文件句柄。当需要读取或写入文件的时候，使用`stdio.h`和`stdlib.h`两个头文件提供的函数来对文件进行读写等操作。实际上有几个特殊的文件句柄，分别是`stdin`和`stdout`，这两个句柄在程序被创建时自动创建
```c
// echo.c
#include <stdio.h>
#include <stdlib.h>

int main(){
	int n;
	scanf("%d", &n);
	printf("%d\n", n);
	return 0;
}
```
上面的代码实际上可以把文件的读写改写为`fscanf(stdin, "%d", &n)`和`fprintf(stdout, "%d\n", n);`，只不过大家平常很少这么写就是了

```bash
./echo 
./echo < 1.in
./echo > 1.out
./echo < 1.in > 1.out
```
可以使用`<`和`>`对于`stdin`和`stdout`进行重定向。可以使用`|`将某个程序的`stdout`连接到另一个程序的`stdin`上
如果想要丢弃掉程序的`stdout`输出，可以将其重定向到`/dev/null`中。可以从`/dev/random`中获取随机输入

```
// add-one.c
#include <stdio.h>
#include <stdlib.h>

int main(){
	int n;
	scanf("%d", &n);
	printf("%d\n", n);
	return 0;
}

```
至于为什么要在介绍 `cat more less head tail` 等命令之前先介绍C语言与数据流和命令行参数之间的联系，原因是方便学生对于命令行的常用命令的工作方式进行更加深入的理解

## 对于命令行参数的解析
引入命令行参数解析
```c
// show-args
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char argv[]){
	printf("argc 的值为 %d\n", argc);
	for (int i=0; i<argc; i++){
		printf("argv[%d] 的值为 %s\n", i, argv[i]);
	}
	return 0;
}
```
一些特殊情况
```bash
./show-args one two
./show-args one\ two # 将空格转义
./show-args "one two" # 使用引号
./show-args "one\"two" # 使用转义符
```
## 关于信号
当某个**进程**接受到一个信号后，该进程对该信号做出相应的响应。例如，通常当给一个进程发送`SIGINT`信号时（输入`ctrl+c`），则该进程一般会终止。通常程序开发者可自行编写信号处理函数，这样便可以修改进程在收到信号后的默认相应。比如在使用`vim`的时候输入`ctrl+c`，该程序并不会自动终止
可以使用`kill`指令向某个进程发送信号（但这个指令只能在真正的`linux`系统上才可以使用）
	如果时间充足，可以在这里也给出C语言例子

## 文件流处理
`cat`:用于连接多个文件，并输出到`stdout`中。也可用于只查看某个文件的内容
```bash
$ cat a.txt
this is a.txt
$ cat b.txt
this is b.txt
$ cat a.txt b.txt
a.txt
b.txt
$
```
`tee`:将从`stdin`中读取的内容输出到`stdout`中，同时也该部分内容输出到指定文件内。使用`-a`参数可以追加输入
```bash
$ cat a.txt | tee b.txt
a.txt
$ 
```
`head`可用于读取文件的首部
```bash
$ head a.txt
$ head -n 10 a.txt
$ head -n10 a.txt
```
`tail` 可用于读取文件的尾部。也可使用`tail -f file-name`实时监控某文件更新
```bash
$ tail a.txt
$ tail -n10 a.txt
```
`grep` 可用于搜索文件内容
```bash
$ cat a.txt | grep "hello"

----

## 放松一下：一些常见的简单命令和操作
```bash
$ clear # 清屏
$ history # 历史命令
```
可以使用上下方向键来搜索之前输入过的指令
可以使用`tab`键帮助补齐指令
如果电脑已经安装了 `python`，可以使用 `python a.py` 来运行 `python` 程序。也可以试一下 `python` 的命令行操作
要不要试一下`tree /`（如果用的是`mobaxterm`，可以使用`find /`来替代），然后打开任务管理器，看一下磁盘读写是不是高了
要不要试一下 `open` 指令？
```bash
$ open .
$ open a.png # 打开图片
$ open b.html
$ 
```
敲一下`vim`，然后回车，试一下。只是想让新人体验一下在不会`vim`的时候误打误撞使用`vim`是一个什么感受；这种机会以后就没了
敲一下`exit`。啊，窗口怎么没了？
试一下`time du /`
切换到 `/drives/d` （其他的盘也可以），然后敲一下`time du /`和`time du -hd1 /`。这两个哪个更耗时？
	提示：I/O操作很耗时
## ubuntu 虚拟机安装



## 常识部分









