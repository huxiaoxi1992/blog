---
title: Linux 常用命令
date: 2019-07-25 13:12:29
tags: linux
categories: linux
---

# 包管理器

## apt-get Debian/Ubuntu 系统包管理器

`apt-get` 是 Debian/Ubuntu 系统中一个用于快速下载/安装的简单命令行管理工具

### 参数介绍

<!-- more -->

```sh
# 命令
update -检索新的包列表
upgrade -升级可更新的所有软件包
install -安装新软件包（pkg是libc6不是libc6.deb）
remove -删除软件包
autoremove -自动删除所有未使用的软件包
purge -删除软件包和配置文件
clean -清除已下载的归档文件
autoclean -清除旧的下载的档案文件
check -验证是否有损坏的依赖
download -下载二进制包到当前目录
# 选项：
-q ：不输出任何信息
-qq ：除了错误之外，没有输出
-d ：仅下载，不要安装或解压缩存档
-y ：对所有确定询问都选择Yes，并且不提示
-f ：尝试纠正被破坏依赖关系的系统
-m ：如果存档是可定位的，则尝试继续
-u ：显示升级包的列表
-b ：在获取源代码包后构建源包
# 更多的命令可以用 apt-get --help 查看。
```

### 使用示例

```sh
# 检索 新的包列表
apt-get update
# 升级 可更新的所有软件包（注意这个命令会升级所有的软件包，所以会升级很长时间）
apt-get upgrade
# 安装 Nginx 软件包
apt-get install nginx
# 卸载 Nginx 软件包
apt-get remove nginx
# 卸载 Nginx 软件包 并删除所有相关配置文件
apt-get remove --purge nginx
# 在安装软件和卸载的时候，为了避免误操作，都会询问是否继续，每次都要输入 y 来确定会很麻烦，可以加上 -y 参数
# 安装 Nginx 软件包 并不显示确定提示
apt-get install nginx -y
# 卸载 Nginx 软件包，删除所有相关配置文件 并不显示提示
apt-get remove --purge nginx -y
# 清除 旧的/无用 的软件包
apt-get clean && apt-get autoclean
# 下载 Nginx 二进制软件包到当前目录，但不解压和安装
apt-get download nginx -d
# 更多的命令可以用 apt-get --help 查看。
```

## yum CentOS 系统包管理器

yum 是 CentOS 系统中 一个用于快速下载/安装的简单命令行管理工具！

### 参数介绍

```sh
# 命令：
update -检索新的包列表
upgrade -升级软件包
search -搜索软件包
install -安装软件包
list -列出软件包或者软件包组
info -显示软件包或者软件包组的详细信息
erase -删除软件包（这两个命令一样）
remove -删除软件包（这两个命令一样）
groupinfo -显示有关包组的详细信息
groupinstall -安装软件包组（就像一种软件合集）
grouplist -列出可用的软件包组
groupremove -删除软件包组
check -检查软件包
check-update -检查可更新的软件包
clean -清除缓存目录内的软件包
deplist -列出一个包的依赖关系
distribution-synchronization -同步已安装的软件包到最新的版本
downgrad -降级一个软件包
reinstall -重新安装软件包（自动删除重装）
repolist -显示配置的软件包仓库
resolvedep -确定软件包需要的依赖关系
# 选项：
-t ：容忍错误
-C ：完全从系统缓存运行，不要更新缓存
-R 分钟：最大命令等待时间
-q ：安静的操作
-y ：对于所有问题回答是
--nogpgcheck ：禁用gpg签名检查
# 更多的命令可以用 yum --help 查看。
```

### 使用示例

```sh
# 检索 新的包列表
yum update
# 安装 Nginx 软件包
yum install nginx
# 安装 Development Tools 软件包组（这个软件包组中包含了编译所需的软件）
# 注意：当软件包或者软件包组的名字中包含空格的时候，请把 软件包或软件包组 加上双引号！
yum groupinstall "Development Tools"
# 卸载 Nginx 软件包
yum erase nginx / yum remove nginx
# 卸载 Development Tools 软件包组
yum groupremove "Development Tools"
# 升级 所有可更新的软件包
yum upgrade
# 升级 Nginx 可更新的软件包
yum upgrade nginx
# 在安装软件和卸载的时候，为了避免误操作，都会询问是否继续，每次都要输入 y 来确定会很麻烦，可以加上 -y 参数
# 安装 Nginx 软件包 并不显示确定提示
yum install nginx -y
# 卸载 Nginx 软件包 并不显示确定提示
yum erase nginx -y / yum remove nginx -y
# 搜索 Nginx 软件包是否存着
yum search nginx
# 列出 可用的软件包
yum list
# 列出 可用的软件包组
yum grouplist
# 清除 缓存目录中的所有软件包
yum clean
# 清除 缓存目录中的 Nginx 软件包
yum clean nginx
# 重装 Nginx 软件包
yum reinstall nginx
# 更多的命令可以用 yum --help 查看。
```

# 文件/文件夹 操作

以下除特殊说明，都以当前目录为 /root 示例。

## mkdir 新建文件夹

```sh
# 在当前文件夹新建一个 bash 文件夹，完整的绝对路径就是 /root/bash
mkdir bash
# 更多的命令可以用 mkdir --help 查看。
cd 进入 文件夹
# 你当前在 /root目录中，使用这个命令会进入 /root/bash目录，这是相对路径
cd bash
# 如果你不在 /root目录中的话，就不能用上面的相对路径了，就需要绝对路径
cd /root/bash
# 假设你当前在 /root/bash目录中，使用相对路径，你可以用这个命令进入上一级 /root目录， .. 代表相对路径 上级目录
cd ..
# 当然你也可以用绝对路径来进入上一级 /root目录
cd /root
```

## cd 进入 文件夹

```sh
# 你当前在 /root目录中，使用这个命令会进入 /root/bash目录，这是相对路径
cd bash
# 如果你不在 /root目录中的话，就不能用上面的相对路径了，就需要绝对路径
cd /root/bash
# 假设你当前在 /root/bash目录中，使用相对路径，你可以用这个命令进入上一级 /root目录， .. 代表相对路径 上级目录
cd ..
# 当然你也可以用绝对路径来进入上一级 /root目录
cd /root
```

## cp 复制或重命名 文件/文件夹

```sh
# 复制当前目录内的 log.txt文件到 /var目录
cp log.txt /var/log.txt
# 复制当前目录内的 bash文件夹到 /home目录
cp -R bash /home/bash
cp *.txt /var/log
# 复制当前目录内的所有以 test开头的文件到 /var/log目录
cp test*/var/log
# 复制当前目录内的所有以 test开头 以.txt后缀结尾的文件到 /var/log目录
cp test*.txt /var/log
# 假设当前目录是 /root/test/log，要把这个目录中的所有.txt后缀的文件复制到上一级目录 /root/test，那么这样做
cp *.txt ..
# .. 就是相对路径，代表上一级目录，当然你也可以用绝对路径，这样更不容易出错
cp *.txt /root/test
# 重命名当前目录内的 log.txt文件为 log2.txt
cp log.txt log2.txt
# 复制当前目录内的 log.txt文件到 /var目录并重命名为 log1.txt
cp log.txt /var/log1.txt
# 复制当前目录内的 bash文件夹到 /home目录并重命名为 bash2
cp -R bash /home/bash2
# 复制当前目录内的 log.txt文件到 /var目录，但是 /var 目录中已经存着 log.txt，那么会提示 cp: overwrite `/var/log.txt'? 可以用 -f 强制覆盖
cp -f log /var/log.txt
# 复制当前目录内的 log.txt log1.txt log2.txt文件和 log233目录到 /home/log目录中
cp -R log.txt log1.txt log2.txt log233 /home/log
# 更多的命令可以用 cp --help 查看。
```

## mv 移动或重命名 文件/文件夹

```sh
# 关于 mv 命令，可以参考上面 cp 的使用方法，没什么区别，只是一个是复制，一个是移动，把上面 cp 命令改成 mv 就能套用了。
# 移动当前目录内的 log.txt文件到 /var目录
mv log.txt /var/log.txt
# 移动当前目录内的 bash文件夹到 /home目录
mv bash /home/bash
# 重命名当前目录内的 log.txt文件为 log2.txt
mv log.txt log2.txt
# 复制当前目录内的 log.txt文件到 /var目录并重命名为 log1.txt
mv log.txt /var/log1.txt
# 复制当前目录内的 bash文件夹到 /home目录并重命名为 bash2
mv bash /home/bash2
# 更多的命令可以用 mv --help 查看。
```

## rm 删除 文件/文件夹

```sh
# 删除当前目录下的 log.txt文件
rm log.txt
# 删除当前目录下所有.txt后缀的文件
rm *.txt
# 使用 rm 命令删除时，会提示你是否确定删除，输入 y 即删除，输入 n 则取消
# rm: remove regular file `log.txt'? y
# 删除当前目录下所有.txt后缀的文件
rm *.txt
# 删除当前目录下所有以 test开头的文件
rm test*
# 删除当前目录下所有以 test开头 以.txt后缀结尾的文件
rm test*.txt
# 当你用 rm 删除目录的时候会发现提示这不是一个文件
# rm bash
# rm: cannot remove `bash': Is a directory
# 可以加上 -r 来归递删除目录及其目录下的内容
rm -r bash
# 因为为了避免手误删除错误，所以 rm默认是加上了 -i 的参数，也就是每一次删除文件/目录都会提示，如果觉得烦可以用 -rf 参数
rm -rf bash
# rm -rf 这个命令请慎重使用，而且千万不要使用 rm -rf / 或者 rm -rf /* 之类的命令(系统自杀)，可能会让你系统爆炸，所以使用请慎重！
# 更多的命令可以用 rm --help 查看。
```

# 查找 操作

## find 文件查找

find 命令用于查找文件系统中的指定文件

```sh
# 命令格式
find 要查找的路径表达式
# 在当前目录及其子目录下查找文件 “1.txt”；
find . -name 1.txt
# 在 “/tmp” 目录及其子目录下查找文件“1.txt”。
find /tmp -name 1.txt
```

## grep 文件内容查找

grep 命令用于查找指定的模式匹配

```sh
# 命令格式
grep [命令选项] 要查找的匹配模式 [要查找的文件]
# 在 “test.txt” 文件中查找cams 字符串；
grep cams test.txt
# 在 “/root/cams” 目录及其子目录下的所有文件中，查找cams 字符串；
grep -r cams /root/cams
# grep 命令除了能够查找文件外，还能够将任意输出流重定向到grep 进行查找：
# 查找进程名中包含 “ora” 的所有进程信息。
ps -ef | grep ora
```

# 查看/编辑文件 操作

## ls 显示目录中文件

```sh
# 显示当前目录下的所有文件
ls -a
# 命令后面加上 绝对路径/相对路径 就会显示指定文件夹内的所有文件
ls -a bash/log
# 相对路径，当前目录是 /root ，欲查看的目录是 /root/bash/log
ls -a /root/bash/log
# 绝对路径， 当前目录是 /root ，欲查看的目录是 /root/bash/log
# 更多的命令可以用 ls --help 来查看。
```

## du 查看 文件/文件夹 占用磁盘空间的大小

### 参数介绍

```sh
-h ：以人类易读的方式显示
-a ：显示目录占用的磁盘空间大小，并显示其下目录和文件占用磁盘空间的大小
-s ：显示目录占用的磁盘空间大小，但不显示其下子目录和文件占用的磁盘空间大小
-c ：显示几个目录或文件占用的磁盘空间大小，还要统计它们的总和
--apparent-size：显示目录或文件自身的大小
-l ：统计硬链接占用磁盘空间的大小
-L ：统计符号链接所指向的文件占用的磁盘空间大小
# 待写...
# 更多的命令可以用 du --help 来查看。
```

### 使用示例

```sh
# 显示 /root 文件夹的大小，但不显示其子目录和文件的大小
du -sh
# 显示 /root 文件夹的大小，并显示其子目录和文件的大小
du -ah
# 待写...
# 更多的命令可以用 du --help 来查看。
```

## cat 查看文件内容

假设 log.txt 文件的内容为：

```sh
test233
test
test666
test2366
test8888
```

### 查看文件

```sh
# 查看 log.txt 文件的所有内容

cat log.txt

# 输出示例如下

test233
test
test666
test2366
test8888

# 查看 log.txt 文件的所有内容，并对所有行编号

cat -n log.txt

# 输出示例如下：

1 test233
2 test
3
4
5 test666
6
7 test2366
8 test8888

# 查看 log.txt 文件的所有内容，并对非空行编号

cat -b log.txt

# 输出示例如下：

1 test233
2 test
3 test666
4 test2366
5 test8888

# 查看 log.txt 文件的所有内容，并对非空行编号，且不输出多行空行

cat -bs log.txt

# 输出示例如下：

1 test233
2 test
3 test666
4 test2366
5 test8888
```

### 清空文件

```sh
# 清空当前目录中的 log.txt 文件

cat /dev/null> log.txt

# 清空 /var 目录中的 log.txt 文件

cat /dev/null>/var/log.txt
```

### 写入文件

```sh
# 写入文本到当前目录中的 log.txt 文件中(加入文本到文件内容最后)

cat >> log.txt <<-EOF
test
test233
test666
EOF

# 清空文件并写入文本到 /var 目录中的 log.txt 文件中(先清空后写入)

cat >/var/log.txt <<-EOF
test
test233
test666
EOF

# 更多的命令可以用 cat --help 来查看。
```

## head 查看文件内容（主要用于正查）

### 参数介绍

```sh
-c 数字：显示指定文件的前 xx 字节的内容（bytes）
-n 数字：显示指定文件的前 xx 行的内容
-q ：不显示包含指定文件名的文件头（当使用 head 打开多个文件的时候，会去在每个文件输出结果的顶部添加一个包含文件名的文件头用于区分）

# 更多的命令可以用 head --help 来查看。
```

### 使用示例

假设 log.txt 文件内容为：

```sh
test1
test2
test3
test4
test5

# 查看 log.txt 文件的全部内容

head log.txt

# 查看 log.txt 文件的前 4 字节的内容

head -c 4 log.txt

# 输出示例

doub

# 查看 log.txt 文件的前 2 行的内容

head -n 2 log.txt

# 输出示例

test1
test2

# 查看 log.txt 文件的从倒数第 2 行到行首的内容

head -n -2 log.txt

# 输出示例

test1
test2
test3

# 查看 log.txt log1.txt log2.txt 文件的前 3 行内容

head -n 3 log.txt log1.txt log2.txt

# 更多的命令可以用 head --help 来查看。
```

## tail 查看文件内容（主要用于倒查）

```sh
-c 数字：如果数字为正数(例如-c +5)，显示指定文件从行首第 xx 字节到最后的内容；如果数字为负数(例如-c -5)，显示指定文件从行尾第 xx 字节到最后内容。
-n 数字：如果数字为正数(例如-c +3)，显示指定文件从行首第 xx 行到最后的内容；如果数字为负数(例如-c -3)，显示指定文件从行尾第 xx 行到最后的内容。
-f ：即时输出文件变化后增加的内容，也就是监视一个文件的内容变化（常用于监视日志输出），使用 Ctrl ＋ C 终止

# 更多的命令可以用 tail --help 来查看。
```

### 使用示例

假设 log.txt 文件内容为：

```sh
test1
test2
test3
test4
test5

# 查看 log.txt 文件的全部内容

tail log.txt

# 查看 log.txt 文件从行首 第 25 字节到最后的内容

tail -c +25 log.txt

# 输出示例

bi4
test5

# 查看 log.txt 文件从行尾 第 4 字节到最前面的内容

tail -c -4 log.txt

# 输出示例

bi5

# 查看 log.txt 文件的从第 2 行到最后一行的内容

tail -n +2 log.txt

# 输出示例

test2
test3
test4
test5

# 查看 log.txt 文件的后 2 行的内容

tail -n -2 log.txt

# 输出示例

test4
test5

# 持续查看（监视） log.txt 文件的变化内容（新增加的内容），使用 Ctrl ＋ C 终止

tail -f log.txt

# 查看 log.txt log1.txt log2.txt 文件的前 3 行内容

tail -n 3 log.txt log1.txt log2.txt

# 更多的命令可以用 tail --help 来查看。
```

## sed 查看/编辑文件内容

### 参数介绍

```sh
-i ：操作后应用保存到原文件（如果不加这个参数，那么任何修改都不会影响原文件里的内容，只会把结果输出）

# 待写...

# 更多的命令可以用 sed --help 来查看。
```

### 使用示例

```sh
# 查看 log.txt 第 3 行的内容

sed '3p' log.txt

# 查看 log.txt 第 2-8 行的内容

sed '2,8p' log.txt

# 删除 log.txt 第 4 行

sed -i '4d' log.txt

# 删除 log.txt 第 3-7 行

sed -i '3,7d' log.txt

# 删除 log.txt 第 1 行

sed -i '1d' log.txt

# 删除 log.txt 最后 1 行

sed -i '\$d' log.txt

# 删除 log.txt 文件中所有包含 233 内容的行

sed -i '/233/d' log.txt

# 替换 log.txt 文件中所有 233 为 666

sed -i 's/233/666/' log.txt

# 替换 log.txt 文件中所有 /ver 为 test/，因为有斜杠，所以需要使用 \ 转义，但是单引号会导致无法转义，所以要改成双引号。

sed -i "s/\/ver/test\//" log.txt

# 待写...

# 更多的命令可以用 sed --help 来查看。
```

# vi、vim、nano 编辑文件内容

## vi 介绍

vi 是 Linux 很棒的一个文本编辑器，不过也存在一些缺点，比如操作略麻烦。而 vim 就相当于 vi 的扩展或者加强版，主要介绍 vim。

## vim 介绍

vim 相当于 vi 的扩展或者加强版，一些系统只安装了 vi，所以想要用 vim 还需要手动安装( yum install vim -y / apt-get install vim -y)，安装 vim 后，会自动替换或者说整合 vi。

当你使用 vi 命令的时候，首先进入的是 命令行模式，这个模式就是 vi 自身的功能，而点击 I 键 后就会进入编辑模式(插入模式)，这时候就可以直接输入字符了，这个就是 vim 的扩展功能了。当修改完成后，按 ESC 键 即可退出编辑模式回到命令行模式，这时候输入 :wq 并回车代表保存并退出，如果不想保存可以使用 :q! 不保存强制退出。
vim 的命令行 命令很多，我也没打算都写出来，只写出最常用的好了。

```sh
# 打开当前目录下的 log.txt 文件，如果没有那么会新建 log.txt 文件（安装 vim 后，使用 vi 和 vim 打开文件没区别）

vi log.txt
vim log.txt

# 在命令行模式下，直接输入以下 符号和字母(区分大小写)

## 进入编辑模式（插入模式，按 Esc 键 即可返回命令行模式）

