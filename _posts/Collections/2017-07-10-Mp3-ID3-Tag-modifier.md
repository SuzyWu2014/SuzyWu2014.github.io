---
layout: post
title: "修复 mp3 文件导入 iTunes 时的乱码问题"
description: ""
category: Tools
tags:  [ID3 Tag]
---
{% include JB/setup %}

TLDR:

```bash
pip install mutagen
mid3iconv -e gbk *.mp3 # fix files in the same dir

# fix files in the nested dir
find . -iname "*.mp3" -execdir mid3iconv -e gbk {} \;
```

Reference:
[如何解决Linux下mp3乱码](http://www.xzcblog.com/post-104.html)