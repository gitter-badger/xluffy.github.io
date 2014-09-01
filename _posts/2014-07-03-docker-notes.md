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

## 1.1 Docker là gì?

Docker là một công cụ được tạo bởi dotCloud giúp cho việc sử dụng Linux Containers (LXC) trở nên dễ dàng hơn. Linux Containers 
là một phương thức cung cấp một lớp Hệ Điều Hành ảo hóa, cho phép chạy nhiều môi trường máy chủ độc lập trên một host điều khiển.
LXC không cung cấp một virtual machine, nhưng cung cấp một môi trường ảo có các process và không gian mạng riêng biệt. LXC tương
tự như `chroot` nhưng cung cấp nhiều tính năng giúp các môi trường trở lên `độc lập` hơn.

## 1.2 Docker Containers khác với Virtual Machines như thế nào?

Docker, công cụ sử dụng Linux Containers (LXC) chạy chung kernel với host (nghĩa là không có container Windows). Điều này cho phép docker 
có thể chia sẻ nhiều tài nguyên của host. Docker sử dụng AuFS cho hệ thống tập tin.

AuFS là hệ thống tập tin cho phép `union mount`, hiểu đơn giản nghĩa là AuFS cho phép bạn mount nhiều thư mục vào một mount-point __với 
các quyền đọc ghi khác nhau__. `union mount` được sử dụng phổ biến trên các LiveCD, cho phép boot vào hệ điều hành mà không "ghi" gì 
vào ổ cứng (kiểu như bạn có thể boot vào LiveCD, cài một số thứ nhưng khi thoát ra thì trở về trạng thái cũ). Vẫn khá là khó hiểu đúng 
không, vậy thử một vài ví dụ

```bash
	# mkdir /tmp/docker
	# mkdir /tmp/aufs-root
	# mount -t aufs -o br=/tmp/docker:/home/xluffy none /tmp/aufs-root/
```

```bash
	-o – chi tiết những option
	br – chi tiết một nhánh, mỗi nhánh phân cách nhau bởi dấu hai chấm `:`
	none – mount 2 thư mục chứ không phải filesystem
```

=> Giờ nếu xem nội dung trong thư mục `/tmp/aufs-root` sẽ thấy chứa nội dung của cả 2 thư mục `/tmp/docker` và thư mục `/home/xluffy`, 
và default không có quyền gì thì nhánh đầu tiên sẽ có quyền write, còn nhánh thứ 2 sẽ chỉ có quyền đọc. Nghĩa là nếu bạn tạo một file 
mới trong `/tmp/aufs-root` thì file đó sẽ được tạo trong `/tmp/docker`

AuFS được viết lại hoàn toàn dựa trên Unionfs trước đó. Mục đích nhằm cải thiện độ tin cậy, hiệu xuất và cung cấp thêm vào tính năng 
mới, cải tiến mới. Một vài bản phân phối Linux đã sử dụng AuFS thay thế cho Unionfs như Arch Linux, Ubuntu,Debian, Gentoo ...

AuFS và LXC hoạt động chung như thế nào? Bạn có nhiều container, chúng sử dụng chung tài nguyên của máy chủ chứa chúng, ví dụ chúng 
xài chung cây thư mục `/`, nghĩa là cũng có `/usr/lib, /bin/, /usr/bin .etc.`, nhưng nếu cho các container "ghi" dữ liệu vào đây thì có 
thể làm hỏng cả host của bạn. Vì thế `union mount` được sử dụng vừa để chia sẻ tài nguyên, vừa để phân quyền để tạo một môi trường độc
lập, tránh các container ảnh hưởng và làm hỏng dữ liệu của nhau.


## 1.3 Cài đặt

### 1.3.1 Yêu cầu 

- Kernel phiên bản lớn hơn 3.8 và cgroups, namespaces phải được bật.
- AUFS : AUFS bao gồm trong kernels được build bởi Debian, Ubuntu, Arch Linux, nhưng không được build sẵn trong các kernel tiêu chuẩn. Nếu
bạn sử dụng bản phân phối khác, cần load AuFS vào kernel trước.
- LXC : Linux Containers

Có nhiều cách cài docker, từ source, từ các trình quản lý gói của các bản phân phối phổ biến, bạn tự chọn một cách cho mình, rất dễ nếu 
bạn đã đi được một đoạn đường dài với Linux. Ở đây tôi dùng Arch Linux nên sẽ hướng dẫn cách cài bằng trình quản lý gói tin `pacman` của Arch
Linux.

### 1.3.2 Cài đặt trên Arch Linux

