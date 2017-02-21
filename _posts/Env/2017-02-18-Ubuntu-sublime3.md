---
layout: post
title: "Set up Sublime3 in Ubuntu"
description: ""
category: Ubuntu
tags:  [Ubuntu]
---
{% include JB/setup %}

# Ansible Role

Run Ansible playbook: [Ubuntu Environment Set Up](https://github.com/SuzyWu2014/my-ansible-roles)

# Packages

`[Ctrl+Shift+P] -> Install Packages`

+ SidebarEnhancement
+ SublimeLinter
+ SublimeLinter-pep8
+ SublimeCodeIntel
+ SublimeHaskell
+ GitGutter
+ JSLint
+ Markdown-editing (update line_numbers to true for each user setting)
+ Markdown Preview
+ Trailing-space
+ Color-highlighter
+ bracketHighlighter
+ CTag
+ Anisble
+ [afterglow-theme](https://github.com/YabataDesign/afterglow-theme)

# Configuration

`Preferences -> Settings (User)`

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

`Preferences -> key bindings`
```json
[{ "keys": ["ctrl+shift+o"], "command": "prompt_open_folder" },
{
    "extensions":
    [
        "mmd"
    ],

    //"color_scheme": "Packages/MarkdownEditing/MarkdownEditor.tmTheme",
    "color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Dark.tmTheme",
    //"color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Yellow.tmTheme",

    // This is a quick and dirty focus theme. In order to make it work, you've to
    // set `"highlight_line": true,` in this settings file. It is likely that there will
    // be more mature focus improvements in the future (maybe similar to iA Writer).
    //"color_scheme": "Packages/MarkdownEditing/MarkdownEditor-Focus.tmTheme",

    "tab_size": 4,
    "translate_tabs_to_spaces": true,
    "trim_trailing_white_space_on_save": false,
    "auto_match_enabled": true,

    // Layout
    "draw_centered": true,
    "word_wrap": true,
    "wrap_width": 80,
    "rulers": [],

    // Line
    "line_numbers": true,
    "highlight_line": true,
    "line_padding_top": 2,
    "line_padding_bottom": 2,

    // Caret
    "caret_style": "wide",
    "caret_extra_top": 3,
    "caret_extra_bottom": 3,

    // add trailing #'s to headlines
    "mde.match_header_hashes": false,

    // Automatically switches list bullet when indenting blank list item with <Tab>.
    "mde.list_indent_auto_switch_bullet": true,

    // List bullets to be used for automatically switching. In their order.
    "mde.list_indent_bullets": ["*", "-", "+"],

    // Auto increments ordered list number. Set to false if you want always "1".
    "mde.auto_increment_ordered_list_number": true,

    // Always keep current line vertically centered.
    "mde.keep_centered": false,

    // Distraction free mode improvements. In order for these to work, you have to install
    // FullScreenStatus plugin: https://github.com/maliayas/SublimeText_FullScreenStatus
    "mde.distraction_free_mode": {
        "mde.keep_centered": true
    },

    "mde.lint":{
        // disabled rules, e.g. "md001".
        "disable": ["md013"],
        // Options:
        //      atx,        ## title    only
        //      atx_closed, ## title ## only
        //      setext,     title       only
        //                  =====
        //      any,        consistent within the document
        "md003": "any",
        // Options:
        //      asterisk,   * only
        //      plus,       + only
        //      dash,       - only
        //      cyclic,     different symbols on different levels
        //                  and same symbol on same level
        //      single,     same symbol on different levels
        //      any,        same symbol on same level
        "md004": "cyclic",
        // Number of spaces per list indent. Set to 0 to use Sublime tab_size instead
        "md007": 0,
        // Maximum line length, Set to 0 to use Sublime wrap_width instead
        "md013": 0,
        // Disallowed trailing punctuation in headers
        "md026": ".,;:!",
        // Options:
        //      one,        '1.' only
        //      ordered,    ascending number
        //      any,        consistent within one list
        "md029": "any",
        // (ordered vs unordered, single-line vs multi-line)
        "md030": {
            "ul_single": 1,
            "ol_single": 1,
            // optinally, 3
            "ul_multi": 1,
            // optionaly, 2
            "ol_multi": 1
        }
    }
}
]

```

## Add Chinese Support

[Enable sougou-pinyin in SublimeText 3](http://html5beta.com/page/ubuntu-14-04-install-fcitx-sougoupinyin-sublime-text-3-chinese-input-fix.html#)

+ Go to `/opt/sublime_text`
+ Run `sudo gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC`