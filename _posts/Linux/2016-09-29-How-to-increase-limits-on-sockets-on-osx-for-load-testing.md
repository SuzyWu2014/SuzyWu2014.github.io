---
layout: post
title: "How to increase limits on sockets on osx for load testing?"
description: ""
category: Linux
tags:  [Load Testing]
---
{% include JB/setup %}

Refer to [How to increase limits on sockets on osx for load testing?](http://stackoverflow.com/questions/7578594/how-to-increase-limits-on-sockets-on-osx-for-load-testing)

# Instruction

```
$ sysctl kern.maxfiles
kern.maxfiles: 12288
$ sysctl kern.maxfilesperproc
kern.maxfilesperproc: 10240
$ sudo sysctl -w kern.maxfiles=1048600
kern.maxfiles: 12288 -> 1048600
$ sudo sysctl -w kern.maxfilesperproc=1048576
kern.maxfilesperproc: 10240 -> 1048576
$ ulimit -S -n
256
$ ulimit -S -n 1048576
$ ulimit -S -n
1048576
```