Cài đặt, start trên Arch Linux và kiểm tra

```bash
  ~$ pacman -S docker
  ~$ sudo systemctl start docker or sudo <path to>/docker -d &
  ~$ sudo docker info
  ~$ sudo docker version
```

Về cơ bản, có 2 khái niệm cần phân biệt là Container và Images, cụ thể từng phần như ở dưới.

## 1.4 Docker Container

### 1.4.1 Vòng đời

+ `docker run` tạo một container.
+ `docker stop` tắt một container.
+ `docker start` và khởi động container.
+ `docker restart` khởi động lại một container.
+ `docker rm` xóa một container.
+ `docker kill` gửi một SIGKILL tới một container. Has issues.
+ `docker attach` sẽ connect với một container đang chạy (lệnh này tương tự như vzctl enter <node> hoặc virsh console <node-id>).
+ `docker wait` blocks until container stops.
	
Nếu bạn muốn chạy và tương tác với một container, `docker start` và `docker attach`
Nếu muốn có một container tạm thời, chạy `docker run -rm`, lệnh này sẽ xóa container đó khi stop.
Nếu muốn chia sẻ một thư mục từ host tới docker container, chạy `docker run -v $HOSTDIR:DOCKERDIR`
Nếu muốn có một container tạm thời, chạy `docker run -rm`, lệnh này sẽ xóa container đó khi stop.

### 1.4.2 Thông tin

+ `docker ps` hiển thị các container đang chạy.
+ `docker inspect` tìm kiếm tất cả thông tin của một container, bao gồm cả địa chỉ IP.
+ `docker logs` lấy log từ một container.
+ `docker events` lấy events từ một container.
+ `docker port` hiển thị public port của một container.
+ `docker top` hiển thị các process trong một container.
+ `docker diff` hiển thị các file thay đổi trong FS của một container.
+ `docker ps -a` hiển thị tất cả các container, bao gồm cả đang chạy và stop.

### 1.4.3 Import/Export

+ `docker cp` copy file hoặc thư mục ra khỏi filesystem của container.
+ `docker export` đóng gói filesystem của container vô một tarball

## 1.5 Images

Images chỉ là một template cho docker container, tương tự như khái niệm template của OpenVZ

### 1.5.1 Vòng đời

+ `docker images` hiển thị tất cả các images.
+ `docker import` tạo một image từ tarball.
+ `docker build` tạo một images từ Dockerfile.
+ `docker commit` tạo một image từ một container.
+ `docker rmi` xóa một image.
+ `docker insert` inserts một file từ URL vào image(kind of odd, you'd think images would be immutable after create)
+ `docker load` load một image từ một tar như STDIN, bao gồm cả images và tags (as of 0.7).
+ `docker save` lưu một image vào một file tar từ STDOUT with all parent layers, tags & versions (as of 0.7).

### 1.5.2 Info

+ `docker history` hiển thị history của một images.
+ `docker tags` tags một image thành tên (local hoặc reg).

## 1.6 Dockerfile (là một trong các cách tạo image, nhưng phổ biến nên sẽ nói riêng nó)

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

## 1.7 Docker Index/registry hub.docker.io)

Ở đầu bài tôi có một link giới thiệu tới docker index của tôi. Vấn đề đặt ra là khi tôi tạo một image, file image
đó sẽ chỉ có trên máy chủ đó, trong trường hợp tôi di chuyển qua một máy chủ khác, tôi phải tạo lại một image 
khác mà không đảm bảo là iamge mới sẽ giống cái cũ (quên chẳng hạn). Và để giúp mọi thứ trở lên linh động hơn, cần
có một `kho chung` để quản lý image. Và Docker Index/registry là một nơi như thế.

Về cơ bản gồm 2 phần, phần web giúp thuận tiện cho việc quản lý các image thông qua giao diện, và phần API
cho phép giao tiếp với giao diện dòng lệnh của docker.

Bạn có thể đăng ký một tài khoản trên docker index của dotCloud (free 1 private) hoặc tự dựng cho mình một docker
index riêng để vừa bảo mật, vừa cải thiện tốc độ.

Thông tin về cách cài đặt, cấu hình một docker registry ở [đây](https://github.com/docker/docker-registry), bạn 
có thể tham khảo và cài thử.

![Kiến trúc của Docker](http://i.imgur.com/0IFcxUB.png)

Một số lệnh khi làm việc với Docker Index

+ `docker login` login vào một registry.
+ `docker search` search registry cho image.
+ `docker pull` pulls một image từ registry về local machine.
+ `docker push` push một images tới registry từ local machine.


