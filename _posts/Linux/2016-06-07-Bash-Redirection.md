---
layout: post
title: "Bash - Redirection"
description: ""
category: Linux
tags:  [BASH]
---
{% include JB/setup %}

# standard output VS standard error output
+ 标准输入　　(stdin) ：代码为 0 ，使用 `<` 或 `<<` ；
+ 标准输出　　(stdout)：代码为 1 ，使用 `>` 或 `>>` ；
+ 标准错误输出(stderr)：代码为 2 ，使用 `2>` 或 `2>>` ;

+ `1>` ：以覆盖的方法将『正确的数据』输出到指定的文件或装置上；
+ `1>>`：以累加的方法将『正确的数据』输出到指定的文件或装置上；
+ `2>` ：以覆盖的方法将『错误的数据』输出到指定的文件或装置上；
+ `2>>`：以累加的方法将『错误的数据』输出到指定的文件或装置上；

# `/dev/null` 垃圾桶黑洞装置与特殊写
```bash
将错误的数据丢弃，屏幕上显示正确的数据
[dmtsai@www ~]$ find /home -name .bashrc 2> /dev/null
/home/dmtsai/.bashrc  <==只有 stdout 会显示到屏幕上， stderr 被丢弃了
```

将正确与错误数据通通写入同一个文件

```bash
将命令的数据全部写入名为 list 的文件中
[dmtsai@www ~]$ find /home -name .bashrc > list 2> list  <==错误
[dmtsai@www ~]$ find /home -name .bashrc > list 2>&1     <==正确
[dmtsai@www ~]$ find /home -name .bashrc &> list         <==正确
```

# standard input ： `<` 与 `<<`
```bash
用 stdin 取代键盘的输入以创建新文件的简单流程
[root@www ~]# cat > catfile < ~/.bashrc
[root@www ~]# ll catfile ~/.bashrc
-rw-r--r-- 1 root root 194 Sep 26 13:36 /root/.bashrc
-rw-r--r-- 1 root root 194 Feb  6 18:29 catfile
# 注意看，这两个文件的大小会一模一样！几乎像是使用 cp 来复制一般！

[root@www ~]# cat > catfile << "eof"
> This is a test.
> OK now stop
> eof  <==输入这关键词，立刻就结束而不需要输入 [ctrl]+d

[root@www ~]# cat catfile
This is a test.
OK now stop     <==只有这两行，不会存在关键词那一行！

```

# 命令运行的判断依据： `;` , `&&`, `||`
+ `cmd ; cmd` (不考虑命令相关性的连续命令下达)
+ `$?` (命令回传值) 与 `&&` 或 `||`
    * 若前一个命令运行的结果为正确，在 Linux 底下会回传一个 `$? = 0 `的值

# Reference

[鸟哥的Linux私房菜 － 数据流重导向 (Redirection)](http://vbird.dic.ksu.edu.tw/linux_basic/0320bash_5.php)




