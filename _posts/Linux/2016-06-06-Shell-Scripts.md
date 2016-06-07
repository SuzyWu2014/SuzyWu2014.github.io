---
layout: post
title: "Learn Shell Scripts"
description: ""
category: Linux
tags:  [Bash]
---
{% include JB/setup %}

# How shell scripts are interpreted 
1. from top to bottom, from left to right 
2. spaces/[tab] spaces between commands, options and arguments are ignored
3. blank lines are ignored
4. run the command when a `[Enter] (CR)` is read
5. `#` comments
6. `\[Enter]` break long line

# Script Header 

```bash
#!/bin/bash -->  ~/.bashrc as env configuration, and use bash to run 
# Program:
#      Describe the functionality of the script 
# History: --> modification history
# 2016/06/06  Author First release

# set env variable
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH

```

# Different ways to run scripts(source, sh script, ./script)

## sh script 
!(https://github.com/SuzyWu2014/SuzyWu2014.github.io/tree/master/assets/images/non-source.gif)
script run as a child bash, when it completes, all the variable defined in the child bash will be removed.

## source script 
!(https://github.com/SuzyWu2014/SuzyWu2014.github.io/tree/master/assets/images/source.gif)
run in parent bash, all variable remains.

# Condition

## test command 
| test options | meaning |
|--------------|---------|
|1. 关於某个档名的『文件类型』判断|如 test -e filename 表示存在否|
|-e  | 该『档名』是否存在？(常用)|
|-f  | 该『档名』是否存在且为文件(file)？(常用)|
|-d  | 该『档名』是否存在且为目录(directory)？(常用)|
|-b  | 该『档名』是否存在且为一个 block device 装置？|
|-c  | 该『档名』是否存在且为一个 character device 装置？|
|-S  | 该『档名』是否存在且为一个 Socket 文件？|
|-p  | 该『档名』是否存在且为一个 FIFO (pipe) 文件？|
|-L  | 该『档名』是否存在且为一个连结档？|

## if condition

# Note
+ Shell scripts 不适用于处理大量数值运算。 因为其速度较慢，且使用的CPU资源较多，造成主机资源的分配不良。