i

## 删除光标当前所在的一行

dd

## 删除文件内所有内容

dddG

## 复制光标当前所在的一行

yy

## 粘贴刚才复制的一行内容

p

## 撤销上个操作（误操作可以用这个恢复）

u

## 保存当前文件（ : 是英文的冒号）

:w

## 另存当前文件内容为 log2.txt

:w log2.txt

## 退出当前文件

:q

## 不保存 并强制退出当前文件

:q!

## 保存并退出当前文件

:wq

# 更多的命令可以用 vi --help / vim --help 来查看。
```

## nano 介绍

nano 我挺少用的，一些系统也默认安装了这个 文本编辑器，在一些地方比 vim 好用，不过我已经习惯了 vim。

```sh
# 打开当前目录下的 log.txt 文件，如果没有那么会新建 log.txt 文件

nano log.txt

# 进入后直接就可以输入修改文本内容了，修改后我们可以使用这个 按键保存内容

Ctrl+O

# 如果不需要编辑了，那么可以用这个 按键退出当前文件

Ctrl+X

# 如果你在退出前已经修改但没有保存，那么会提醒你是否保存，如果保存就输入 y ，不保存输入 n

# 然后就会让你输入要保存的文件名（默认原文件名，所以直接按 Enter 回车即可，除非你要另存为其他文件名）

Enter

# 更多的命令可以用 nano --help 来查看。
```

# 解压缩 操作

在 Linux 中经常会下载到压缩文件，而压缩文件的格式有很多，比如 zip、rar、gz、xz、tar.gz、tar.xz 等。
比较常见的就是各种 .tar、.tar.xz、.tar.gz、.tar.bz、.tar.bz2、.tar.Z 后缀压缩包，这几个的解压缩命令基本一样，说明一下参数的意义。
tar 本身只是一个打包的作用，而 .tar 后面的 .zx / .gz / .bz 等等才是压缩格式，也就是比如 log.tar.gz 压缩包，就是先用 .tar 把指定文件/文件夹打包到一起，然后再用 gz 来压缩打包后的 .tar 为 .tar.gz 。

```sh
-x 是从压缩文件提取(解压)文件出来，所以在解压命令中都有这个参数。
-c ：创建一个新的压缩包文件，所以在压缩命令中都有这个参数。
-f ：指定要解压的压缩包文件或要压缩的文件/文件夹，所以这个参数必须放在解压缩命令参数的最后，然后后面跟着要解压到压缩包文件或要压缩的文件/文件夹。
-j ：解压缩 bz / bz2 格式的参数
-J ：解压缩 xz / lzip 格式的参数
-z ：解压缩 gz / tgz 格式的参数
-Z ：解压缩 Z 格式的参数
-v ：详细列出解压缩过程中处理的文件

# 更多的命令可以用 tar --help 来查看。
```

## tar gz zip 等 解压 压缩包 示例

```sh
# 解压后缀为 .tar 的压缩包

tar -xf log.tar

# 解压后缀为 .tar.xz 的压缩包

tar -xJf log.tar.xz

# 解压后缀为 .tar.gz 的压缩包，有两个方法

tar -xzf log.tar.gz

# 解压后缀为 .gz 的压缩包，有两个方法，如提示命令不存在，请安装 yum install gzip -y / apt-get install gzip -y

gzip -d log.gz
gunzip log.gz

# 解压后缀为 .bz / .bz2 / tar.bz2 的压缩包，有两个方法

bzip2 -d log.bz
bunzip2 log.bz
tar -jxf log.tar.bz

bzip2 -d log.bz2
bunzip2 log.bz2
tar -jxf log.tar.bz2

# 解压后缀为 .Z / tar.Z 的压缩包，有两个方法

uncompress log.Z log.txt
uncompress log.Z log
tar xZf log.tar.Z log.txt
tar xZf log.tar.Z log

# 解压后缀为 .rar 的压缩包，如提示命令不存在，请安装 yum install unrar -y / apt-get install unrar -y ，注意 rar 和 unrar 是分开的

unrar x log.rar

# 解压后缀为 .zip 的压缩包，如提示命令不存在，请安装 yum install unzip -y / apt-get install unzip -y，注意 zip 和 unzip 是分开的

unzip log.zip

# 更多的命令可以用 tar --help / gzip --help / unrar --help / unzip --help 来查看。
```

## 压缩 文件/文件夹 示例

```sh
# 分别压缩当前目录下的 log.txt 文件 / log 文件夹为 log.tar 压缩包

tar -cf log.tar log.txt
tar -cf log.tar log

# 如果要压缩多个文件和文件夹，那么只需要在后面一直加下去即可

tar -cf log.tar log.txt doub.txt log bash

# 分别压缩当前目录下的 log.txt 文件 / log 文件夹为 log.tar.xz 压缩包，以下的其他后缀压缩命令都是一样

tar -cJf log.tar.xz log.txt
tar -cJf log.tar.xz log

# 分别压缩当前目录下的 log.txt 文件 / log 文件夹为 log.tar.gz 压缩包

tar -czf log.tar.gz log.txt
tar -czf log.tar.gz log

# 分别压缩当前目录下的 log.txt 文件 / log 文件夹为 log.gz 压缩包

gzip log.gz log.txt
gzip log.gz log

# 分别压缩当前目录下的 log.txt 文件 / log 文件夹为 log.tar.bz 压缩包

暂时没查到

# 分别压缩当前目录下的 log.txt 文件 / log 文件夹为 log.bz / log.tar.bz / log.bz2 / log.tar.bz2 压缩包

bzip2 -z log.txt
bzip2 -z log
tar cjf log.tar.bz2 log.txt
tar cjf log.tar.bz2 log

# 分别压缩当前目录下的 log.txt 文件 / log 文件夹为 log.Z / log.tar.Z 压缩包

compress log.txt
compress log
tar cZf log.tar.Z log.txt
tar cZf log.tar.Z log

# 分别压缩当前目录下的 log.txt 文件 / log 文件夹为 log.rar 压缩包，如提示命令不存在，请安装 yum install rar -y / apt-get install rar -y ，注意 rar 和 unrar 是分开的

unrar a log.rar log.txt
unrar a log.rar log

# 分别压缩当前目录下的 log.txt 文件 / log 文件夹为 log.zip 压缩包，如提示命令不存在，请安装 yum install zip -y / apt-get install zip -y ，注意 zip 和 unzip 是分开的

zip log.zip log.txt
zip log.zip log

# 更多的命令可以用 tar --help / gzip --help / rar --help / zip --help 来查看。
```

# 网络工具

## wget 下载工具

wget 是 Linux 系统最常用的工具之一，命令行方式的多功能下载工具，支持 HTTP，HTTPS 和 FTP 协议。

### 参数介绍

```sh
# 只介绍最常用的参数

# 如果提示命令不存在，那么使用 yum install wget -y / apt-get install wget -y 来安装（有一些非常精简的系统可能会没装）

-b ：启动后，后台下载
-q ：安静模式（不输出任何信息）
-c ：断点续传下载文件
-O ：指定下载后的文件名（可使用绝对路径目录+文件名）
-P ：指定下载后的文件目录（-P 只能指定下载目录，并不能指定文件名）
-t ：设置重试次数（0 代表无限）
-T ：设置超时时间（单位：秒）
-N ：只获取比本地新的文件（新的覆盖旧的）
-4：仅连接至 IPv4 地址
-6：仅连接至 IPv6 地址
--limit-rate=xxxk :限制下载速度（k 代表 KB/S）
--post-data ：通过 POST 方式发送数据
--no-check-certificate ：不验证服务器的 SSL 证书

# 更多的命令可以用 wget --help 来查看。
```

### 使用示例

```sh
# 下载一个文件到当前目录

wget https://softs.pw/100MB.bin

# 下载文件到当前目录并重命名为 200MB.bin

wget -O "200MB.bin" https://softs.pw/100MB.bin

# 下载文件到 /root 目录（-P 只能指定下载目录，并不能指定文件名）

wget -P "/root" https://softs.pw/100MB.bin

# 下载文件到 /root/test 目录并重命名为 200MB.bin

wget -O "/root/test/200MB.bin" https://softs.pw/100MB.bin

# 下载文件完成之前 wget 进程结束了，那么可以使用断点续传重新下载中断的文件（前提是下载服务器支持断点续传）

wget -c https://softs.pw/100MB.bin

# 通过后台下载文件到 /root/test 目录并重命名为 200MB.bin

wget -b -O "/root/test/200MB.bin" https://softs.pw/100MB.bin

# Continuing in background, pid 2333.

# Output will be written to `wget-log'.

# 后台下后，你可以使用以下命令来查看下载进度：

tail -f wget-log

# 有时候一些 Linux 系统中的 SSL 证书不完整，会导致下载一些 HTTPS 网站文件的时候会验证 SSL 证书失败，可以这样做

# 不验证服务器 SSL 证书，下载文件到当前目录并重命名为 200MB.bin

wget --no-check-certificate -O "200MB.bin" https://softs.pw/100MB.bin

# 使用 wget 发送 POST 请求数据

wget --post-data "user=test&passwd=23333" https://xxx.xx/

# 下载文件到当前目录 并仅通过 IPv4 连接 只获取比本地新的文件，限速 200KB/S

wget --limit-rate=200k-N -4 https://softs.pw/100MB.bin

# 下载文件到当前目录 并重试次数为 1，超时时间为 2 秒

wget -t1 -T2 https://softs.pw/100MB.bin

# 通过 wget 来获取服务器的外网 IP（-qO- 代表运行完会输出下载的信息，并不会保存到本地文件）

wget -qO- ipinfo.io/ip

# 更多的命令可以用 wget --help 来查看。
```

## curl 下载工具

curl 是 Linux 系统一个利用 URL 规则在命令行下工作的文件传输工具，是一款很强大的 HTTP 命令行工具。它支持文件的上传和下载，是综合传输工具，但习惯称 curl 为下载工具。

### 参数介绍

```sh
# 只介绍最常用的参数

# 如果提示命令不存在，那么使用 yum install curl -y / apt-get install curl -y 来安装（有一些非常精简的系统可能会没装）

-s ：安静模式（不会输出任何信息）
-C ：断点续传下载文件
-o ：输出写入到文件中
-O ：输出写入到文件，文件名为远程文件的名称
-k ：不验证服务器 SSL 证书
-T ：上传文件
-4：仅连接至 IPv4 地址
-6：仅连接至 IPv6 地址
-m ：设置传输总时间（单位：秒）
--retry：设置重试次数
--data ：通过 POST 方式发送数据
--limit-rate xxxK ：限制下载速度（K 代表 KB/S）

# 更多的命令可以用 curl --help 来查看。
```

### 使用示例

```sh
# 获取当前服务器的外网 IP

curl ipinfo.io/ip

# 获取一个文件保存到当前目录中

wget -O https://softs.pw/Bash/ssr.sh

# 获取一个文件保存到 /root/test 目录中 并修改文件名为 233.sh

curl -o "/root/test/233.sh" https://softs.pw/Bash/ssr.sh

# 下载文件完成之前 curl 进程结束了，那么可以使用断点续传重新下载中断的文件（前提是下载服务器支持断点续传）

curl -C -O https://softs.pw/100MB.bin

# 有时候一些 Linux 系统中的 SSL 证书不完整，会导致访问/下载一些 HTTPS 网站/文件的时候会验证 SSL 证书失败，可以这样做

# 不验证服务器 SSL 证书，下载文件到当前目录并重命名为 233.sh

curl -k -o "233.sh" https://softs.pw/Bash/ssr.sh

# 使用 curl 发送 GET 请求数据

curl https://xxx.xx/?user=test

# 使用 curl 发送 POST 请求数据

curl --data "user=test&passwd=23333" https://xxx.xx/

# 下载文件到当前目录 并仅通过 IPv4 连接，限速 200KB/S

curl --limit-rate 200K-4 https://softs.pw/100MB.bin

# 下载文件到当前目录 并重试次数为 1，超时时间为 2 秒

curl --retry1-m 10 https://softs.pw/100MB.bin

# 更多的命令可以用 curl --help 来查看。
```

## netstat 查看链接和端口监听等信息

### 参数介绍

```sh
-n ：不显示别名（主机名/域名以数字或 IP 显示）
-e ：显示其他/更多信息
-p ：显示进程 PID/进程名
-c ：持续输出（设置后会每隔 1 秒输出一次，Ctrl+C 终止）
-l ：显示正在监听的套接字
-a ：显示全部信息

# 下面这些就不很常用了。

