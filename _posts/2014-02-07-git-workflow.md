---
layout: post
title: Simple git workflow 
author: xluffy
date:   2014-02-07
tags: git, system, develop
description: git workflow
---

### master

```bash
    1) git pull origin master
    2) git checkout -b weekly-news
        -> weekly-news
        ...
    9) git pull origin master
    10)git merge --no-ff weekly-news
```

### weekly-news

```bash
    3) git fetch origin
    4) git rebase origin/master
    5) git rebase origin/weekly-news
    6) git push origin weekly-news
    7) git rebase -i origin/master
    8) git checkout master
        -> master
```
