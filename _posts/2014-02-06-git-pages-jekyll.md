---
layout: post
title:  "How to create blog with git pages and jekyll"
author: xluffy
date:   2014-02-07
tags: tips
description: "Huong dan tao blog cho n~ ke ngheo"
---

###Step 1: Create a repository

Head over to GitHub and create a new repository named __*xluffy.github.io*__, where xluffy is your username (or organization name) on GitHub.

If the first part of the repository doesn’t exactly match your username, it won’t work, so make sure to get it right.

###Step 2: Creating Pages with the automatic generator

1. Go to the repository's settings page.
2. Click the Automatic Page Generator button.
3. Author your content in the Markdown editor.
4. Click the Continue To Layouts button.
5. Preview your content in our themes.
6. When you find a theme that you like, click Publish.

###Step 3: Clone the repository

Go to the folder where you want to store your project, and clone the new repository:

```
    ~$ git clone https://github.com/xluffy/xluffy.github.io
	~$ cd xluffy.github.io
	~$ rm -rf * 
	// xoa het cac template cu cua git pages, 
	// vi ta se su dung layout cua jekyll, chi giu lai .git/
```

###Step 4: Setup Jekyll and create layout blog

1. Setup Jekyll on Arch Linux

```
	~ $ sudo gem install jekyl
	~ $ jekyll new ~/xluffy.github.io 
	~ $ git add --all
	~ $ git commit -m"Init Jekyll blog"
	~ $ git push origin master
```

2. Start Jekyll Server and access http://localhost:4000 or http://127.0.0.1:4000

```
	~ $ sudo /root/.gem/ruby/2.1.0/gems/jekyll-1.4.3/bin/jekyll serve # If you not fix PATH
```

3. Modify config and first post
	
```
	~ $ cd xluffy.github.io
	~ $ vim _config.yaml -> fix anything
	~ $ vim _layouts/default.html -> fix your infomation
	~ $ echo "
			#!/usr/bin/env ruby
			puts "Hello, World!!"
		" _posts/2014-02-06-hello-world.markdown 
```
