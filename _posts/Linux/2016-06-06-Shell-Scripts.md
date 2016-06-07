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
![](https://raw.githubusercontent.com/SuzyWu2014/SuzyWu2014.github.io/master/assets/images/non-source.gif)
script run as a child bash, when it completes, all the variable defined in the child bash will be removed.

## source script 
![](https://raw.githubusercontent.com/SuzyWu2014/SuzyWu2014.github.io/master/assets/images/source.gif)
run in parent bash, all variable remains.

# Condition

## test command 
--------------------------------------------------------------------------------------
1. 关於某个档名的『文件类型』判断，如 `test -e filename` 表示存在否
--------------------------------------------------------------------------------------
`-e`  该『档名』是否存在？(常用)
`-f`  该『档名』是否存在且为文件(file)？(常用)
`-d`  该『档名』是否存在且为目录(directory)？(常用)
`-b`  该『档名』是否存在且为一个 block device 装置？
`-c`  该『档名』是否存在且为一个 character device 装置？
`-S`  该『档名』是否存在且为一个 Socket 文件？
`-p`  该『档名』是否存在且为一个 FIFO (pipe) 文件？
`-L`  该『档名』是否存在且为一个连结档？
--------------------------------------------------------------------------------------
2. 关於文件的权限侦测，如 test -r filename 表示可读否 (但 root 权限常有例外)
--------------------------------------------------------------------------------------
-r  侦测该档名是否存在且具有『可读』的权限？
-w  侦测该档名是否存在且具有『可写』的权限？
-x  侦测该档名是否存在且具有『可运行』的权限？
-u  侦测该档名是否存在且具有『SUID』的属性？
-g  侦测该档名是否存在且具有『SGID』的属性？
-k  侦测该档名是否存在且具有『Sticky bit』的属性？
-s  侦测该档名是否存在且为『非空白文件』？
--------------------------------------------------------------------------------------
3. 两个文件之间的比较，如： test file1 -nt file2
--------------------------------------------------------------------------------------
-nt (newer than)判断 file1 是否比 file2 新
-ot (older than)判断 file1 是否比 file2 旧
-ef 判断 file1 与 file2 是否为同一文件，可用在判断 hard link 的判定上。 主要意义在判定，两个文件是否均指向同一个 inode 哩！
--------------------------------------------------------------------------------------
4. 关於两个整数之间的判定，例如 test n1 -eq n2 
--------------------------------------------------------------------------------------
-eq 两数值相等 (equal)
-ne 两数值不等 (not equal)
-gt n1 大於 n2 (greater than)
-lt n1 小於 n2 (less than)
-ge n1 大於等於 n2 (greater than or equal)
-le n1 小於等於 n2 (less than or equal)
--------------------------------------------------------------------------------------
5. 判定字串的数据
--------------------------------------------------------------------------------------
test -z string  判定字串是否为 0 ？若 string 为空字串，则为 true
test -n string  判定字串是否非为 0 ？若 string 为空字串，则为 false。注： -n 亦可省略
test str1 = str2    判定 str1 是否等於 str2 ，若相等，则回传 true
test str1 != str2   判定 str1 是否不等於 str2 ，若相等，则回传 false
--------------------------------------------------------------------------------------
6. 多重条件判定，例如： test -r filename -a -x filename
--------------------------------------------------------------------------------------
-a  (and)两状况同时成立！例如 test -r file -a -x file，则 file 同时具有 r 与 x 权限时，才回传 true。
-o  (or)两状况任何一个成立！例如 test -r file -o -x file，则 file 具有 r 或 x 权限时，就可回传 true。
!   反相状态，如 test ! -x file ，当 file 不具有 x 时，回传 true

```bash
# 1. 让使用者输入档名，并且判断使用者是否真的有输入字串？
echo -e "Please input a filename, I will check the filename's type and \
permission. \n\n"
read -p "Input a filename : " filename
test -z $filename && echo "You MUST input a filename." && exit 0
# 2. 判断文件是否存在？若不存在则显示信息并结束脚本
test ! -e $filename && echo "The filename '$filename' DO NOT exist" && exit 0
# 3. 开始判断文件类型与属性
test -f $filename && filetype="regulare file"
test -d $filename && filetype="directory"
test -r $filename && perm="readable"
test -w $filename && perm="$perm writable"
test -x $filename && perm="$perm executable"
# 4. 开始输出资讯！
echo "The filename: $filename is a $filetype"
echo "And the permissions are : $perm"
```

## use `[]`
```bash
[ "$HOME" == "$MALL" ] --> correct, 4 spaces are needed
name = "Shujin Wu"
[ $name == "Shujin Wu"] --> incorrect, can't compare Shujin Wu with "Shujin Wu"
```

## Default variable: $0, $1

+ `$#`：代表后接的参数『个数』，以上表为例这里显示为『 4 』；
+ `$@`：代表『 "$1" "$2" "$3" "$4" 』之意，每个变量是独立的(用双引号括起来)；
+ `$*`：代表『 "$1c$2c$3c$4" 』，其中 c 为分隔字节，默认为空白键， 所以本例中代表『 "$1 $2 $3 $4" 』之意。

```bash 
echo "The script name is        ==> $0"
echo "Total parameter number is ==> $#"
[ "$#" -lt 2 ] && echo "The number of parameter is less than 2.  Stop here." \
    && exit 0
echo "Your whole parameter is   ==> '$@'"
echo "The 1st parameter         ==> $1"
echo "The 2nd parameter         ==> $2"
```

Result:
```bash

[root@www scripts]# sh sh07.sh theone haha quot
The script name is        ==> sh07.sh            <==档名
Total parameter number is ==> 3                  <==果然有三个参数
Your whole parameter is   ==> 'theone haha quot' <==参数的内容全部
The 1st parameter         ==> theone             <==第一个参数
The 2nd parameter         ==> haha               <==第二个参数
```

## shift: 造成参数变量号码偏移
```bash
echo "Total parameter number is ==> $#"
echo "Your whole parameter is   ==> '$@'"
shift   # 进行第一次『一个变量的 shift 』
echo "Total parameter number is ==> $#"
echo "Your whole parameter is   ==> '$@'"
shift 3 # 进行第二次『三个变量的 shift 』
echo "Total parameter number is ==> $#"
echo "Your whole parameter is   ==> '$@'"


[root@www scripts]# sh sh08.sh one two three four five six <==给予六个参数
Total parameter number is ==> 6   <==最原始的参数变量情况
Your whole parameter is   ==> 'one two three four five six'
Total parameter number is ==> 5   <==第一次偏移，看底下发现第一个 one 不见了
Your whole parameter is   ==> 'two three four five six'
Total parameter number is ==> 2   <==第二次偏移掉三个，two three four 不见了
Your whole parameter is   ==> 'five six'
```

## if condition
```bash
if [ 条件判断式 ]; then
    当条件判断式成立时，可以进行的命令工作内容；
fi   <==将 if 反过来写，就成为 fi 啦！结束 if 之意！


# 一个条件判断，分成功进行与失败进行 (else)
if [ 条件判断式 ]; then
    当条件判断式成立时，可以进行的命令工作内容；
else
    当条件判断式不成立时，可以进行的命令工作内容；
fi


# 多个条件判断 (if ... elif ... elif ... else) 分多种不同情况运行
if [ 条件判断式一 ]; then
    当条件判断式一成立时，可以进行的命令工作内容；
elif [ 条件判断式二 ]; then
    当条件判断式二成立时，可以进行的命令工作内容；
else
    当条件判断式一与二均不成立时，可以进行的命令工作内容；
fi

[ "$yn" == "Y" -o "$yn" == "y" ]
上式可替换为
[ "$yn" == "Y" ] || [ "$yn" == "y" ]
```

## 利用 case ..... esac 判断

```bash
case  $变量名称 in   <==关键字为 case ，还有变量前有钱字号
  "第一个变量内容")   <==每个变量内容建议用双引号括起来，关键字则为小括号 )
    程序段
    ;;            <==每个类别结尾使用两个连续的分号来处理！
  "第二个变量内容")
    程序段
    ;;
  *)                  <==最后一个变量内容都会用 * 来代表所有其他值
    不包含第一个变量内容与第二个变量内容的其他程序运行段
    exit 1
    ;;
esac                  <==最终的 case 结尾！『反过来写』思考一下！

case $1 in
  "hello")
    echo "Hello, how are you ?"
    ;;
  "")
    echo "You MUST input parameters, ex> {$0 someword}"
    ;;
  *)   # 其实就相当於万用字节，0~无穷多个任意字节之意！
    echo "Usage $0 {hello}"
    ;;
esac
```

# function 
```bash 
function fname() {
    程序段
}

function printit(){
    echo -n "Your choice is "     # 加上 -n 可以不断行继续在同一行显示
}

echo "This program will print your selection !"
case $1 in
  "one")
    printit; echo $1 | tr 'a-z' 'A-Z'  # 将参数做大小写转换！
    ;;
  "two")
    printit; echo $1 | tr 'a-z' 'A-Z'
    ;;
  "three")
    printit; echo $1 | tr 'a-z' 'A-Z'
    ;;
  *)
    echo "Usage $0 {one|two|three}"
    ;;
esac
```

### variables in funciton 
```bash 
function printit(){
    echo "Your choice is $1"   # 这个 $1 必须要参考底下命令的下达
}

echo "This program will print your selection !"
case $1 in
  "one")
    printit 1  # 请注意， printit 命令后面还有接参数！
    ;;
  "two")
    printit 2
    ;;
  "three")
    printit 3
    ;;
  *)
    echo "Usage $0 {one|two|three}"
    ;;
esac
```

# Loop

## while do done, until do done
```bash 
while [ condition ]  <==中括号内的状态就是判断式
do            <==do 是回圈的开始！
    程序段落
done          <==done 是回圈的结束

until [ condition ]
do
    程序段落
done
```

## for...do...done
```bash 
for var in con1 con2 con3 ...
do
    程序段
done

users=$(cut -d ':' -f1 /etc/passwd)  # 撷取帐号名称
for username in $users               # 开始回圈进行！
do
        id $username
        finger $username
done


for (( 初始值; 限制值; 运行步阶 ))
do
    程序段
done

read -p "Please input a number, I will count for 1+2+...+your_input: " nu

s=0
for (( i=1; i<=$nu; i=i+1 ))
do
    s=$(($s+$i))
done
echo "The result of '1+2+3+...+$nu' is ==> $s"
```

# Debug
```bash
[root@www ~]# sh [-nvx] scripts.sh
选项与参数：
-n  ：不要运行 script，仅查询语法的问题；若语法没有问题，则不会显示任何资讯！
-v  ：再运行 sccript 前，先将 scripts 的内容输出到萤幕上；
-x  ：将使用到的 script 内容显示到萤幕上，这是很有用的参数！


```

# Note
+ Shell scripts 不适用于处理大量数值运算。 因为其速度较慢，且使用的CPU资源较多，造成主机资源的分配不良。

# Reference
[第十三章、学习 Shell Scripts](http://vbird.dic.ksu.edu.tw/linux_basic/0340bashshell-scripts_3.php)