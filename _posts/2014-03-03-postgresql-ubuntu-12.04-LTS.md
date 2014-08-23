---
layout: post
title:  "Install PostgreSQL 9.2 on Ubuntu 12.04 LTS"
date:   2014-02-19
tags: ubuntu, postgresql, lts
description: "Install PostGreSQL 9.2 on Ubuntu 12.04 LTS"
---

#### Install PostgreSQL 

```
	sudo apt-get update
	sudo apt-get -y install python-software-properties
	sudo add-apt-repository ppa:pitti/postgresql
	sudo apt-get update
	
	sudo apt-get -y install postgresql-9.2 libpq-dev

	sudo passwd postgres => *********

	sudo -u postgres psql postgres
	\password postgres	=> *********

	vim /etc/sysctl.conf

	kernel.shmmax=8589934592 

	shared_buffers = 4096MB  

```