-r ：显示路由表
-i ：显示网络接口（网卡）
-g ：显示多播组信息
-s ：显示网络统计
-M ：显示伪装连接
-v ：显示正在进行的工作

# 更多的命令可以用 netstat --help 来查看。
```

### 使用示例

```sh
# 显示当前服务器的所有连接信息

netstat -a

# 显示当前服务器的所有 TCP 连接信息

netstat -at

# 显示当前服务器的所有 UDP 连接信息

netstat -au
```

一般来说经常使用这个命令：

```sh
# 显示当前服务器的所有正在监听 TCP 端口的信息，并且 显示进程 PID 和进程名，但不显示别名（域名以 IP 显示），这个命令算是最常用的了。

netstat -lntp

# 输出示例

Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address Foreign Address State PID/Program name
tcp 0 0 0.0.0.0:80 0.0.0.0:_ LISTEN 14233/nginx.conf
tcp 0 0 0.0.0.0:22 0.0.0.0:_ LISTEN 1555/sshd
tcp 0 0 0.0.0.0:443 0.0.0.0:_ LISTEN 14233/nginx.conf
tcp6 0 0 :::22 :::_ LISTEN 1555/sshd

# 显示监听 80 端口的进程 PID 和进程名，grep 是匹配并显示 符合关键词的行。

netstat -lntp|grep ":80"

# 输出示例

Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address Foreign Address State PID/Program name
tcp 0 0 0.0.0.0:80 0.0.0.0:\* LISTEN 14233/nginx.conf

# 显示 ssh 的监听情况，grep 是匹配并显示 符合关键词的行。

netstat -lntp|grep "ssh"

# 输出示例

Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address Foreign Address State PID/Program name
tcp 0 0 0.0.0.0:22 0.0.0.0:\* LISTEN 1555/sshd
```

表头解释

```sh
Proto：连接协议（tcp/udp 是 IPv4，tcp6/udp6 是 IPv6）
Recv-Q ：接收队列（基本都是 0，如果不是代表堆积）
Send-Q ：发送队列（基本都是 0，如果不是代表堆积）
LocalAddress：本地地址和端口
ForeignAddress：对外地址和端口
State：连接状态
PID/Program name ：进程 PID/进程名

# 每隔 1 秒显示一次当前服务器的所有连接信息

netstat -c

# 每隔 1 秒显示一次当前服务器的所有 TCP 连接信息

netstat -ct

# 每隔 1 秒显示一次当前服务器的所有 UDP 连接信息

netstat -cu

# 显示当前服务器的路由表

netstat -r

# 显示当前服务器的网络接口信息（网卡）

netstat -i

# 显示当前服务器的网络统计信息

netstat -s

# 更多的命令可以用 netstat --help 来查看。
```

在使用 netstat 命令中，会显示一些连接状态，下面是各状态的意思：
LISTEN

```sh
# 监听来自远程连接的 TCP 端口连接请求

SYN-SENT

# 在发送连接请求后，等待匹配的连接请求

SYN-RECEIVED

# 在收到和发送一个连接请求后，等待对方对连接请求的确认

ESTABLISHED

# 代表一个打开的连接

FIN-WAIT-1

# 等待远程 TCP 连接中断请求，或先前的连接中断请求的确认

FIN-WAIT-2

# 从远程 TCP 等待连接中断请求

CLOSE-WAIT

# 等待从本地用户发来的连接中断请求

CLOSING

# 等待远程 TCP 对连接中断的确认

LAST-ACK

# 等待原来的发向远程 TCP 的连接中断请求的确认

TIME-WAIT

# 等待足够的时间，以确保远程 TCP 接收到连接中断请求的确认

CLOSED

# 没有任何连接状态（或者关闭了连接）
```

# 系统命令

## ps 查看进程信息

### 参数介绍

```sh
待写...

# 更多的命令可以用 man ps 来查看。

使用示例：

# 显示当前进程信息

ps -ef

# 显示 ssh 进程（ grep -v grep 表示排除关键词 grep，因为使用 grep 匹配 ssh，也会把 grep 自己的进程匹配进去的）

ps -ef|grep -v grep|grep ssh

# 输出示例

UID PID PPID C STIME TTY TIME CMD #注意使用上面命令的话是不会显示表头这一行的，我只是为了更好理解加上的
root 17381001/27?00:08:56/usr/sbin/sshd

# 待写...
```

表头解释：

```sh
UID ：启动进程的用户
PID ：进程标识符（进程 1 代表 init 是整个系统的父进程）
PPID ：父进程标识符（进程 1 代表 init 是整个系统的父进程）
C ：CPU 占用率%
STIME ：启动进程的日期
TTY ：终端号
TIME ：进程运行时间（非休眠状态）
CMD ：启动进程的命令（或进程名/进程程序所在目录）
```

## kill 结束进程

```sh
# 当我们想要结束一个进程的时候，我们可以用 kill 命令

# PID 是每个进程的一个唯一标识符，可以使用上面的 ps 命令来查看你要结束进程的 PID。

# 假设我们要结束 Nginx 的进程，那么这样做（ grep -v grep 表示排除关键词 grep，因为使用 grep 匹配 ssh，也会把 grep 自己的进程匹配进去的）：

ps -ef|grep -v grep|grep "nginx"

# 输出示例

UID PID PPID C STIME TTY TIME CMD #注意使用上面命令的话是不会显示表头这一行的，我只是为了更好理解加上的
root 23561004/03?00:03:12 nginx

# 然后我们可以看到第二列的 PID 进程标识符，然后我们 kill 即可。

kill -92356

# 中断进程 -2 相当于 程序运行在前台，然后输入 Ctrl+C 的效果，但是进程有权利忽略，所以不一定能结束进程

kill -2 PID

# 强制结束进程 -9 ，注意：强制结束某个进程后，可能会造成进程数据丢失等问题！

kill -9 PID
```

## free 查看内存使用信息

### 参数介绍

```sh
-b ：以字节(bytes/B)为单位显示
-k ：以 KB 为单位显示
-m ：以 MB 为单位显示
-g ：以 GB 为单位显示
--tera ：以 TB 为单位显示
-h ：以人类易读的方式输出
--si ：以 1000 为单位转换，而不是 1024（1MB=1*1024KB 改成 1MB=1*1000KB）
-t ：显示内存总数行
-s 时间：每隔 X 秒输出一次（重复输出监视内存，使用 Ctrl+C 终止）
-c 次数：每隔 1 秒输出 X 次

