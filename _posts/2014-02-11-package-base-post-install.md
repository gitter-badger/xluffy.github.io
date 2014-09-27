---
layout: post
title:  "Một số gói cần thiết sau khi cài CentOS minimum"
author: xluffy
date:   2014-02-11
tags: linux, package, CentOS
description: "Một số gói cần thiết sau khi cài CentOS minimum"
---

### Vấn đề:

Khi cài CentOS phiên bản minimum sẽ thiếu rất nhiều gói cần thiết cho việc quản
trị hệ thống. Ví dụ như `vim, htop, wget...` các gói cần cho quá trình biên dịch
các gói phần mềm khác.

### Giải quyết

Cài đặt repo Epel

``` 
	~$ wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm 	
	~$ sudo rpm -Uvh epel-release-6*.rpm
```

Cai dat repo Remi

```
	~$ wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
	~$ rpm -Uvh remi-release-5*.rpm
```

Các gói base:

```
	~$ yum install setuptool system-config-network-tui vim libtool -y 	
	~$ yum install make autoconf automake htop wget curl gcc gcc-c++ -y
```

Thiết lập:

```
	~$ /etc/init.d/iptables stop 	
	~$ /etc/init.d/ip6tables stop 	
	~$ chkconfig iptables off 	
	~$ chkconfig ip6tables off
```


