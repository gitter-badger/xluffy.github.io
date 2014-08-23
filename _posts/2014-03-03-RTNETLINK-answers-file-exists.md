---
layout: post
title:  "RTNETLINK answers: File exists"
date:   2014-02-19
tags: ubuntu, linux, network
description: "RTNETLINK answers: File exists"
---

#### Van de

Ubuntu 12.04 LTS co 2 NIC, sau khi cau hinh eth1 voi noi dung nhu sau:

```
	auto eth1
	iface eth1 inet static
	address 192.168.1.117
	netmask 255.255.255.0
	gateway 192.168.1.1  
```

Chay command thi xuat hien loi ___RTNETLINK answers: File exists___

```
	~ $ ifup eth1
```

Chay lai thu them option -v thi thay

```
	ip addr add 192.168.0.2/255.255.255.0 broadcast 192.168.0.255     dev eth1 label eth1
```

#### Giai phap

```
	ip addr flush dev eth1
```
