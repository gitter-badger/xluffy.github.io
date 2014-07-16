---
layout: post
title:  "Vài ghi chú về Docker"
author: xluffy
date:   2014-07-07
tags: docker, container, vm
description: "Vài ghi chú về Docker"
---

Vài ghi chép các nhân để nhớ những thao tác và khái niệm khi sử dụng Docker

Bài dịch, viết có tham khảo ở [https://gist.github.com/wsargent/7049221](https://gist.github.com/wsargent/7049221)

Docker Index của tôi: [https://registry.hub.docker.com/u/xluffy/saigon/](https://registry.hub.docker.com/u/xluffy/saigon/)

## Cài đặt trên ArchLinux

Cài đặt, start trên ArchLinux và kiểm tra

```bash
  ~$ pacman -S docker
  ~$ sudo systemctl start docker
  ~$ sudo docker info
  ~$ sudo docker version
```

Về cơ bản, có 2 khái niệm cần phân biệt là Container và Images, cụ thể từng phần như ở dưới.

## Docker Container

### Vòng đời

+ `docker run` tạo một container.
+ `docker stop` tắt một container.
+ `docker start` và bật nó lại.
+ `docker restart` khởi động lại một container.
+ `docker rm` xóa một container.
+ `docker kill` gửi một SIGKILL tới một container. Has issues.
+ `docker attach` sẽ connect với một container đang chạy (lệnh này tương tự như vzctl enter <node> hoặc virsh console <node-id>).
+ `docker wait` blocks until container stops.
	
Nếu bạn muốn chạy và tương tác với một container, `docker start` và `docker attach`

Nếu muốn có một container tạm thời, chạy `docker run -rm`, nó sẽ xóa container đó khi stop.

Nếu muốn chia sẻ một thư mục từ host tới docker container, chạy `docker run -v $HOSTDIR:DOCKERDIR`

Nếu muốn có một container tạm thời, chạy `docker run -rm`, nó sẽ xóa container đó khi stop.

### Thông tin

+ `docker ps` hiển thị các container đang chạy.
+ `docker inspect` tìm kiếm tất cả thông tin của một container, bao gồm cả địa chỉ IP.
+ `docker logs` lấy log từ một container.
+ `docker events` lấy events từ một container.
+ `docker port` hiển thị public port của một container.
+ `docker top` hiển thị các process trong một container.
+ `docker diff` hiển thị các file thay đổi trong FS của một container.
+ `docker ps -a` hiển thị tất cả các container, bao gồm cả đang chạy và stop.

### Import/Export

+ `docker cp` copy file hoặc thư mục ra khỏi filesystem của container.
+ `docker export` đóng gói filesystem của container vô một tarball

## Images

Images chỉ là một template cho docker container, tương tự như khái niệm template của OpenVZ

### Vòng đời

+ `docker images` hiển thị tất cả các images.
+ `docker import` tạo một image từ tarball.
+ `docker build` tạo một images từ Dockerfile.
+ `docker commit` tạo một image từ một container.
+ `docker rmi` xóa một image.
+ `docker insert` inserts một file từ URL vào image(kind of odd, you'd think images would be immutable after create)
+ `docker load` load một image từ một tar như STDIN, bao gồm cả images và tags (as of 0.7).
+ `docker save` lưu một image vào một file tar từ STDOUT with all parent layers, tags & versions (as of 0.7).

### Info

+ `docker history` hiển thị history của một images.
+ `docker tags` tags một image thành tên (local hoặc reg).

### Registry & Repository (hub.docker.io)

Một repository là một bộ sưu tập lưu trữ các images được đánh tags giúp tạo ra filesytem cho một container.

Một registry là một máy chủ -- máy chủ lưu trữ các repositorry và cung cấp một HTTP API cho phép quản lý việc upload và download của các repository.

Docker.io là một máy chủ, nó là một central registry chứa một số lượng lớn các repository, với tài khoản free sẽ được một private repo.

Registry là cách tôi gọi, chính xác phải gọi nó là Docker Index.

+ `docker login` login vào một registry.
+ `docker search` search registry cho image.
+ `docker pull` pulls một image từ registry về local machine.
+ `docker push` push một images tới registry từ local machine.

## Dockerfile (là một trong các cách tạo image, nhưng phổ biến nên sẽ nói riêng nó)

Thú thật là chả có gì để viết về phần này, 1 là Dockerfile vô cùng basic của tôi, dùng để thử, và 1 của người khác, cũng đơn giản ko kém

```bash
	# Build an Image from Dockerfile
	FROM ubuntu:12.04
	MAINTAINER Quang Nguyen <xquang.foss@gmail.com>
```

```bash
	# Base image
	FROM ubuntu:12.04
	ENV REFRESHED_AT 2013-11-16

	RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list
	RUN apt-get update
	RUN apt-get upgrade -y

	# Install common packages (from http://youtu.be/1Fm3MJhQZZg)
	run apt-get -y -q install aptitude sudo apt-utils ntp build-essential curl tzdata wget less \
	dnsutils gzip netcat screen unzip sysstat git python-software-properties vim zsh

	# set up timezone
	run ln -s /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime

	# set up locale
	run locale-gen en_US.UTF-8
	run update-locale LANG=en_US.UTF-8
	ENV LC_ALL en_US.UTF-8
	ENV LANG en_US.UTF-8
```




