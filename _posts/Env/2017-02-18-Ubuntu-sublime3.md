---
layout: post
title: "Set up Sublime3 in Ubuntu"
description: ""
category: Ubuntu
tags:  [Ubuntu]
---
{% include JB/setup %}

# Packages

SidebarEnhancement
SublimeLinter
SublimeCodeIntel
SublimeHaskell
GitGutter
JSLint
Markdown-editing
Trailing-space
Color-highlighter
bracketHighlighter
CTag
(afterglow-theme)[https://github.com/YabataDesign/afterglow-theme]

# Configuration

```json
{
    "added_words":
    [
        "filesystems"
    ],
    "color_inactive_tabs": true,
    "color_scheme": "Packages/User/SublimeLinter/Afterglow-markdown (SL).tmTheme",
    "font_size": 14,
    "ignored_packages":
    [
        "Auto Hide Sidebar",
        "Vintage"
    ],
    "ignored_words":
    [
        "Cgroup",
        "Filesystem",
        "Namespace",
        "Seccomp",
        "filesystem",
        "len",
        "namedtuple",
        "namespace",
        "namespacing",
        "obj1",
        "obj2",
        "plit",
        "seccomp",
        "str",
        "uits"
    ],
    "open_externally_patterns":
    [
        "*.jpg",
        "*.jpeg",
        "*.png",
        "*.gif",
        "*.zip",
        "*.pdf"
    ],
    "sidebar_icon": true,
    "sidebar_row_padding_medium": true,
    "sidebar_size_12": true,
    "status_bar_brighter": true,
    "tab_size": 4,
    "tabs_medium": true,
    "tabs_padding_medium": true,
    "theme": "Afterglow-blue.sublime-theme",
    "translate_tabs_to_spaces": true,
    "word_wrap": true
}
```
