---
layout: post
title:  "Một số lệnh ngắn và hữu ích"
author: xluffy
date:   2014-08-23
tags: command-line, linux
description: "Một số lệnh ngắn và hữu ích"
---

### Find

Có một đám log nén bỏ lung tung các thư mục, giờ muốn gom về một chỗ để lưu trữ

```bash
	~$ find . -name php-fpm*.tgz -exec mv {} archive/ \;
```

Có một đống các thư mục tên `log` nằm rải rác trong các thư mục con, giờ muốn xóa đi

```bash
	~$ find . -name "log" -exec rm -rf {} \;
```