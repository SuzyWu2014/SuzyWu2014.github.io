---
layout: post
title: "Wiki - Linux"
description: ""
category: Linux
tags:  [Linux]
---
{% include JB/setup %}

# `tar.bz2` vs `tar.gz`

`.bz2`和`.gz`都是linux下压缩文件的格式，有点类似windows下的`.zip`和`.rar`文件。
`.bz2`和`.gz`的区别在于，前者比后者压缩率更高，后者比前者花费更少的时间。也就是说同一个文件，压缩后，`.bz2`文件比`.gz`文件更小，但是`.bz2`文件的小是以花费更多的时间为代价的。 

```bash
tar xvfz XXX.tar.gz
cd XXX
./configure
make
make install
```

# Mac OSX

## Enable Apache
```bash
sudo su -       # become root
apachectl start # enable apache
httpd -v        # check apache version
```

# Change Shell

```bash
chsh -s /usr/local/bin/fish
chsh -s /bin/bash
```

# Tree display

```bash
brew tree
tree /dir
```