# 更多的命令可以用 free --help 来查看。
```

### 使用示例

```sh
# 显示当前系统内存（默认 free = free -k，单位为 KB）

free

# 输出示例

             total       used       free     shared    buffers     cached

Mem: 250872 237752 13120 0 34620 70520
-/+ buffers/cache: 132612 118260
Swap: 643064 1744 641320

# 以单位 B/KB/MB/GB/TG 显示当前系统内存

free -b / free -k / free -m / free -g / free --tera

# 以人类易读的方式 显示当前系统内存

free -h

# 输出示例

            total       used       free     shared    buffers     cached
Mem:        244M        232M        12M       0B        33M         68M
-/+ buffers/cache:      129M        115M
Swap:       627M        1.7M        626M

# 以 1000 为单位转换并使用 MB 为单位 显示当前系统内存（1MB=1*1024KB 改成 1MB=1*1000KB）

free -m --si

# 每隔 3 秒并使用 MB 为单位 显示一次当前系统内存

free -ms 3

# 每隔 1 秒并使用 MB 为单位 显示 5 次当前系统内存

free -mc 5

# 每隔 2 秒并使用 MB 为单位 总共显示 6 次当前系统内存

free -m -c 6-s 2

# 更多的命令可以用 free --help 来查看。
```

表头解释：

```sh
# 说明示例

             total       used         free     shared    buffers     cached
Mem:          244M        232M        12M       0B        33M         69M
-/+ buffers/cache:        129M        115M
Swap:         627M        1.7M        626M

# Mem 行，表示物理内存统计

total :系统 总物理内存
used :系统 已分配物理内存（但非全部都在使用，包含 buffers 好 cached）
free :系统 未分配物理内存
shared :系统 共享内存，一般都是 0
buffers :系统 分配但未使用的 buffers 数量
cached :系统 分配但未使用的 cached 数量

# -/+ buffers/cache 行，表示物理内存的缓存统计

used :系统 实际使用的内存

# user= Mem 行 used-buffers-cached（232-33-69=130，因单位转换问题 所以会有一点差距）

free :系统 实际可用的内存

# free= Mem 行 free+buffers+cached（12+33+69=114，因单位转换问题 所以会有一点差距）

# 所以我们看系统的真实 使用/剩余内存 只需要看这一行即可！

# Swap 行，表示硬盘的交换分区（虚拟内存）统计

total :系统 总虚拟内存
used :系统 已分配虚拟内存
free :系统 未分配虚拟内存

# KVM 虚拟化的 VPS，可以用这个教程手动添加 SWAP 虚拟内存：https://doub.io/linux-jc7/
```

## date 查看/设置 系统时间

### 参数介绍

```sh
-d ：以指定的时间格式显示时间
-f ：显示 DATE FILE 文件中的每行时间（我也不懂）
-r ：显示文件/文件夹最后修改时间
-s ：设置系统时间
-u ：显示 UTC 时间

# 时间格式

%%-显示字符%
%a -星期几的缩写(Sun..Sat)
%A -星期几的完整名称（Sunday...Saturday）
%b -月份的缩写(Jan..Dec)
%B -月份的完整名称(January..December)
%c -日期与时间。只输入 date 指令也会显示同样的结果
%C -世纪(年份除 100 后去整)[00-99]
%d -日期(以 01-31 来表示)。
%D -日期(含年月日)。
%e -一个月的第几天(1..31)
%F -日期，同%Y-%m-%d
%g -年份(yy)
%G -年份(yyyy)
%h -同%b
%H -小时(00..23)
%I -小时(01..12)
%j -一年的第几天(001..366)
%k -小时(0..23)
%l -小时(1..12)
%m -月份(01..12)
%M -分钟(00..59)
%n -换行
%N -纳秒(000000000..999999999)
%p - AM or PM
%P - am or pm
%r -12 小时制时间(hh:mm:ss [AP]M)
%R -24 小时制时间(hh:mm)
%s -从 00:00:001970-01-01 UTC 开始的秒数
%S -秒(00..60)
%t -制表符
%T -24 小时制时间(hh:mm:ss)
%u -一周的第几天(1..7);1 表示星期一
%U -一年的第几周，周日为每周的第一天(00..53)
%V -一年的第几周，周一为每周的第一天(01..53)
%w -一周的第几天(0..6);0 代表周日
%W -一年的第几周，周一为每周的第一天(00..53)
%x -日期(mm/dd/yy)
%X -时间(%H:%M:%S)
%y -年份(00..99)
%Y -年份(1970…)
%z - RFC-2822 风格数字格式时区(-0500)
%Z -时区(e.g., EDT),无法确定时区则为空

# 更多的命令可以用 date --help 来查看。
```

### 使用示例

```sh
# 显示 当前系统时间

date

# 输出：Wed Apr 5 12:38:44 CST 2017

# 显示当前系统的 UTC 时间

date -u

# 输出：Wed Apr 5 04:30:06 UTC 2017

# 显示 log.txt 文件的最后修改时间

date -r log.txt

# 显示 当前日期的年份

date +%Y

# 输出：2017

# 显示 当前日期的月份

date +%m

# 输出：4

# 显示 各种格式类型的日期

date +%D

# 输出：04/05/17

date +%Y-%m-%d

# 输出：2017-04-05

date +%m/%d/%y

# 输出：04/05/17

date +%m/%d/%Y

# 输出：04/05/2017

# 显示 Unix 时间戳

date +%s

# 输出：1491367399

# 显示一个完整的时间（年、月、日、小时、分钟、秒钟、周几 时区）

date "+%Y-%m-%d %H:%I:%S %u %Z"

# 输出：2017-04-05 12:12:15 3 CST

# 设置 系统时间（年、月、日）

date -s "2017-04-05"

# 设置 系统时间（小时、分钟、秒钟）

date -s "10:29:05"

# 设置 系统时间（年、月、日、小时、分钟、秒钟）

date -s "2017-04-05 10:29:05"

# 更多的命令可以用 date --help 来查看。
```

## chmod 修改 文件/文件夹 权限

### 参数介绍

```sh
-c :只输出被改变权限的文件信息
-f :当 chmod 不能改变文件模式时，不通知文件的用户
-R :可递归遍历子目录，把修改应到目录下所有文件和子目录
-v :无论修改是否成功，输出每个文件的信息

# 操作符号：

