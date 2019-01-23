---
layout: post
title: Easy Project Switching With Itermocil + Command Line Shortcuts
tags:
  - itermocil
  - alias
  - freelance
---

I often need to switch between multiple projects, or maybe I'm just getting started for the day. Spending time booting up these project environments becomes a big waste.

## Itermocil

First install [Itermocil](https://github.com/TomAnthony/itermocil) for pre-configured layouts and commands saved in `.yml` files.

```
$ brew update && brew install TomAnthony/brews/itermocil
$ mkdir ~/.itermocil && touch ~/.itermocil/project-name.yml && open ~/.itermocil/project-name.yml
```

Paste in this layout:

```
windows:
  - name: project-name
    root: /path/to/project
    layout: even-horizontal
    panes:
      - npm run start
      - git status

```

You can test this by typing `itermocil layout` in iTerm2.

## Command Line Shortcuts

To make this process even faster (9 keystrokes faster) setup an alias for each of your layouts.

```
$ echo 'alias project-name = "itermocil project-name"' >>~/.bash_profile
$ souce ~/.bash_profile
$ project-name
```
