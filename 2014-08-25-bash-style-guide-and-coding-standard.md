---
layout: post
title:  "Bash Style Guide and Coding Standard"
author: xluffy
date:   2014-08-25
tags: bash, coding, style
description: "Bash Style Guide and Coding Standard"
---

## Bài toán

### 1. Length of Line

Tổng độ dài của một dòng (bao gồm cả comment) không được phép dài quá __88 ký tự__.
Việc này giúp tránh phải tìm kiếm theo chiều ngang (đối với các dòng quá dài vượt quá
chiều dài của editor)

### 2. Indentation

Là quy định về __thụt đầu dòng__, trong hầu hết các trường hợp, 2, 4, 8 là các giá trị 
thường được lựa chọn

### 3. Comment

#### 3.1 Introductory comments in files

Là đoạn comment đầu tiên mỗi file, mục đích giới thiệu một số thông tin về script, về
tác giả, hướng dẫn sử dụng.

```bash
	#!/bin/bash
	#===================================================================================
	#
	#	FILE:  stale-links.sh
	#
	#	USAGE:  stale-links.sh [-d] [-l] [-oD logfile] [-h] [starting directories]
	#
	#	DESCRIPTION: List and/or delete all stale links in directory trees.
	#				 The default starting directory is the current directory.
	#				 Don’t descend directories on other filesystems.
	#
	#	OPTIONS:  see function ’usage’ below
	# 	REQUIREMENTS:  ---
	#	BUGS:  ---
	#	NOTES:  ---
	#	AUTHOR:  Dr.-Ing. Fritz Mehner (fgm), mehner.fritz@fh-swf.de
	#	COMPANY:  FH Südwestfalen, Iserlohn
	#	VERSION:  1.3
	#	CREATED:  12.05.2002 - 12:36:50
	#   REVISION:  20.09.2004
	#===================================================================================
```

#### 3.2 Line of comments

Là các phần comment ở cuối mối dòng, mục đích làm rõ ý nghĩa, ví dụ biến  đó được sử dụng cho mục đích gì

```bash
	MYSQL="/usr/bin/mysql"	# path of mysql service
```

#### 3.3 Section comments

Là phần comment mô tả một __section__

```bash
	#----------------------------------------------------------------------
	#  delete links, if demanded write logfile
	#----------------------------------------------------------------------
	if[ "$action" == ’d’ ] ;then
		rm --force "$file" && ((deleted++))
		echo "removed link :  ’$file’"
		[ "$logfile" != "" ] && echo"$file" >> "$logfile"
	fi
```

#### 3.4 Function comments

Là một phần mô tả ngắn về comment, bao gồm tên func, chức năng, mục đích sử dụng

```bash
	#===  FUNCTION  ================================================================
	#  NAME:  usage
	#  DESCRIPTION:  Display usage information for this script.
	#  PARAMETER  1:  ---
	#===============================================================================
```

#### 3.5 Commenting style

__Ngắn, gọn, xúc tích và chính xác__

### 4. Biến và hằng







## Ref

[http://lug.fh-swf.de/vim/vim-bash/StyleGuideShell.en.pdf](http://lug.fh-swf.de/vim/vim-bash/StyleGuideShell.en.pdf)