+:添加某个权限。
-:取消某个权限。
=:赋予给定权限并取消其他所有权限（如果有的话）。

# 权限设置字母：

r :可读
w :可写
x :可执行
X :只有目标文件对某些用户是可执行的或该目标文件是目录时才追加 x 属性
s :在文件执行时把进程的属主或组 ID 置为该文件的文件属主。方式“u ＋ s”设置文件的用户 ID 位，“g ＋ s”设置组 ID 位
t :保存程序的文本到交换设备上
u :当前用户的权限
g :当前用户同组的权限
o :其他用户的权限

# 权限设定数字：

# 数字表示的属性含义：

0：表示没有权限
1：表示可执行权限
2：表示可写权限
4：表示可读权限

# 然后将其相加，所以数字属性的格式应为 3 个从 0 到 7 的八进制数，其顺序是（u）（g）（o）。

# 如果想让某个文件的属主有“读/写”二种权限，需要把 4（可读）+2（可写）＝ 6（读/写）。

# 更多的命令可以用 chmod --help 来查看。
```

### 使用示例

```sh
# 当需要运行 可执行的脚本或者程序（比如 Go 语言编写的软件）的时候，需要赋予执行权限

chmod +x ssr.sh

# 赋予 log.txt 文件可读权限

chmod 444 log.txt

# 赋予 /ver/log 文件夹 可读、可写权限

chmod 666 log.txt

# 赋予 /home/www 文件夹 可读、可写、可执行权限

chmod 777 log.txt

# 赋予 /home/www 文件夹极其所有子目录和文件 可读、可写、可执行权限

chmod -R 777 log.txt

# 更多的命令可以用 chmod --help 来查看。
```

## uname 获取操作系统信息

### 参数介绍

```sh
-a：显示全部信息
-m：显示系统位数
-n：显示主机名称
-r：显示操作系统的发行编号
-s：显示操作系统的名称
-v：显示操作系统的版本
-p：输出处理器类型或"unknown"
-i：输出硬件平台或"unknown"
-o：输出操作系统名称

# 更多的命令可以用 uname --help 来查看。
```

### 使用示例

```sh
$ uname #在使用 uname 的时候，相当于是使用 uname -s
Linux
$ uname -a
Linux doub.io 2.6.32-042stab120.6#1 SMP Thu Oct 27 16:59:03 MSK 2016 i686 GNU/Linux
$ uname -m #输出一般是 64 位: x86_64 / 32 位: i386 或分支 i686
i686
$ uname -n
doub.io
$ uname -r
2.6.32-042stab120.6
$ uname -s
Linux
$ uname -v
#1 SMP Thu Oct 27 16:59:03 MSK 2016
$ uname -p
unknown
$ uname -i
unknown
$ uname -o
GNU/Linux
```

## cron 定时任务

crontab 是用来定期执行程序的命令。

当安装完成操作系统之后，默认便会启动此任务调度命令。

cron 命令每分钟会定期检查是否有要执行的工作，如果有要执行的工作便会自动执行该工作。

而 linux 任务调度的工作主要分为以下两类：

1. 系统执行的工作：系统周期性所要执行的工作，如备份系统数据、清理缓存
2. 个人执行的工作：某个用户定期要做的工作，例如每隔 10 分钟检查邮件服务器是否有新信，这些工作可由每个用户自行设置

### 参数说明

```sh
crontab [ -u user ] file
# 或者
crontab [ -u user ] { -l | -r | -e }

# crontab 是用来让使用者在固定时间或固定间隔执行程序之用，换句话说，也就是类似使用者的时程表。
-u : user 是指设定指定 user 的时程表，这个前提是你必须要有其权限(比如说是 root)才能够指定他人的时程表。如果不使用 -u user 的话，就是表示设定自己的时程表。
-e : 执行文字编辑器来设定时程表，内定的文字编辑器是 VI，如果你想用别的文字编辑器，则请先设定 VISUAL 环境变数来指定使用那个文字编辑器(比如说 setenv VISUAL joe)
-r : 删除目前的时程表
-l : 列出目前的时程表
```

### 日程表格式

```sh
f1 f2 f3 f4 f5 program
```

- 其中 f1 是表示分钟，f2 表示小时，f3 表示一个月份中的第几日，f4 表示月份，f5 表示一个星期中的第几天。program 表示要执行的程序。
- 当 f1 为 _ 时表示每分钟都要执行 program，f2 为 _ 时表示每小时都要执行程序，其馀类推
- 当 f1 为 a-b 时表示从第 a 分钟到第 b 分钟这段时间内要执行，f2 为 a-b 时表示从第 a 到第 b 小时都要执行，其馀类推
- 当 f1 为 _/n 时表示每 n 分钟个时间间隔执行一次，f2 为 _/n 表示每 n 小时个时间间隔执行一次，其馀类推
- 当 f1 为 a, b, c,... 时表示第 a, b, c,... 分钟要执行，f2 为 a, b, c,... 时表示第 a, b, c...个小时要执行，其馀类推

### 使用示例

```sh
# 每月每天每小时的第 0 分钟执行一次 /bin/ls
0 * * * * /bin/ls

# 在 12 月内, 每天的早上 6 点到 12 点，每隔 3 个小时 0 分钟执行一次 /usr/bin/backup
0 6-12/3 * 12 * /usr/bin/backup

# 周一到周五每天下午 5:00 寄一封信给 alex@domain.name
0 17 * * 1-5 mail -s "hi" alex@domain.name < /tmp/maildata

# 每月每天的午夜 0 点 20 分, 2 点 20 分, 4 点 20 分....执行 echo "haha"
20 0-23/2 * * * echo "haha"

# 意思是每两个小时重启一次apache
0 */2 * * * /sbin/service httpd restart

# 意思是每天7：50开启ssh服务
50 7 * * * /sbin/service sshd start

# 意思是每天22：50关闭ssh服务
50 22 * * * /sbin/service sshd stop

# 每月1号和15号检查/home 磁盘
0 0 1,15 * * fsck /home

# 每小时的第一分执行 /home/bruce/backup这个文件
1 * * * * /home/bruce/backup

# 每周一至周五3点钟，在目录/home中，查找文件名为*.xxx的文件，并删除4天前的文件。
00 03 * * 1-5 find /home "*.xxx" -mtime +4 -exec rm {} \;

# 意思是每月的1、11、21、31日是的6：30执行一次ls命令
30 6 */10 * * ls
```
