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
$ mkdir ~/.itermocil && touch ~/.itermocil/projectname.yml && open ~/.itermocil/projectname.yml
```

Paste in this layout:

```
windows:
  - name: projectname
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
$ echo 'alias projectname = "itermocil projectname"' >>~/.bash_profile
$ souce ~/.bash_profile
$ projectname
```

You could take this a step further by opening VS Code to your project directory as part of the same alias.

[Add `code` to your shell](https://stackoverflow.com/questions/29963617/how-to-call-vs-code-editor-from-command-line) by following these steps (These are Mac instructions, check the link for other platforms):

1. Open VS Code
2. Press `F1` and search "shell"
3. Click on `Shell Command: Install 'code' command in PATH`

Now update your alias:

```
alias projectname = "itermocil projectname && code path/to/project/dir"
```
