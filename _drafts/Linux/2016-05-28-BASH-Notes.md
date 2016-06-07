---
layout: post
title: "Learn BASH"
description: ""
category: Linux
tags:  [BASH]
---
{% include JB/setup %}

# Bash Shell 功能

## History:

+ `~/.bash_history` 记录的是前一次登录以前所运行的命令, 而这一次登陆所使用的命令都被缓存在内存中， 当你成功注销系统后，该命令记忆才会记录到 `.bash_history` 中。

## Alias:

alias lm='ls -al'

## env variables 

```bash
范例一：列出目前的 shell 环境下的所有环境变量与其内容。
[root@www ~]# env
HOSTNAME=www.vbird.tsai    <== 这部主机的主机名
TERM=xterm                 <== 这个终端机使用的环境是什么类型
SHELL=/bin/bash            <== 目前这个环境下，使用的 Shell 是哪一个程序？
HISTSIZE=1000              <== 『记录命令的笔数』在 CentOS 默认可记录 1000 笔
USER=root                  <== 使用者的名称啊！
LS_COLORS=no=00:fi=00:di=00;34:ln=00;36:pi=40;33:so=00;35:bd=40;33;01:cd=40;33;01:
or=01;05;37;41:mi=01;05;37;41:ex=00;32:*.cmd=00;32:*.exe=00;32:*.com=00;32:*.btm=0
0;32:*.bat=00;32:*.sh=00;32:*.csh=00;32:*.tar=00;31:*.tgz=00;31:*.arj=00;31:*.taz=
00;31:*.lzh=00;31:*.zip=00;31:*.z=00;31:*.Z=00;31:*.gz=00;31:*.bz2=00;31:*.bz=00;3
1:*.tz=00;31:*.rpm=00;31:*.cpio=00;31:*.jpg=00;35:*.gif=00;35:*.bmp=00;35:*.xbm=00
;35:*.xpm=00;35:*.png=00;35:*.tif=00;35: <== 一些颜色显示
MAIL=/var/spool/mail/root  <== 这个用户所取用的 mailbox 位置
PATH=/sbin:/usr/sbin:/bin:/usr/bin:/usr/X11R6/bin:/usr/local/bin:/usr/local/sbin:
/root/bin                  <== 不再多讲啊！是运行文件命令搜寻路径
INPUTRC=/etc/inputrc       <== 与键盘按键功能有关。可以配置特殊按键！
PWD=/root                  <== 目前用户所在的工作目录 (利用 pwd 取出！)
LANG=en_US                 <== 这个与语系有关，底下会再介绍！
HOME=/root                 <== 这个用户的家目录啊！
_=/bin/env                 <== 上一次使用的命令的最后一个参数(或命令本身)
```


