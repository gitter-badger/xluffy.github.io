---
layout: post
title:  "Một số gói cần thiết sau khi cài CentOS minimum"
author: xluffy
date:   2014-02-11
tags: linux, package, CentOS
description: "Một số gói cần thiết sau khi cài CentOS minimum"
---

### Vấn đề:

Khi cài CentOS phiên bản minimum sẽ thiếu rất nhiều gói cần thiết cho việc quản trị hệ thống. Ví dụ như
`vim, htop, wget...` các gói cần cho quá trình biên dịch các gói phần mềm khác.

### Giải quyết

Cài đặt repo Epel
```
	wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
	sudo rpm -Uvh epel-release-6*.rpm
```

Các gói base:

```
	yum install systemtool system-config-network-tui vim wget curl gcc libtool make autoconf automake htop -y
```

