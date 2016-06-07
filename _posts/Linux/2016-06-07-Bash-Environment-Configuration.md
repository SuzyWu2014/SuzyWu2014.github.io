---
layout: post
title: "Bash Environment Configuration"
description: ""
category: Linux
tags:  [Bash]
---
{% include JB/setup %}

# Overview
+ 路径与命令搜寻顺序: `type -a cmd`
+ bash 的进站与欢迎信息： `/etc/issue, /etc/motd`
+ 环境配置文件: `login, non-login shell, /etc/profile, ~/.bash_profile, source, ~/.bashrc`
+ 终端机的环境配置：` stty, set`
+ 通配符与特殊符号

# 路径与命令搜寻顺序 － to find out: `type -a cmd`
1. 以相对/绝对路径运行命令， 例如 `/bin/ls` or `./ls`
2. 由 alias 找到该命令来运行；
3. 由 bash 内建的 (builtin) 命令来运行；
4. 透过 $PATH 这个变量的顺序搜寻到的第一个命令来运行

# bash 的进站与欢迎信息： `/etc/issue`, `/etc/motd`

## `/etc/issue`
```bash 
ubuntu@test:~$ cat /etc/issue
Ubuntu 14.04.2 LTS \n \l
```

+ `\d` 本地端时间的日期；
+ `\l`显示第几个终端机接口；
+ `\m` 显示硬件的等级 (i386/i486/i586/i686...)；
+ `\n` 显示主机的网络名称；
+ `\o` 显示 domain name；
+ `\r` 操作系统的版本 (相当于 uname -r)
+ `\t` 显示本地端时间的时间；
+ `\s` 操作系统的名称；
+ `\v` 操作系统的版本。

### Note:
`/etc/issue.net`: for telnet

## `/etc/motd`: Custom information showed when users login.
```bash
[root@www ~]# vi /etc/motd
Hello everyone,
Our server will be maintained at 2009/02/28 0:00 ~ 24:00.
Please don't login server at that time. ^_^
```

# Bash Comfiguration Files 

## login vs non-login shell
+ login shell: need to login every time. usually needs username/password or keypair to login
  - `/etc/profile`: 系统整体的配置， 最好不要修改
  - `~/.bash_profile 或 ~/.bash_login 或 ~/.profile`：属于使用者个人配置，你要改自己的数据，就写入这里!
+ non-login shell: don't always need to login. 

### `/etc/profile` (only for login shell; for all users)
这个文件配置的变量主要有：
+ PATH：会依据 UID 决定 PATH 变量要不要含有 sbin 的系统命令目录；
+ MAIL：依据账号配置好使用者的 mailbox 到 /var/spool/mail/账号名；
+ USER：根据用户的账号配置此一变量内容；
+ HOSTNAME：依据主机的 hostname 命令决定此一变量内容；
+ HISTSIZE：历史命令记录笔数。CentOS 5.x 配置为 1000 ；

同时`/etc/profile` 还会去呼叫外部的配置文件：
+ `/etc/inputrc`: 
    * didn't run
    * /etc/profile 会主动的判断使用者有没有自定义输入的按键功能，如果没有的话， /etc/profile 就会决定配置『INPUTRC=/etc/inputrc』这个变量
    * 文件内容为 bash 的热键啦、[tab]要不要有声音啦等等的数据
    
+ `/etc/profile.d/*.sh`
    * 这个目录底下的文件规范了 bash 操作接口的颜色、 语系、ll 与 ls 命令的命令别名、vi 的命令别名、which 的命令别名等等
    * 如果你需要帮所有使用者配置一些共享的命令别名时， 可以在这个目录底下自行创建扩展名为 .sh 的文件，并将所需要的数据写入即可喔！
    
+ `/etc/sysconfig/i18n`
    * 这个文件是由 /etc/profile.d/lang.sh 呼叫进来的
    * 这也是我们决定 bash 默认使用何种语系的重要配置文件

### `~/.bash_profile` (for login shell)
在 `login shell` 的 bash 环境中，所读取的个人偏好配置文件其实主要有三个，依序分别是：
1. `~/.bash_profile`
2. `~/.bash_login`
3. `~/.profile`

Only one of the three will be read!

#### Login shell 读取流程
![](https://raw.githubusercontent.com/SuzyWu2014/SuzyWu2014.github.io/master/assets/images/bashrc_1.gif)
实线的的方向是主线流程，虚线的方向则是被呼叫的配置文件！从上面我们也可以清楚的知道，在 CentOS 的 login shell 环境下，最终被读取的配置文件是『 ~/.bashrc 』这个文件喔！所以，你当然可以将自己的偏好配置写入该文件即可。 

### `source`: 读入环境配置的命令

利用 source 或小数点 (.) 都可以将配置文件的内容读进来目前的 shell 环境中！
```bash 
[root@www ~]# source 配置文件档名

范例：将家目录的 ~/.bashrc 的配置读入目前的 bash 环境中
[root@www ~]# source ~/.bashrc  <==底下这两个命令是一样的！
[root@www ~]#  .  ~/.bashrc
```

### `~/.bashrc` (for non-login shell)
 In CentOS, it will call `/etc/bashrc`:
+ 依据不同的 UID 规范出 umask 的值；
+ 依据不同的 UID 规范出提示字符 (就是 PS1 变量)；
+ 呼叫 /etc/profile.d/*.sh 的配置

### Other configuration files 

+ `/etc/man.config`: 这的文件的内容『规范了使用 man 的时候， man page 的路径到哪里去寻找！』
    * 如果你是以 tarball 的方式来安装你的数据，那么你的 man page 可能会放置在 /usr/local/softpackage/man 里头，那个 softpackage 是你的套件名称， 这个时候你就得以手动的方式将该路径加到 /etc/man.config 里头，否则使用 man 的时候就会找不到相关的说明档啰。
    * 这个文件内最重要的其实是 MANPATH 这个变量配置. 我们搜寻 man page 时，会依据 MANPATH 的路径去分别搜寻啊！另外，要注意的是， 这个文件在各大不同版本 Linux distributions 中，檔名都不太相同，例如 CentOS 用的是 /etc/man.config ，而 SuSE 用的则是 /etc/manpath.config ， 可以利用 [tab] 按键来进行文件名的补齐啦！
+ `~/.bash_history`: related to `HISTFILESIZE`
+ `~/.bash_logout`: 这个文件则记录了『当我注销 bash 后，系统再帮我做完什么动作后才离开』的意思
 

# Terminal 的环境配置： stty, set

## stty 
```bash 
[root@www ~]# stty [-a]
选项与参数：
-a  ：将目前所有的 stty 参数列出来；

范例一：列出所有的按键与按键内容
[root@www ~]# stty -a
speed 38400 baud; rows 24; columns 80; line = 0;
intr = ^C; quit = ^\; erase = ^?; kill = ^U; eof = ^D; eol = <undef>; 
eol2 = <undef>; swtch = <undef>; start = ^Q; stop = ^S; susp = ^Z;
rprnt = ^R; werase = ^W; lnext = ^V; flush = ^O; min = 1; time = 0;
....(以下省略)....
```

+ `^` 表示 [Ctrl] 那个按键的意思。举例来说， `intr = ^C` 表示利用 [ctrl] + c 来达成的
+ eof   : End of file 的意思，代表『结束输入』。
+ erase : 向后删除字符，
+ intr  : 送出一个 interrupt (中断) 的讯号给目前正在 run 的程序；
+ kill  : 删除在目前命令列上的所有文字；
+ quit  : 送出一个 quit 的讯号给目前正在 run 的程序；
+ start : 在某个程序停止后，重新启动他的 output
+ stop  : 停止目前屏幕的输出；
+ susp  : 送出一个 terminal stop 的讯号给正在 run 的程序。

```bash 
如果你想要用 [ctrl]+h 来进行字符的删除，那么可以下达：

[root@www ~]# stty erase ^h
```

## set 
```bash 
[root@www ~]# set [-uvCHhmBx]
选项与参数：
-u  ：默认不激活。若激活后，当使用未配置变量时，会显示错误信息；
-v  ：默认不激活。若激活后，在信息被输出前，会先显示信息的原始内容；
-x  ：默认不激活。若激活后，在命令被运行前，会显示命令内容(前面有 ++ 符号)
-h  ：默认激活。与历史命令有关；
-H  ：默认激活。与历史命令有关；
-m  ：默认激活。与工作管理有关；
-B  ：默认激活。与刮号 [] 的作用有关；
-C  ：默认不激活。若使用 > 等，则若文件存在时，该文件不会被覆盖。

范例一：显示目前所有的 set 配置值
[root@www ~]# echo $-
himBH
# 那个 $- 变量内容就是 set 的所有配置啦！ bash 默认是 himBH 喔！

范例二：配置 "若使用未定义变量时，则显示错误信息" 
[root@www ~]# set -u
[root@www ~]# echo $vbirding
-bash: vbirding: unbound variable
# 默认情况下，未配置/未宣告 的变量都会是『空的』，不过，若配置 -u 参数，
# 那么当使用未配置的变量时，就会有问题啦！很多的 shell 都默认激活 -u 参数。
# 若要取消这个参数，输入 set +u 即可！

范例三：运行前，显示该命令内容。
[root@www ~]# set -x
[root@www ~]# echo $HOME
+ echo /root
/root
++ echo -ne '\033]0;root@www:~'
# 看见否？要输出的命令都会先被打印到屏幕上喔！前面会多出 + 的符号！
```

## `/etc/inputrc`
```bash 
[root@www ~]# cat /etc/inputrc
# do not bell on tab-completion
#set bell-style none

set meta-flag on
set input-meta on
set convert-meta off
set output-meta on
.....以下省略.....
```

## Bash 默认组合键

+ `Ctrl + C`    终止目前的命令
+ `Ctrl + D`    输入结束 (EOF)，例如邮件结束的时候；
+ `Ctrl + M`    就是 Enter 啦！
+ `Ctrl + S`    暂停屏幕的输出
+ `Ctrl + Q`    恢复屏幕的输出
+ `Ctrl + U`    在提示字符下，将整列命令删除
+ `Ctrl + Z `   『暂停』目前的命令

# 通配符与特殊符号  

## wildcard

+ `*`   代表『 0 个到无穷多个』任意字符
+ `?`  代表『一定有一个』任意字符
+ `[ ]` 同样代表『一定有一个在括号内』的字符(非任意字符)。例如 [abcd] 代表『一定有一个字符， 可能是 a, b, c, d 这四个任何一个』
+ `[ - ]`   若有减号在中括号内时，代表『在编码顺序内的所有字符』。例如 [0-9] 代表 0 到 9 之间的所有数字，因为数字的语系编码是连续的！
+ `[^ ]`    若中括号内的第一个字符为指数符号 (^) ，那表示『反向选择』，例如 [^abc] 代表 一定有一个字符，只要是非 a, b, c 的其他字符就接受的意思。

```bash 

[root@www ~]# LANG=C              <==由于与编码有关，先配置语系一下

范例一：找出 /etc/ 底下以 cron 为开头的档名
[root@www ~]# ll -d /etc/cron*    <==加上 -d 是为了仅显示目录而已

范例二：找出 /etc/ 底下文件名『刚好是五个字母』的文件名
[root@www ~]# ll -d /etc/?????    <==由于 ? 一定有一个，所以五个 ? 就对了

范例三：找出 /etc/ 底下文件名含有数字的文件名
[root@www ~]# ll -d /etc/*[0-9]*  <==记得中括号左右两边均需 *

范例四：找出 /etc/ 底下，档名开头非为小写字母的文件名：
[root@www ~]# ll -d /etc/[^a-z]*  <==注意中括号左边没有 *

范例五：将范例四找到的文件复制到 /tmp 中
[root@www ~]# cp -a /etc/[^a-z]* /tmp
```

## special character

+ `#`    批注符号：这个最常被使用在 script 当中，视为说明！在后的数据均不运行
+ `\`    跳脱符号：将『特殊字符或通配符』还原成一般字符
+ `|`    管线 (pipe)：分隔两个管线命令的界定(后两节介绍)；
+ `;`    连续命令下达分隔符：连续性命令的界定 (注意！与管线命令并不相同)
+ `~`    用户的家目录
+ `$`    取用变量前导符：亦即是变量之前需要加的变量取代值
+ `&`    工作控制 (job control)：将命令变成背景下工作
+ `!`   逻辑运算意义上的『非』 not 的意思！
+ `/`    目录符号：路径分隔的符号
+ `>, >>`   数据流重导向：输出导向，分别是『取代』与『累加』
+ `<, <<`   数据流重导向：输入导向 (这两个留待下节介绍)
+ `' '` 单引号，不具有变量置换的功能
+ `" "` 具有变量置换的功能！
+ `` `` 两个『 ` 』中间为可以先运行的命令，亦可使用 $( )
+ `( )` 在中间为子 shell 的起始与结束
+ `{ }` 在中间为命令区块的组合！


# Reference

[第十一章、认识与学习 BASH](http://vbird.dic.ksu.edu.tw/linux_basic/0320bash_4.php)

