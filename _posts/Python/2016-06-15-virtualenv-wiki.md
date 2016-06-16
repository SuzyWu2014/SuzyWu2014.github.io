---
layout: post
title: "Wiki - Virtualenv "
description: ""
category: Python
tags:  [Python,Python Data Structure]
---
{% include JB/setup %}

# Installation

```bash
[sudo] pip install virtualenv
```

# Usage

```bash
mkdir virtual_env               # create a directory to place different envs
virtualenv virtual_env/myenv1   # create one env
source virtual_env/myenv1/bin/activate.fish # activate the env under fish shell
deactivate
```

# Reference:

[Virtualenv Tutorial](http://www.simononsoftware.com/virtualenv-tutorial/)
[User Guide](https://virtualenv.pypa.io/en/stable/userguide/)