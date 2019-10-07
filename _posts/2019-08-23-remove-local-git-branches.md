---
layout: post
title: Delete all local Git branches that have been merged into the current branch
tags:
  - github
  - commands
---
```
  git branch --merged | egrep -v "(^\*|master|staging|stage|develop|dev)" | xargs git branch -d
```
