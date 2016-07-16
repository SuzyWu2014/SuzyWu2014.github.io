---
layout: post
title: "Dropwizard Internals"
description: ""
category: API
tags:  [API]
---
{% include JB/setup %}

# Startup Sequence

## 1. Application Class

### Contructor: initialize()

+ new Bootstrap
+ bootstrap.addCommand(new ServerCommand)
+ bootstrap.addCommand(new CheckCommand)
+ initialize(bootstrap) (implemented by your Application)
    - bootstrap.addBundle(bundle)
        * bundle.initialize(bootstrap)
    - bootstrap.addCommand(cmd)
        * cmd.initialize()

### Cli
+ new Cli(bootstrap and other params)
    - for each cmd in bootstrap.getCommands()
        * configure parser w/ cmd
+ cli.run()
    - is help flag on cmdline? if so, print usage
    - parse cmdline args, determine subcommand (rest of these notes are specific to ServerCommand)
    - command.run(bootstrap, namespace) (implementation in ConfiguredCommand)
        * parse configuration
        * setup logging
    - command.run(bootstrap, namespace, cfg) (implementation in EnvironmentCommand)
        * create Environment
        * bootstrap.run(cfg, env)
            * for each Bundle: bundle.run()
            * for each ConfiguredBundle: bundle.run()
        * application.run(cfg, env) (implemented by your Application)

### Command..run()
+ command.run(env, namespace, cfg) (implemented by ServerCommand)
    - starts Jetty


