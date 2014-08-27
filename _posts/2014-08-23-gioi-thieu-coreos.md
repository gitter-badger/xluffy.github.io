---
layout: post
title:  "[CoreOS-Series] [1] Giới thiệu cơ bản về CoreOS"
author: xluffy
date:   2014-08-23
tags: coreos, docker
description: "[CoreOS-Series] [1] Giới thiệu cơ bản về CoreOS"
---

### Giới thiệu

CoreOS được thiết kế cho mục đích bảo mật, ổn định và đánh tin cậy. Thay vì cài đặt các gói phần mềm qua `yum` hoặc `apt`, CoreOS sử dụng
Linux containers để quản lý các dịch vụ của bạn ở một mức độ trừu tượng cao hơn. Một dịch vụ duy nhất và tất cả các gói phụ thuộc sẽ được
đónh gói vào một container và có thể chạy trên một hoặc nhiều máy CoreOS.

Linux Container mang tới một lợi ích tương tự như một máy ảo thuần (virtual machines) nhưng nó tập trung vào ứng dụng thay vì ảo hóa toàn 
bộ máy chủ. Kể từ khi container khôn còn chạy trên chính Linux kernel của nó hoặc không yêu cầu một lớp ảo hóa, nó gần như không tốn chi phí
để thực hiện. 

CoreOS chạy được trên hầu hết các nền tảng bao gồm Vagrant, Amazon EC2, QEMU / KVM, VMware, OpenStack và phần cứng của riêng bạn. 

Lướt qua mỗi block của CoreOS — etcd, docker and systemd.

### Run Services with docker

![Three-Tier-Webapp](https://coreos.com/assets/images/media/Three-Tier-Webapp.png)

Khối xây dựng chính của CoreOS là `docker`, một Linux container, nơi đặt ứng dụng của bạn và chạy. Docker được cài trên mỗi máy CoreOS. Bạn
nên xây dựng mỗi container cho mội dịch vụ của bạn (web server, caching, database) bắt đầu chúng với `fleet` và kết nối chúng lại với nhau
bằng cách đọc và ghi vào `etcd`

CoreOS không có một package manager - bất kỳ phần mềm nào muốn chạy phải chạy thông qua container. Bạn có thể nhanh chóng thử một Ubuntu
container từng bước bằng cách đọc bài viết giới thiệu về `docker` tại [đây](http://xluffy.github.io/docker-notes/). Mục tiêu cuối cùng là xây dựng một hệ thống mà kết quả là
container như một hệ thống vật lý thông thường. CoreOS sử dụng `systemd` và `fleet` để quản lý các containers 



