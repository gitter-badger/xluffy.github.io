---
layout: post
title:  "Cài đặt và cấu hình VNC server trên Linux"
author: xluffy
date:   2014-08-2
tags: vnc, remote desktop, ubuntu, centos
description: "Cài đặt và cấu hình VNC server trên Linux"
---

### Tại sao??

Nhu cầu remote desktop trên Linux khá hạn chế, phần vì server thì không nhất thiết phải có giao diện,
phần vì SSH lúc nào cũng tuyệt vời, phần vì remote-desktop trên Linux thì chán không tả nổi.

Tuy nhiên, có những lúc vẫn cần phải remote-desktop dù muốn hay không, nếu lười, cách nhanh nhất là tải
teamviewer về và chạy luôn. Bản thân tôi rất ghét VNC, vì một lần từng cài qua và không được, phần vì ghét
vì remote lag quá. Ngoài VNC thì tôi còn được bạn bè giới thiệu No-Machine, nhưng cài mãi mà chả được.

Lần này bắt buộc phải cài nên đành viết lại để mà nhớ

### Với Ubuntu

Step 1: Cài giao diện để có cái mà remote, ở đây cài xfce4 cho nhẹ

```bash
	~$ apt-get -y install ubuntu-desktop tightvncserver xfce4 xfce4-goodies
```

Step 2: Add user và set password

```bash
	~$ adduser xluffy
	~$ passwd xluffy
```

Cấp sudo

```bash
	~$ echo "xluffy ALL=(ALL) ALL" >> /etc/sudoers
```

Thiết lập VNC passwd

```bash
	~$ su - xluffy
	~$ vncpasswd
	exit
```

Step 3: Init script

Đặt tại `/etc/init.d/vncserver`

```bash
	#!/bin/bash
	PATH="$PATH:/usr/bin/"
	export USER="xluffy"
	DISPLAY="1"
	DEPTH="16"
	GEOMETRY="1024x768"
	OPTIONS="-depth ${DEPTH} -geometry ${GEOMETRY} :${DISPLAY}"
	. /lib/lsb/init-functions

	case "$1" in
	start)
	log_action_begin_msg "Starting vncserver for user '${USER}' on localhost:${DISPLAY}"
	su ${USER} -c "/usr/bin/vncserver ${OPTIONS}"
	;;

	stop)
	log_action_begin_msg "Stoping vncserver for user '${USER}' on localhost:${DISPLAY}"
	su ${USER} -c "/usr/bin/vncserver -kill :${DISPLAY}"
	;;

	restart)
	$0 stop
	$0 start
	;;
	esac
	exit 0
```

Edit `/home/xluffy/.vnc/xstartup`

```bash
	#!/bin/sh
	xrdb $HOME/.Xresources
	xsetroot -solid grey
	startxfce4 &
```

Thiết lập quyền

```bash
	~$ chown -R vnc. /home/xluffy/.vnc && chmod +x /home/xluffy/.vnc/xstartup
	~$ sed -i 's/allowed_users.*/allowed_users=anybody/g' /etc/X11/Xwrapper.config
```

Khởi động dịch vụ

```bash
	~$ chmod +x /etc/init.d/vncserver && service vncserver start
```

Tự start khi khởi động

```bash
	~$ update-rc.d vncserver defaults
```

Step 5: Connect từ client

Tải VNC view về máy tính 

![VNC-View](http://i.imgur.com/IJPrWR7.png)

Nhập password ở bước vncpasss

![Pass](http://i.imgur.com/UVGhCxv.png)

Và xong

![Xfce4](http://i.imgur.com/nMertRl.png)

### Trên CentOS

Về bản chất là như nhau, có một điểm khác là cần cấu hình file config

```bash
	~$ yum groupinstall Desktop
	~$ yum install tigervnc-server
	~$ yum install xorg-x11-fonts-Type1
	~$ yum install vnc
```

Các bước tạo user và set vncpasss là như trên Ubuntu

Thêm một bước cấu hình như sau

```bash
	~$ nano /etc/sysconfig/vncservers
```

Ví dụ cấu hình cho 2 user có thể remote-desktop

```bash
	VNCSERVERS="1:xluffy 2:few"
	VNCSERVERARGS[1]="-geometry 1024x600"
	VNCSERVERARGS[2]="-geometry 1024x600"
```

Kill 

```bash
	~$ vncserver -kill :1
```

```
	~$ nano /home/xluffy/.vnc/xstartup
	#twm & 
	exec gnome-session &	
```

LƯU Ý:

- VNC không dùng chung session với khi truy cập trực tiếp 

- Ubuntu  dùng xfce4, CentOS là gnome, nên phần xstartup có khác nhau, start các DE khác nhau







