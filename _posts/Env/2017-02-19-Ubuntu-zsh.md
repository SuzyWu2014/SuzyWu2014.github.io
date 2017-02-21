---
layout: post
title: "Set up Oh-my-zsh in Ubuntu"
description: ""
category: Ubuntu
tags:  [Ubuntu]
---
{% include JB/setup %}

# Installation

+ install zsh
+ install oh-my-zsh

```bash
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"

chsh -s which zsh
# and after that restart it
sudo shutdown -r 0

```

# Configuration

+ Edit `~/.zshrc`

```
ZSH_THEME="agnoster"

plugins=(git, zsh-autosuggestions, git-prompt)
```

+ Install [Powerline fonts](https://github.com/powerline/fonts)

+ Terminal Profile
    - Select one of Powerline fonts with 14pt
    - resize: `170 * 20`
    - Colors: select Solarize-dark and modify from there

+ install [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
