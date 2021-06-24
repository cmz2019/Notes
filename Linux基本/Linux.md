# 一、系统状态查看工具

## 1、使用 Linux

### 超级用户

**root 用户（超级用户）**

+ root 不受权限的制约，可随意修改和删除文件
+ root 用户误删重要文件可能带来严重后果

**创建新用户**

由 root 用户创建（useradd 命令），用户信息存放 /etc/passwd 文件中，包括用户名和用户 ID，以及 Home 目录， 登录 shell

登录 shell：一般为 bash，也可以选其他 shell，其他系统程序，甚至自设计的程序。

+ 用户可以从普通终端或者网络虚拟终端登录进入系统

### 使用 Linux

**Shell 的提示符**

登录成功后出现shell提示符

+ `$` Bourne Shell 系列 (sh, ksh, bash)

+ `#` 当前用户为超级用户 root（操作时要小心）

**系统的使用**

+ 出现shell提示符以后就可输入系统命令
+ 与Windows不同的是组成命令的英文字母大小写敏感

## 2、几个基本的Linux命令

### man

作用：查阅手册

**使用方法**：

+ `man name`
+ `man section name`，章节编号：1 命令，2 系统调用，3 库函数，5 配置文件
+ `man -k regexp`，列出关键字（keyword）与正则表达式 regexp 匹配的手册项目录

**手册内容**：

+ 列出基本功能和语法
+ 对于 C 语言的函数调用，列出头文件和链接函数库
+ 功能说明
+ SEE ALSO：有关的其它项目的名字和章节号

### date

作用：读取系统日期和时间

**使用方法**：

+ `date`，读取系统日期和时间，Wed Nov 7 21:09:16 CST 2018

+ 可以根据需要定制使出格式

  + `date "+%Y-%m-%d %H:%M:%S Day %j"`，2018-11-07 21:09:54 Day 311

  + `date "+%s"`，1541596457

  + > 311指的是今天是今年的第311天
    >
    > 格式控制字符串：第一个字符必须为 + 号，%Y 代表年号， %m 代表月份，%M 代表分钟。
    >
    > %s 秒坐标（从 UTC1970 开始），常用于计算时间间隔

+ 通过 NTP 协议校对系统事件，`ntpdate`

  + `ntpdate 0.pool.ntp.org`，设置时间，必须 root 用户
  + `ntpdate -q 0.pool.ntp.org`，查询时间，普通用户也可以

### cal

作用：打印日历

**使用方法**：

+ `cal`，打印当前月份的日历
+ `cal year`，打印某年的日历
+ `cal month year`，打印某年某月的日历

### bc

作用：计算器

+ 基本计算器功能
+ 支持变量、函数，条件和循环等编程功能（类似 C 语言的小编程语言）
+ 可以进行任意精度的计算

**精度**：

+ 缺省精度
  + `bc`，缺省精度为小数点后0位
  + `bc -l`，缺省精度为小数点后20位
+ 可以通过设置 scale 自行决定精度（小数点位数）
  + scale=10000
  + s(1.0)

### passwd

作用：更换口令

+ 普通用户

  + 使用 `passwd` 命令更改自己的口令，更改前系统先验证原来的口令

+ 超级用户 root

  + 修改口令之前不验证旧的口令

  + 可修改自己的口令，还可强迫设置其它用户口令

  + 命令 `passwd liu`

    > 将用户 liu 的口令强迫设置为某一已知口令
    >
    > 超级用户无法读取其它用户的口令
    >
    > 当普通用户忘记口令时，可请求超级用户强设口令

  + 修改超级用户 root 的口令时要特别注意

## 3、了解系统状态

### who

作用：确定有谁再系统中

**使用方法**：

+ `who`，列出当前已登录入系统的用户信息

  ![image-20210623162639555](https://gitee.com/cmz2000/album/raw/master/image/image-20210623162639555.png)

  + 第一列：用户名；第二列：终端设备的设备文件名
  + 设备在文件系统中有一个文件名（同普通磁盘文件不同的是文件类型属于特殊文件），设备文件一般放于目录 /dev 下

+ `tty`，打印出当前终端的设备文件名

+ `who am i`，列出当前终端上的登录用户信息

+ `whoami`，仅列出当前终端上的登录用户名

### uptime

作用：已开机时间（年龄）

**使用方法**：

+ `uptime`
  + 系统自启动后到现在的运行时间
  + 当前登录入系统的用户数
  + 近期1分钟，5分钟，15分钟内系统CPU的负载
    + 平均调度队列长度

![image-20210623162832054](https://gitee.com/cmz2000/album/raw/master/image/image-20210623162832054.png)

### top

作用：列出资源占用排名靠前的进程

![image-20210623163005443](https://gitee.com/cmz2000/album/raw/master/image/image-20210623163005443.png)

+ VIRT 进程逻辑地址空间大小（virtual）
+ RES 驻留内存数（Resident），也就是占用物理内存数
+ SHR 与其他进程共享的内存数（share）
+ %CPU 占用CPU百分比，%MEM 占用内存百分比
+ TIME+ 占用的CPU时间  

### ps

作用：查阅进程状态（process status），实际上就是将内核中进程状态信息有选择地打印出来

**选项**：

+ 用于控制列表的行数(进程范围)和列数(每进程列出的属性内容)
+ 无选项：只列出在当前终端上启动的进程，列出的项目有：PID，TTY，TIME，COMMAND
+ `-e` 选项：列出系统中所有的进程（进程范围）
+ `-f` 选项：以 full 格式列出每一个进程（控制列的数目）
+ `-l` 选项：以 long 格式列出每一个进程（控制列的数目）

![image-20210623163657438](https://gitee.com/cmz2000/album/raw/master/image/image-20210623163657438.png)

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210623163722895.png" alt="image-20210623163722895" style="zoom:67%;" />

### free

作用：了解内存使用情况

![image-20210623164354170](https://gitee.com/cmz2000/album/raw/master/image/image-20210623164354170.png)

+ 内存总量 4GB, 空闲 309MB。Linux 为提高效率，利用程序暂时不用的内存，缓冲读写过的磁盘信息。当前有 717MB 的 buffer / cache。
+ 不计 buffers / cache,系统有实际可利用资源 734MB
+ 打印了磁盘 Swap 区的使用情况

### vmstat

作用：了解系统负载

![image-20210623165029470](https://gitee.com/cmz2000/album/raw/master/image/image-20210623165029470.png)

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210623164724206.png" alt="image-20210623164724206" style="zoom:67%;" />

# 二、文本文件处理

## 1、读取文件内容

### more / less

作用：逐屏显示文件

**使用方法**：

+ `more shudu.c` 指定一个文件
+ `more *.[ch]` 指定多个文件
+ `ls -l | more` 指定0个文件
+ `less shudu.c` 

**more**

满屏后，显示--more--或--more--(15%)，可以使用 more 命令：

![image-20210621144815902](https://gitee.com/cmz2000/album/raw/master/image/image-20210621144815902.png)

**less**

less is more

+ 回退浏览的功能更强
+ 可直接使键盘的上下箭头键，或者 j, k，类似 vi 的光标定位键，以及 `PgUp`, `PgDn`, `Home`, `End`键
+ 有的 Unix 系统不提供 less 命令，但是可利用 more 命令的增强功能

### cat 与 od

作用：列出文件内容

`cat` concatenate：串结，文本格式打印 （选项- n：行号）
`od` octal dump：逐字节打印（`-c`, `-t c`, `-t x1`, `-t d1`, `-t u1` 选项）

**举例**

+ `cat tryl.c` 命令行参数 1 个
+ `cat –n shudu.c` 显示行号
+ `cat tryl.c tryx.c try.h` 命令行参数 3 个
+ `cat >try` 命令行参数 =0 个，从 stdin 获取数据，直到 `ctrl-d`
+ `cat tryl.c try2.c try.h > trysrc`
+ `cat makefile *.[ch] > src`
+ `od –t x1 x.dat` 以十六进制打印文件 x.dat 各字节
+ `od –t x1 x.dat | more` 以十六进制打印文件 x.dat 各字节
+ `od –c /bin/bash` 逐字符方式打印文件，遇到不可打印字符打印编码
+ `echo abcdABCD | od –t x1` 十六进制显示字符的 ASCII 码

### head 与 tail

作用：显示文件的头部或者尾部

默认只选择 10 行， `-n` 选项可以选择行数

**举例**

+ `head –n 15 ab.c` 显示文件 ab.c 中前15行
+ `head –n 23 a.c b.c c.c | more` 显示三个文件各自的前23行共显示69行
+ `tail –n 40 liu.mail`
+ `head –n -20 msg.c` 除去文件尾部20行其余均算“头”
+ `tail –n +20 msg.c` 除去文件头部20行其余均算作“尾”
+ `tail -f debug.txt` 实时打印文件尾部被追加的内容（选项 -f : forever）
  `netstat -s -p tcp | head`
  `ls -s | sort | head –n 20`

### tee

作用：三通。将从标准输入 stdin 得到的数据抄送到标准输出 stdout 显示，同时存入磁盘文件中。

**举例**

+ `./myap | tee myap.log`

### wc

作用：字计数（word count）

+ 列出文件中一共有多少行，有多少个单词，多少字符
+ 当指定的文件数大于1时，最后还列出一个合计
+ 常用选项 `-l`：只列出行计数

**举例**

+ `wc sum.c` （1个文件）
+ `wc x.c makefile stat.sh` （多个文件）
+ `wc -l *.c makefile start.sh` （只列出文件分别有多少行）
+ `ps -ef | wc -l` （0个）
+ `ps -ef | grep liang | wc -l` （0个）
+ `who | wc -l` （0个）

### sort

作用：对文件内容排序

+ `-n` 选项（Numberic）：对于数字按照算术值大小排序，而不是按照字符串比较规则，例如123与67
+ 可以选择行中某一部分作为排序关键字
+ 选择升序或降序
+ 字符串比较时对字母是否区分大小写
+ 内排序外排序等算法参数选择（当数据量较大时，性能调优）

**举例**

+ `sort telno > telno1`
+ `ls -s | sort | tail –10`
+ `ls -s | sort -n | tail –10`

### tr

作用：翻译字符

`tr string1 string2`：把标准输入拷贝到标准输出，string1 中出现的字符替换为 string2 中的对应字符

**举例**

+ `cat telnos | tr UVX uvx`
+ 例：用 [] 指定一个集合
  `cat report | tr '[a-z]' '[A-Z]'` 将小写字母改为大写字母
+ 例：用 \ 加三个八进制数字（类似 C 语言）表示一字符
  `cat file1 | tr % '\012'` 将%改为换行符，注意不要漏掉必需的单引号

### uniq

作用：筛选文件中的重复行（重复的行：紧邻的两行内容相同）

**用法**：

+ `uniq options`
+ `uniq options input-file`
+ `uniq options input-file output-file`

**选项**：

+ `-u`（uniqe）只保留没有重复的行

+ `-d` （duplicated）只保留有重复的行（但只打印一次）

  没有以上两个选项，则打印没有重复的行和有重复的行（但只打印一次)

+ `-c` （count）计数同样的行出现几次

## 2、正则表达式

### 功能

描述一个字符串模式

作用范围：

+ 字符串匹配操作和替换操作
+ 举例：Linux 中的 vi more grep yacc lex awk sed
+ 其他：Visual Studio，Word 等文本编辑器

**注意：**

正则表达式规则与文件名通配符规则不同

+ 正则表达式规则用于文本处理的场合
+ 文件名匹配规则用于文件处理的场合

### 单字符正则表达式

+ 长的正则表达式由单字符正则表达式构成的

+ 非特殊字符与其自身匹配

  如：正则表达式 a 与字符串 a 匹配，b 与 b，/ 与 /

#### 转义字符（\\）

`\. \* \$ \^ \[ \\`

正则表达式 \\* 与字符串 * 匹配，与字符串 \\* 不匹配

转义字符后除以上六种之外的不该出现其他字符，例如：不该出现 \u，这样的组合被视为undefined（未定义的），后出的软件有可能会有特殊的解释

#### 圆点（·）

匹配任意单字符

#### 星号（\*）

单字符正则表达式后跟 \*，匹配此单字符正则表达式的 0 次或任意多次出现

#### 锚点（\^与\$）

+ \$ 在尾部时有特殊意义，否则与其自身匹配
  + 例：123\$ 匹配文件中行尾的123，不在行尾的123字符不匹配
  + 例：​\$123 与字符串 \$123 匹配
  + 例：\.\$ 匹配行尾的任意字符
+ \^ 在首部时有特殊意义，否则与其自身匹配
  + 例：\^printf 匹配行首的 printf 字符串，不在行首的 printf 串不匹配
  + 例：Hel\^lo 与字符串 Hel\^lo 匹配
  + 例：在 vi 中使用：`10,50s/^    //g`，删除10-50行的每行行首的4个空格

#### 集合（\[与\]）

##### 基本用法

+ 在一对方括号之间的字符为集合的内容，如：单字符正则表达式 \[abcd\] 与 a 或 b, c, d 匹配
+ 圆点、星号、反斜线在方括号内时，代表它们自己，如：\[\\\*\.\] 可匹配 3 个单字符

##### 用减号 \- 定义一个区间

+ 如 \[a\-d\]  \[A\-Z\]  \[a\-zA\-Z0\-9\]
+ `[][]`集合含左右中括号两个字符
+ 减号在最后，则失去表示区间的意义
  + \[ad\-\] 只与3个字符匹配

##### 用 \^ 表示补集

+ \^ 在开头,则表示与集合内字符之外的任意字符匹配
  + `[^a-z]` 匹配任一非小写字母
  + `[^][]` 匹配任一非中括号字符
+ \^ 不在开头,则失去其表示补集的意义
  + `[a-z^]` 能匹配 27 个单字符

##### 举例

![image-20210621154836478](https://gitee.com/cmz2000/album/raw/master/image/image-20210621154836478.png)

![image-20210621154904429](https://gitee.com/cmz2000/album/raw/master/image/image-20210621154904429.png)

### 正则表达式扩展

+ 表示**分组**：圆括号 `()`

+ 表示逻辑运算：表示逻辑 “**或**” 的符号 `|`
  \(xy\)\* 可匹配空字符串, xy, xyxy, xyxyxy
  \(pink\|green\) 与 pink 或 green 匹配

+ 重复次数定义：与星号地位类似的 `+` 和 `?`，限定重复次数 `\{m,n\}`

  + \* 号表示它左边的单字符正则表达式的0次或多次重复
  + \+ 号表示1次或多次： `[0-9]+` 匹配长度至少为1数字串
  + ? 表示0次或一次： `a?` 匹配零个或一个a
  + 限定重复次数 `\{m,n\}`，例如： `[1-9][0-9]\{6,8\}` 匹配7-9位数字，首位非0

+ 命名的预定义集合

  `[[:xdigit:]]` 表示十六进制数字，`\d` 数字，`\D` 非数字

+ 比 \^ 和 \$ 更灵活的**锚点**定义

  例如：寻找一个数字串，但是要求这个数字串不许出现在“合计”两个字之后（“合计”可作为锚点）。

### 文本行筛选 grep

Global regular expression print

作用：在文件中查找字符串

**语法**：`grep 模式 文件名列表`，注意，文件名列表中只包含一个文件时，结果不会显示文件名，只有包含多个文件才显示每个文件名

**举例**

+ `grep O_RDWR *.h`
+ `ps -ef | grep liang`
+ `ls -l / | grep '^d' | wc –l`
+ `grep '[0-9]*' shudu.c`
+ `grep '[0-9][0-9]*' shudu.c`

**grep选项**

+ `-n` 显示时每行前面显示行号
+ `-v` 显示所有不包含模式的行
+ `-i` 字母比较时忽略字母的大小写
+ `-E` 使用扩展的正则表达式

**举例**

+ `grep -n main *.c`
  查找含有正则表达式 main 的行，并打印行号
  当文件数超过一个时，除了输出行号，还输出文件名
+ `grep -v '[Dd]isable' dev.stat>dev.active`
  取消文件中所有含有指定模式的行，生成新文件
+ `grep -i richard telnos`
  在文件中检索字符串 richard，不顾字母的大小写

### 流编辑 sed

**用法**

+ `sed '命令' 文件名列表`
+ `sed -e '命令1' -e '命令2' -e '命令3' 文件名列表`
+ `sed -f 命令文件 文件名列表`

**举例**

+ `tail -f pppd.log | sed 's/145\.37\.23\.26/桥西/g'`，把 IP 地址 145.37.23.26 替换为 “桥西”，注意不要忘记转义符号 `\`

+ `tail -f pppd.log | sed -f sed.cmd`

  其中 sed.cmd 文件内容为

  ```shell
  s/145\.37\.23\.26/桥西/g
  s/102\.157\.23\.109/柳荫街/g
  s/145\.37\.123\.57/大山子/g
  ```

+ `cat pm25.txt | sed 's/\[[^][]*]//g'`，删除所有被中括号包围的字段

**正则表达式替换**

![image-20210621162545167](https://gitee.com/cmz2000/album/raw/master/image/image-20210621162545167.png)

### 复杂筛选及加工 awk

作用：逐行扫描进行文本处理

**用法**

+ `awk '程序' 文件名列表`
+ `awk -f 程序文件名 文件名列表`

程序   条件 { 动作 }

一段程序：awk 自动对每行文本执行条件判断，满足条件执行动作（内置循环）

多段程序：允许多段程序，程序间用空格或分号隔开

**处理方式**

+ **（行）**输入文件的每行作为一个 **“记录”** ，变量 NR 就是行号

+ **（列）**每行用空格分隔开的部分，叫做记录的 **“域”**

  内置变量 \$1 是第1域内容，依次，​\$2 是第2域内容，…… 特别的，​\$0 指的是整个这一行的内容

+ awk 的处理为：**符合条件的行，执行相应的动作**

**awk描述的条件**

![image-20210621190055677](https://gitee.com/cmz2000/album/raw/master/image/image-20210621190055677.png)

**awk描述的动作**

![image-20210621190107147](https://gitee.com/cmz2000/album/raw/master/image/image-20210621190107147.png)

**举例**

![image-20210621190147914](https://gitee.com/cmz2000/album/raw/master/image/image-20210621190147914.png)

## 3、文件比对

### 文件内容比对

#### cmp

作用：两文件逐字节比较

**用法**：`cmp file1 file2`

**功能**

+ 逐字节比较两个文件是否**完全相同**
+ 两个文件完全相同时，不给出任何提示
+ 两个文件不同时，打印出第一个不同之处
+ 在 Windows 中有类似的命令 COMP

#### md5sum / sha1sum

+ 使用 MD5 算法（散列函数）根据文件内容生成16字节 hash 值，比较 hash 值是否相同，就可断定两文件内容是否完全相同
+ 使用 SHA-1 算法的命令名为 sha1sum（20字节 hash 值）
+ 常用于数据完整性（Data Integrity）验证和判断位于网络不同机器上的**两个文件内容是否相同**
+ 其他散列函数也可以用来完成这一任务：sha512sum

### 求文本文件版本间差异

#### diff

作用：求出两个文件的差别

**用法**

+ `diff file1 file2`
+ `diff -u file1 file2`

**功能**

+ 比较两个版本的文本文件，以寻找两者间差别
+ 输出格式 normal, unified **(-u)**
+ normal 格式：列出一个如何将 file1 转化为 file2 的指令
  + 这些指令有 **a** (Add), **c** (Change) 和 **d** (Delete)
  + 指令字母左边的行号是 file1 的行号，右面是 file2 的行号
  + 列出内容时，大于号后边的内容是需要在 file1 文件中增加的内容；小于号后边的内容是需从 file1 中删除的内容
+ normal 格式文件转化指令如下表

![image-20210621193011646](https://gitee.com/cmz2000/album/raw/master/image/image-20210621193011646.png)

**diff -u举例**

![image-20210621193413002](https://gitee.com/cmz2000/album/raw/master/image/image-20210621193413002.png)

## 4、vi 编辑器

### 编辑状态和光标移动

#### 用户的偏好设置

+ 用户 HOME 目录下的文件 \.exrc，记作 `$HOME/.exrc`（每用户一份，用户独立设置）

  ```shell
  set number		# 每行左边显示行号
  set tabstop=4	# 制表符位置为4格对齐
  ```

+ 用运行时检查偏好设置
  `:set`

#### vi 的两种工作状态

+ **命令状态：键盘输入解释为命令**
  + vi 一启动就进入命令方式，键盘输入解释为命令
  + 一般按键无回显
  + 以冒号可以引入行编辑的命令和查找命令
  + 编辑命令 i a 等，可以从命令状态转到文本状态
+ **文本状态**
  + 键盘输入解释为输入的文本
  + 可以输入多行，每输入完一行后按回车转入下一行
  + 正文输入时有回显
  + 输入完毕按键盘左上角的 Esc 键，返回到命令状态
+ **正文插入**
  + 命令 i 在当前字符前插入正文段，直至按 Esc 键 (insert)
  + 命令 a 在当前字符后插入正文段，直至按 Esc 键 (append)

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210621195017049.png" alt="image-20210621195017049" style="zoom:50%;" />

#### 光标移动

##### 单字符移动

+ 单字符移动（四个字母键盘上相邻的按键）
  + **h** 光标左移一列
  + **j** 光标下移一行
  + **k** 光标上移一行
  + **L** 光标右移一列
  + 一般可以直接使用键盘上的方向键代替这四个字母
+ 命令前加一整数，表示这个命令连续执行多少遍
  + 5h 光标左移5列
  + 6j 光标下移6行
  + 23k 光标上移23行
  + 10L 光标右移10列

注意：在 vi 命令状态下的按键命令没有回显

##### 翻页

+ `Ctrl-b` 向后翻页（Backward）
+ `Ctrl-f` 向前翻页（Forward）
+ 一般可以用 PgDn 键代替 Ctrl-f，用 PgUp 键代替 Ctrl-b
+ 也可以使用下面的命令
  + 6Ctrl-f 向前翻6页
  + 15Ctrl-b 向后翻15页

##### 光标行内快速移动

+ **行尾行首**
  + 将光标移至当前行首 `^`
  + 将光标移至当前行尾 `$`
+ **移动一个单词**
  + 移到右一个单词 `w`
  + 移到左一个单词 `b`
  + 也可以使用 6w 5b 命令

##### 光标移动到指定行

+ 移到指定的行
  + `:476` 将光标定位于第476行
  + `:1` 将光标定位于第1行（文件首）
  + `:$` 将光标定位于文件尾
+ 在描述行号时可以使用
  + 圆点 `.` 代表当前行号
  + `$` 代表最后一行的行号
+ 括号配对 `%`
  + 把光标移到一个花括号（或圆括号，或方括号）上，按 `%` 键，则光标自动定位到与它配对的那一个括号  

### 查找、编辑及存盘

#### 删除和替换

+ 删除字符
  + 删除当前字符的命令 `x`
  + 命令 5x 删除从当前光标开始的5个字符  
+ 删除行
  + 删除当前行的命令 `dd`
  + 命令 3dd 删除从当前行开始的3行  
+ 替换光标处字符 `r`
  + `ra` 命令将当前光标处字符替换为 a
  + 将当前光标处开始的三个字符依次替换为 abc，则需要按命令 `rarbrc` 

#### 取消和重复

+ 取消上一次的编辑操作(undo) `u`
  + 如：误删了一段正文，用 u 命令可撤销删除
  + 如：把文件中的所有abc字符串替换成xyz字符串， 用 u 命令可撤销替换
+ 重复上一次的编辑操作 `.`
  + 按圆点键，可以重复上一次的编辑操作
  + 例如：按 3dd 命令删除了三行，然后按圆点键就再删除三行，接着连续按圆点键，每按一次删三行  

#### 文件操作命令

存盘退出：

+ `ZZ`
+ `:wq<CR>`

存盘不退出：`:w<CR>`

不存盘退出：`:q!<CR>`

读入文件 xyz.c 插入到当前行之下：`:r xyz.c<CR>`

写文件，把第50行至文件尾的内容写到文件 file1 中：

+ `:50,$w file1<CR>`
+ `:50,$w! file1<CR>`，强制覆盖

#### 剪贴板

删除，并拷贝到剪贴板：

+ `:10,50d<CR>`，删除第10-50行
+ `:1,.d<CR>`，删除文件首至当前行的部分
+ `:.,$d<CR>`，删除当前行到文件尾

不删除，拷贝到剪贴板（yank）：`:10,50y<CR>`

粘贴剪贴板信息（paste）：`p`

#### 块操作：复制与删除

复制：`:5,10co56<CR>`，复制第5-10行到第56行之下

移动：`:8,34m78<CR>`，移动第8-34行到第78行之下

#### 行合并、刷屏和状态显示

两行合并（Join） `J`，当前行下面的行合并到当前行

刷新屏幕显示（load） `Ctrl-l`

状态显示 `Ctrl-g`，在屏幕最下面一行列出正在编辑的文件的名字，总行数，当前行号，文件是否被修改过等信息

#### 模式查找与替换

用正则表达式来描述一个字符串模式

**查找命令**

+ 格式为 `/pattern`
+ 例如：`/[0-9][0-9*]`

**继续查找命令**

+ `n` 向下查找下一个next
+ `N` 向上查找下一个
+ 循环式搜索（向下搜索时遇到文件尾则回到文件头继续搜索）

**替换命令（substitution）**

+ 格式 `:n1,n2s/pattern/string/g`
+ 例如
  + `:1,50s/abc/xyz/`，不带 g 则每一行只替换第一个符合模式的
  + `:1,50s/abc/xyz/g`，带 g 会替换一行中所有符合模式的
  + `:50,75s/^/    /`，第50-75行右移4列
  + `:50,75s/^    //`，第50-75行左移4列
  + `:1,$s/ *$//`，消除尾部多余的空格
  + `:1,$s/a[i]/b[j]/g`，小心陷阱：不能把 a[i] 替换为 b[j]，要带转义符 `\`
  + `:1,$/a*b/x+y/g`，小心陷阱：不能把 a*b 替换为 x+y

**转义符**

+ 将 a[i]\*b[j] 替换为 x[k]\*y[n] 的命令：`:1,$s/a\[i]\*b\[j]/x[k]*y[n]/g`

+ 将 buf\.len/1000 替为 buffer\.size/1024 的命令：`:1,$s/buf\.len\/1000/buffer.size\/1024/g`，模式串和替换字符串中的斜线前加转义符\  以区别于替换命令格式中所必须的斜线

  `:1,$s:buf\.len/1000:buffer.size/1024:g`，s后面以冒号取代斜线，分界符就换为冒号，避免对斜线的转义，其他分隔符（如 `^`）同理，`:1,$s^http://www\.myvdo\.com/a/b/c/index\.html^https://www.xyvdo.com/index.html^g`

**假“死机”问题**

+ 现象

  vi 编辑结束后执行存盘操作，结果导致屏幕卡死，输入任何信息都不再有显示（死机，终端死机）

+ 原因

  vi 编辑结束后按下 `Ctrl-S`，因为 Windows 编辑器一般设置 `Ctrl-S` 热键的动作为 Save，但 Linux 却进入流量控制状态

+ 解决方法

  按下 `Ctrl-Q` 键后流量控制解除  

**意外中止问题**

+ 现象

  vi 编辑结束后存盘，程序意外中止，编辑成果丢失，文件内容未发生变化

+ 原因

  vi 存盘命令 `Shift-ZZ`，误操作为 `Ctrl-ZZ`，而 `Ctrl-Z` 按键导致当前运行进程被挂起（suspend），暂停运行（但进程尚在，处于 Stopped 状态）

+ 解决方法

  调用 bash 的作业管理机制，恢复运行被 Stopped 的进程

  + `jobs`，列表当前被 Stopped 的进程有哪些
  + `fg %1`，或 `%1`，将1号作业恢复到前台（foreground）运行

# 三、文件系统管理

## 1、文件通配符

### 文件通配符规则

#### 星号 \*

+ 匹配任意长度的文件名字符串（包括空字符串）
+ 点字符 (\.) 作为文件名或路径名分量的**第一个字符**时，必须**显式**匹配
+ 斜线 (/) 也必须**显式**匹配
+ 例：`*file` 匹配 file, makefile，不匹配 \.profile 文件，`try*c` 匹配 try1\.c, try\.c, try\.basic

#### 问号 ?

+ 匹配任一单字符

#### 方括号 \[ \]

+ 匹配括号内任一字符，也可以用减号指定一个范围
+ 例：`[A-Z]*`, `*.[ch]`, `[Mm]akefile`

#### 波浪线 \~

（Bash特有的）

+ `~`，当前用户的主目录(home)
+ `~kuan`，用户kuan的主目录(home)

#### 注意

![image-20210623171452772](https://gitee.com/cmz2000/album/raw/master/image/image-20210623171452772.png)

### 处理过程

#### shell 和 kernel

+ **shell**
  + shell 是一个用户态进程，如 /bin/bash
  + 对用户提供命令行界面
  + **启动**其他应用程序（ap）使用操作系统核心提供的功能：包括系统命令和用户编写的程序
+ **kernel：操作系统核心**
  + 管理系统资源（包括内存，磁盘等）运行在核心态
  + 通过软中断方式对用户态进程提供系统调用接口

![image-20210623171655566](https://gitee.com/cmz2000/album/raw/master/image/image-20210623171655566.png)

#### shell文件名通配符处理

文件名通配符的处理由 shell 完成，分以下三步：

+ 在 shell 提示符下，从键盘输入命令，被 shell 接受
+ shell 对所键入内容作若干加工处理，其中含有对文件通配符的展开工作（文件名生成），生成结果命令
+ 执行前面生成的结果命令

#### 举例

**设当前目录下只有 try\.c, zap\.c, arc\.c 三文件**

+ 键入内容 `cat *.c`
+ 实际执行 `cat arc.c try.c zap.c`（按字典序）
+ 对命令 cat 来说,指定了3个文件

`grep a*.c try.c` **与** `grep 'a*.c' try.c` **的区别**

+ 设当前目录下有 a1\.c 和 a2\.c
+ 前者实际执行 `grep a1.c a2.c try.c`，在 a2\.c 和 try\.c 中查找正则表达式 a1\.c
+ 后者在 try\.c 文件中查找正则表达式 a\*\.c

**键入命令时的简化输入**

+ 手工键入 `vi m*e`，实际执行 `vi makefile`
+ 手工键入 `cd *sna*`，实际执行 `cd configure-IBM-sna-network.d`

## 2、文件和目录的管理

### 列出文件目录

#### ls

作用：文件名列表

**选项**：

+ `-F`，Flag

  + 若列出的是目录，就在名字后面缀以斜线 /
  + 若列出的是可执行文件，就在名字后面缀以星号 \*
  + 若列出的是符号连接文件，就在名字后面缀以符号 @
  + 若列出的是普通文件，则名字面后无任何标记

+ `-l`，长格式列表

  + `-rwxr-x--x 1 liang stud 519 Jul 5 15:02 arg`

  + 第1列第1字符为文件属性

    > `-` 普通文件，`d` 目录文件(Dir)，`l` 符号连接文件(Link)，`b` 块设备文件(Block)，`c` 字符设备文件(Char)，`p` 命名管道文件(Pipe)

  + 第1列2-10字符为文件访问权限（rwx，读、写、可执行），分别为文件所有者、同组用户、其他用户的权限

  + 第2列是文件 link 数，涉及到此文件的目录项数

+ `-h`，human-readable，以便于人阅读的方式打印数值（例如：1K 234M 2G）

+ `-d`，directory，当 ls 的参数是目录时，不像默认的情况那样列出目录下的文件，而是列出目录自身的信息

+ `-a`，all，列出文件名首字符为圆点的文件（默认情况下这些文件不列出，经常会用来保存用户的偏好设置信息或保存某些软件的状态信息）。

+ `-A`，功能与 `-a` 相同，除了不列出 \. 和 \.\.

+ `-s`，size，列出文件占用的磁盘空间

+ `-i`，inode，列出文件的 i 结点号

### 文件的复制与删除

#### cp

作用：拷贝文件

**使用方法**：

+ `cp file1 file2`
+ `cp file1 file2 … filen dir`，其中 file1, file2, …, filen 为文件名，dir 为**已有目录**名。
+ 第二种格式中：dir 必须已经存在并且是一个目录
  第一种格式中：file2 不存在，则创建；file2 存在且是文件，则覆盖；file2 存在且是目录，则按格式二处理

**选项**：

+ `-u`，update，增量拷贝，便于备份目录
  + 根据文件的时戳，不拷贝相同的或者过时的版本的文件，以提高速度
  + 若 dir1 和 dir2 不慎颠倒位置，不会出现灾难性后果
+ `-r`，若给出的源文件是一目录文件，此时 cp 将递归复制该目录下所有的子目录和文件。此时目标文件必须为一个目录名。
+ `-v`，verbose，cp 命令将告诉用户正在做什么。很多 Linux 命令都带有具有相同意义的 `-v` 选项。

**举例**：

![image-20210623183434049](https://gitee.com/cmz2000/album/raw/master/image/image-20210623183434049.png)

![image-20210623185902666](https://gitee.com/cmz2000/album/raw/master/image/image-20210623185902666.png)

#### mv

作用：移动文件

**使用方法**：

+ `mv file1 file2`
+ `mv file1 file2 … filen dir`
+ `mv dir1 dir2`

**功能**：

+ 使用 mv 命令可以将文件和目录改名
+ 可以将文件和子目录从一个目录移动到另一个目录
+ `mv dir1 dir2` 两种执行情况（同文件系统，不同文件系统）

#### rm

作用：删除文件

**使用方法**：

+ `rm file1 file2 … filen`

**选项**：

+ `-r`，递归地（Recursive）删除实参表中的目录，也就是删除一整棵目录树。
+ `-i`，每删除一个文件前需要操作员确认（Inform）
+ `-f`，强迫删除（Force）。只读文件也被删除并且无提示，无操作权限的文件强制删除也不能删掉。
+ 注意：正在运行的可执行程序文件不能被删除

**显式区分命令选项和处理对象**

![image-20210623184411866](https://gitee.com/cmz2000/album/raw/master/image/image-20210623184411866.png)

### 目录管理

#### pwd

作用：打印当前工作目录，print working directory

#### cd

作用：改变当前工作目录，Change Directory

#### mkdir

作用：创建目录

例如：`mkdir sun/work.d`

`mkdir` 除创建目录外，系统自动建立文件 \. 与 \.\.

**选项**：

+ `-p`，自动创建路径中不存在的目录。例如：`mkdir -p database/2019/09/04/log`

#### rmdir

作用：删除目录

例如：`rmdir sun/work.d`

要求被删除的目录除 \. 与 \.\. 外无其他文件或目录，否则应该使用命令 `rm -r sun/work.d` 来删除

### 目录遍历

#### find

作用：遍历目录树。find 命令从指定的查找范围开始，递归地查找子目录，凡满足**条件**的文件或目录，执行规定的**动作**。

**举例**：

+ `find verl.d ver2.d -name '*.c' -print`
  范围：当前目录的子目录 ver1\.d 和 ver2\.d
  条件：与名字 \*\.c 匹配。注：\*\.c 应当用引号括起
  动作：把查找到的文件的路径名打印出来

**特点**：

+ 递归式查找，提供了一种遍历目录树的手段
+ find 命令提供的灵活性：在“动作”中可以指定任何命令（也包括用户自己编写的处理程序），使得 find 成为一个任意处理命令可以借用来进行目录遍历的壳（类似 awk 对文本的逐行扫描， find 对目录森林中的文件和目录逐个扫描）

**条件选项**：

+ `-name wildcard`，文件名与 wildcard 匹配。注意：
  + 使用正则表达式时必需的引号
  + 这里的 “文件名” **仅指路径名的最后一部分**
  + 对通配符的解释由 find 完成
+ `-regex pattern`，**整个路径名**与正则表达式 pattern 匹配
+ `-type`，`f` 普通文件，`d` 目录，`l` 符号链接文件，`c` 字符设备文件，`b` 块设备文件，`p` 管道文件
+ `-maxdepth number`，指定搜索深度为 number
+ `-size ±n单位`，指定文件大小（大于+，等于，小于-），单位有 c（字符），b （块， 512 字节），k(1024)，M，G，默认为 b
+ `-mtime ±ndays`，文件最近修改时间
+ `-newer file`，文件最近修改时间比 file 的还晚
+ 可以用`() –o !`等表示多条件的 “与”，“或”，“非”

**动作选项**：

+ `-print`，打印查找的文件的路径名
+ `-exec`
  + 对查找到的目标执行某一命令
  + 在 `-exec` 及随后的分号之间的内容作为一条命令
  + 在这命令的命令参数中，`{}` 代表遍历到的目标文件的路径名
+ `-ok`，与 `-exec` 类似，只是对符合条件的目标执行命令前需要经过操作员确认

##### 举例

+ `find . -type d -print`，从当前目录开始查找，寻找所有目录，打印路径名。结果：按层次列出当前的目录结构。

+ `find / -name 'stud*' -type d -print`，指定了两个条件：名字与 stud\* 匹配，类型为目录。两个条件逻辑 “与”，必须同时符合这两个条件。

+ `find / -type f -mtime -10 -print`，从根目录开始检索最近10天之内曾经修改过的普通磁盘文件（目录不算）

+ `find . ! -type d -links +2 -print`，从当前目录开始检索 link 数大于2的非目录文件条件 “非” 用 `！`。注意：`!` 与 `-type` 之间必须保留一空格。

+ `find ~ -size +100k \( -name core -o -name '*.tmp' \) -print`，从主目录开始寻找大于 100KB 的名叫 core 或有 \.tmp 后缀的文件

  + 使用了两条件 “或” `-o`及组合 `()` (圆括号)

  + 不要遗漏了所必需的引号，反斜线和空格，尤其是圆括号前和圆括号后。圆括号是 shell 的特殊字符

  + 其他写法

    `find / -size +100k '(' -name core -o -name \*.tmp ')' -print`

    `find / -size +100k \( -name core –o -name \*.tmp ')' -print`

+ `find /lib /usr -name 'libc*.so' -exec ls -lh {} \;`

  + `-exec` 及随后的分号之间的内容作为一条命令执行
  + shell 中分号有特殊含义，前面加反斜线 \\
  + `{}` 代表遍历时所查到的符合条件的路径名。注意，两花括号间无空格，之后的空格不可省略

+ 利用 find 的递归式遍历目录的功能在文件中搜寻字符串，`find src -name \*.c -exec grep -n -- --help {} /dev/null \;`，在目录 src 中所有 \.c 文件中查找 \-\-help 字符串

### 批量处理文件

![image-20210623194146398](https://gitee.com/cmz2000/album/raw/master/image/image-20210623194146398.png)

#### xargs

`find src -name \*.c -exec grep -n -- --help {} /dev/null \;`，如果能把 `find src -name \*.c –print` 生成的文件名列表追加在 `grep -n -- --help filelist` 命令的后面就可以了。

命令 `xargs` 可以用来完成这个工作：`find src -name \*.c –print | xargs grep -n -- --help` 

`xargs` 命令把标准输入追加到参数表后面，也就是上述 `grep…` 的后面，再作为一个命令来执行。这样利用 `find` 精确筛选，利用 `grep` 批量处理文件，提高效率。

##### 构造参数列表并运行命令

+ 解决 shell 文件名生成时，因为文件太多，缓冲区空间受限而文件名展开失败的问题

  `rm -f *.dat`，文件名 \*\.dat 展开失败，可以使用下面的命令 `ls | grep ".dat$" | xargs rm -f`

+ find 命中目录名因删除目录导致目录遍历过程遇到麻烦

  `find . -name CVS –exec rm –rf {} \;` 改为：`find . -name CVS -print | xargs rm -rf` 

### 打包与压缩

#### tar

作用：文件归档。tar命令最早为顺序访问的磁带机设备而设计的（Tape ARchive，磁带归档），用于保留和恢复磁带上的文件。

**语法**：`tar ctxv[f device] [options] file-list`

**选项**：

+ **选项第一字母指定要执行的操作，是必需的**
  + `c`，create，创建新磁带。从头开始写，以前存于磁带上的数据会被覆盖掉
  + `t`，table，列表。磁带上的文件名列表，当不指定文件名时，将列出所有的文件。
  + `x`，eXtract，抽取。从磁带中抽取指定的文件，不指定文件名则抽取所有文件。
+ **除功能字母外的其他选项**：
  + `v`，verbose，冗长。每处理一个文件，就打印出文件的文件名，并在该名前冠以功能字母。
  + `f`，file，指定设备文件名。
  + `z`，采用压缩格式（gzip 算法）
  + `j`，采用压缩格式（bzip2 算法）
+ **options 选项**：
  + `-C`，大写，解压到指定目录下，前提是目录必须存在

##### 举例

+ `tar cvf /dev/rtc0 .`，将当前目录树备份到设 /dev/rct0 中，圆点目录是当前目录

+ `tar tvf /dev/rct0`，查看磁带设备 /dev/rct0 上的文件目录

+ `tar xvf /dev/rct0`，查看磁带设备 /dev/rct0 上的文件目录

+ `tar cvf my.tar *.[ch] makefile`，指定普通文件代替设备文件，将多个文件或目录树存储成一个文件。

  + 这一命令的**危险误操作**：`tar cvf *.[ch] makefile`，漏掉了功能字母 `f` 必需的 “设备文件名” ，按照 shell 对文件名的展开规则，会覆盖掉现存的排位第一的文件，`tar cvf a1.c a2.c ab.h makefile`

+ 目录打包，设 work 是一个有多个层次的子目录

  + `tar cvf work.tar work`
  + `tar cvzf work.tar.gz work`，gzip 压缩算法，对 C 源程序体积为原来的 20%
  + `tar cvjf work.tar.bz2 work`，bzip2 压缩算法，对 C 源程序 17%，执行时间三倍

  查看归档文件中的文件目录：`tar tvf work.tar.gz`

  从归档文件中恢复目录树：`tar xvf work.tar.gz [-C mydir]`

  注意：文件名后缀 \.tar, \.tar\.gz, \.tar\.bz2 仅仅是惯例，不是系统级强制要求

## 3、命令获取信息

### 配置文件

+ 存储配置信息或者偏好配置信息
+ 分为**系统级偏好设置**和**用户级偏好设置**， 例如 bash 的 `/etc/profile` 和 `~/.bash_profile`
+ 提供了灵活性（同一个程序文件因用户不同读取的配置文件不同而表现不同），变更这些信息不很方便， 一般不需要变化的配置信息或选项信息存入配置文件，持久化存储

### 环境变量

+ 命令 `env` 可以打印当前的环境变量。例如：LANG(语言), HOME(主目录), PATH(可执行文件的查找路径), CLASSPATH(类库), CVSROOT
+ 环境变量值的获取与设置：C 语言有库函数 `getenv()` 

## 4、文件系统

![image-20210623205344386](https://gitee.com/cmz2000/album/raw/master/image/image-20210623205344386.png)

### 创建与安装

#### mkfs

作用：创建文件系统（磁盘格式化）

`mkfs /dev/sdb`，在块设备文件 /dev/sdb 上创建文件系统（make filesystem）

#### mount

作用：安装子文件系统

`mount /dev/sdb /mnt`，/mnt 可以是任一个事先建好的空目录名。此后，操作子目录 /mnt 就是对子文件系统的访问，从文件或目录名看不出和其它根文件系统的对象有什么区别

`mount`，不带参数，列出当前已安装的所有子文件系统

#### umount

作用：卸载已安装的子文件系统

`umount /dev/sdb` 

### 存储结构

这部分建议去学习操作系统，这里不展开

#### stat

作用：读取 i 节点信息

`stat test.c`

![image-20210623205945247](https://gitee.com/cmz2000/album/raw/master/image/image-20210623205945247.png)

## 5、文件和目录的权限

![image-20210623210535588](https://gitee.com/cmz2000/album/raw/master/image/image-20210623210535588.png)

### 相关命令

#### chmod

作用：修改权限

+ **字母形式**

  + `chmod [ugoa] [+-=] [rwxst] 文件名表`
  + u -- user，文件主的权限；g -- group，同组用户；o -- other，其他用户；a -- all，上述所有；t -- sticky；s -- SUID
  + 例子：`chmod u+rw *`，`chmod go-rwx *.[ch]`，`chmod a+x batch`，`chmod u=rx try2`

+ **数字形式**（八进制）

  + 例：`chmod 644 xyz1 xyz2`

  ![image-20210623211133017](https://gitee.com/cmz2000/album/raw/master/image/image-20210623211133017.png)

注意：只允许文件主和超级用户修改文件权限

#### umask

作用：决定文件/目录的初始权限

+ 用 vi 新建文件
+ 用输出重定向创建文件
+ 创建新目录

**使用方法**：

+ `umask`，打印当前的 umask 值
+ `umask 022`，将 umask 值设置为八进制的022

![image-20210623211612067](https://gitee.com/cmz2000/album/raw/master/image/image-20210623211612067.png)

# 四、bash及脚本程序设计

## 1、shell的基本机制

### 历史与别名

#### history

内部命令 `history`，查看历史表（文件 `$HOME/.bash_history`）

查看先前键入的命令

![image-20210624092235143](https://gitee.com/cmz2000/album/raw/master/image/image-20210624092235143.png)

#### 历史替换

人机交互时直接使用上下箭头键

`!!`，引用上一命令

`! str`，以 str 开头的最近使用过的命令，如：`!v`，`!m`，`!.`

#### alias

![image-20210624092641493](https://gitee.com/cmz2000/album/raw/master/image/image-20210624092641493.png)

#### TAB键补全

+ 每行的首个单词：TAB键补全搜索 `$PATH` 下的命令
+ 行中的其它单词：TAB键补全当前目录下的文件名  

### 输入重定向

#### <filename

![image-20210624093047738](https://gitee.com/cmz2000/album/raw/master/image/image-20210624093047738.png)

#### <<word

![image-20210624093128168](https://gitee.com/cmz2000/album/raw/master/image/image-20210624093128168.png)

#### <<<word

![image-20210624093142677](https://gitee.com/cmz2000/album/raw/master/image/image-20210624093142677.png)

### 输出重定向与管道

![image-20210624093204481](https://gitee.com/cmz2000/album/raw/master/image/image-20210624093204481.png)

#### stdout输出重定向

![image-20210624093448704](https://gitee.com/cmz2000/album/raw/master/image/image-20210624093448704.png)

#### stderr输出重定向

![image-20210624093515146](https://gitee.com/cmz2000/album/raw/master/image/image-20210624093515146.png)

#### 举例

![image-20210624093715805](https://gitee.com/cmz2000/album/raw/master/image/image-20210624093715805.png)

![image-20210624093729828](https://gitee.com/cmz2000/album/raw/master/image/image-20210624093729828.png)

### 管道

![image-20210624093804186](https://gitee.com/cmz2000/album/raw/master/image/image-20210624093804186.png)

## 2、变量

### 赋值与引用

![image-20210624094811543](https://gitee.com/cmz2000/album/raw/master/image/image-20210624094811543.png)

![image-20210624094838149](https://gitee.com/cmz2000/album/raw/master/image/image-20210624094838149.png)

## 3、替换

Shell 的替换工作：**先替换命令行再执行命令**

### 文件名生成

![image-20210624095229462](https://gitee.com/cmz2000/album/raw/master/image/image-20210624095229462.png)

### 命令替换

#### 反撇号 \`

![image-20210624095319370](https://gitee.com/cmz2000/album/raw/master/image/image-20210624095319370.png)

#### \$\(\)格式

![image-20210624095428221](https://gitee.com/cmz2000/album/raw/master/image/image-20210624095428221.png)

### 位置参数

![image-20210624095546592](https://gitee.com/cmz2000/album/raw/master/image/image-20210624095546592.png)



## 4、元字符和转义

## 5、条件

条件判断的依据：判定一条命令是否执行成功。方法：命令执行的返回码，0表示成功，非0表示失败。可以把命令执行结束后的“返回码”理解为“出错代码”。

### 逻辑判断

#### 内部命令 \$?

![image-20210624101038229](https://gitee.com/cmz2000/album/raw/master/image/image-20210624101038229.png)

#### 复合逻辑

![image-20210624101125811](https://gitee.com/cmz2000/album/raw/master/image/image-20210624101125811.png)

### test 和 方括号命令 \[

+ 命令 `/usr/bin/[`，要求其最后一个命令行参数必须为 `]`
+ 除此之外 `/usr/bin/[` 与 `/usr/bin/test` 功能相同
+ 注意：不要将方括号理解成一个词法符号，而是一个名字为 `[` 的命令
+ 举例
  `test -r /etc/motd`
  `[ -r /etc/motd ]`

#### 文件特性检测

![image-20210624102226812](https://gitee.com/cmz2000/album/raw/master/image/image-20210624102226812.png)

#### 比较

![image-20210624102852550](https://gitee.com/cmz2000/album/raw/master/image/image-20210624102852550.png)

![image-20210624103007751](https://gitee.com/cmz2000/album/raw/master/image/image-20210624103007751.png)

#### 复合条件

![image-20210624103025596](https://gitee.com/cmz2000/album/raw/master/image/image-20210624103025596.png)

### 命令组合 \{\} 与 \(\)

![image-20210624103226871](https://gitee.com/cmz2000/album/raw/master/image/image-20210624103226871.png)

![image-20210624103238293](https://gitee.com/cmz2000/album/raw/master/image/image-20210624103226871.png)

![image-20210624103311757](https://gitee.com/cmz2000/album/raw/master/image/image-20210624103311757.png)

### 条件分支

#### if

![image-20210624103423101](https://gitee.com/cmz2000/album/raw/master/image/image-20210624103423101.png)

##### 举例

![image-20210624103522258](https://gitee.com/cmz2000/album/raw/master/image/image-20210624103522258.png)

#### case

![image-20210624103535135](https://gitee.com/cmz2000/album/raw/master/image/image-20210624103535135.png)

## 6、循环

### expr

作用：算术运算、关系运算、逻辑运算、正则表达式运算

![image-20210624103928490](https://gitee.com/cmz2000/album/raw/master/image/image-20210624103928490.png)

![image-20210624104327870](https://gitee.com/cmz2000/album/raw/master/image/image-20210624104327870.png)

#### 正则表达式运算

![image-20210624104426585](https://gitee.com/cmz2000/album/raw/master/image/image-20210624104406013.png)

### 内部命令 eval

![image-20210624104459462](https://gitee.com/cmz2000/album/raw/master/image/image-20210624104459462.png)

### while 循环

![image-20210624104627820](https://gitee.com/cmz2000/album/raw/master/image/image-20210624104627820.png)

多个命令放到一行时，要用分号隔开

### for 循环

![image-20210624104746465](https://gitee.com/cmz2000/album/raw/master/image/image-20210624104746465.png)

## 7、函数

![image-20210624104814777](https://gitee.com/cmz2000/album/raw/master/image/image-20210624104814777.png)

# 五、重定向与管道

![image-20210624083231346](https://gitee.com/cmz2000/album/raw/master/image/image-20210624083231346.png)

![image-20210624083334698](https://gitee.com/cmz2000/album/raw/master/image/image-20210624083334698.png)

## 1、重定向

![image-20210624083609746](https://gitee.com/cmz2000/album/raw/master/image/image-20210624083609746.png)

## 2、管道

### 创建

![image-20210624083737990](https://gitee.com/cmz2000/album/raw/master/image/image-20210624083737990.png)

### 写

![image-20210624083756234](https://gitee.com/cmz2000/album/raw/master/image/image-20210624083756234.png)

### 读

![image-20210624083816776](https://gitee.com/cmz2000/album/raw/master/image/image-20210624083816776.png)

### 关闭

![image-20210624083830567](https://gitee.com/cmz2000/album/raw/master/image/image-20210624083830567.png)



### 举例

![image-20210624083917476](https://gitee.com/cmz2000/album/raw/master/image/image-20210624083917476.png)

![image-20210624084104800](https://gitee.com/cmz2000/album/raw/master/image/image-20210624084104800.png)

![image-20210624084118565](https://gitee.com/cmz2000/album/raw/master/image/image-20210624084118565.png)

![image-20210624084205246](https://gitee.com/cmz2000/album/raw/master/image/image-20210624084205246.png)

## 3、一些命令

### kill

作用：用于向进程发送一个信号

**使用方法**：`kill –signal PID-list`

**举例**：

![image-20210624084726950](https://gitee.com/cmz2000/album/raw/master/image/image-20210624084726950.png)

![image-20210624084811305](https://gitee.com/cmz2000/album/raw/master/image/image-20210624084811305.png)



## 4、一些系统调用

### signal

作用：进程对信号的处理

进程对到达的信号可以在下列处理中选取一种

+ 设置为缺省处理方式（大部分处理是程序中止，有的会产生core文件）。

  `signal(SIGINT, SIG_DFL)`，参数1为信号宏，参数2为 SIG_DFL

+ 信号被忽略：`signal(SIGINT,SIG_IGN)`，在执行了这个调用后，进程就不再收到 SIGINT 信号（该属性会被子进程继承）

+ 信号被捕捉：`signal(SIGINT,user_func)`，用户事先注册好一个函数，当信号发生后就去执行这一函数。

### kill

作用：发送信号

**使用方法**：`int kill(int pid, int sig)`，返回值：0 成功，-1 失败

![image-20210624085431899](https://gitee.com/cmz2000/album/raw/master/image/image-20210624085431899.png)

# 六、进程控制

## 1、一些系统调用

### fork

作用：创建新进程

**使用方法**：`int fork()`，对父进程返回子进程的 PID (>0)，对子进程返回值为 0，失败时返回 -1

### exec

![image-20210624091051583](https://gitee.com/cmz2000/album/raw/master/image/image-20210624091051583.png)

![image-20210624091102630](https://gitee.com/cmz2000/album/raw/master/image/image-20210624091102630.png)

### wait

![image-20210624091432426](https://gitee.com/cmz2000/album/raw/master/image/image-20210624091432426.png)

### system

![image-20210624091604961](https://gitee.com/cmz2000/album/raw/master/image/image-20210624091604961.png)



