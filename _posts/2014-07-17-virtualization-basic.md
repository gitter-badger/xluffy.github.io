---
layout: post
title:  "Cơ bản về ảo hóa cho người mới bắt đầu"
author: xluffy
date:   2014-07-16
tags: virtualization, xen, kvm, openvz
description: "Cơ bản về ảo hóa cho người mới bắt đầu"
---

## Why?

Tôi có từng xài qua vmware workstation, virtual-box lúc còn là sinh viên, sau này đi làm thì 
lại sử dụng KVM và OpenVZ. Ngoài ra tôi cũng biết những _cái_ ảo hóa khác như Xen hay công 
nghệ của M$.

Tuy nhiên thời gian gần đây tôi có đọc về Docker, tôi nhận ra là trước giờ mình chỉ hoàn toàn 
sử dụng chứ chưa có chút kiến thức căn bản nào về ảo hóa. Vì vậy tôi dành chút thời gian để
tìm hiểu về nó và có trao đổi với anh VietStack nhờ anh giúp đỡ một số tài liệu cơ bản.

## Những nhập nhằng?

1. Tôi không phân biệt được các sản phẩm của vmware (workstation, fusion, ESX, vSpere), không phân
biệt được Xen Server bản OpenSource và bản thương mại

2. Đánh đồng OpenVZ tương tự như KVM (điển hình là thắc mắc tại sao OpenVZ lại tạo _máy ảo_ nhanh hơn KVM)

3. Không phân biệt được sự khác nhau giữa Container với VM

4. Không hiểu khái niệm Hypervisor

## Phân loại về công nghệ ảo hóa

### Full Virtualization

+ Mã nguồn mở: KVM, Virtual-Box, KQEMU
+ Thương mại: VMware, Hyper-V 

### Para Virtualization

+ Xen, VMware

### OS-Level Virtualization

+ OpenVZ
+ Linux-VServer
+ Docker
+ Chroot (jail, LXC..)

## Hướng tiếp cận ảo hóa

### Host-Architecture

- Được cài đặt như một phần mềm
- Dựa vào OS của máy vật lý đển quản lý các máy ảo và tài nguyên

![Host-Architecture](http://i.imgur.com/BMFYAeF.png)

### Bare-Metal (Hypervisor) Architecture

- Ảo hóa từ lõi, trong kernel
- Cài đặt trực tiếp trên phần cứng

![Bare-Metal](http://i.imgur.com/hKTvbqs.png)

## Phân biệt OS-Level và các loại Hypervisor

### OS-Level Virtualization

- Là một phương thức ảo hóa server, nơi mà kernel của một hệ điều hành cho phép
tạo dựng nhiều môi trường độc lập (mutile isolated user-space instance)

- Instance (hay container, virtualization-engine, jail) nhìn giống như server thật

- Tạm hiểu nó là kỹ thuật siêu cấp của chroot (ai xài Linux sẽ hiểu), nhưng cấp 
kernel sẽ làm vài thứ để đảm bảo container này không ảnh hưởng đến container khác.

- Các giải pháp: Linux (chroot, Docker, LXC, OpenVZ, Jail), Windows (Parallels Virtuozzo Container,
Sanboxee)

### Hypervisor

- Hypervisor hay còn gọi là Virtual-Machine-Manager (VMM) là một phần mềm máy tính, firmware, hardware
giúp tạo, quản lý và chạy máy ảo.

- Phân loại

	- Type-I Hypervisor (native, bare-metal): là loại hypevisor chạy trực tiếp trên phần cứng, điều khiển
	quản lý và cấp phát các máy ảo => Citrix XenServer, VMware ESX/ESXi, M$ Hyper-V.
	
	- Type-II Hypervisor(hosts): chạy như một phần mềm trên một OS chính => VMware Workstation, Virtual-Box
	Virtula-PC.
	
	![Hypervisor](http://i.imgur.com/q1KX9dn.png)
	
### Câu trả lời

1. Tại sao OpenVZ tạo máy ảo nhanh => vì bản chất OpenVZ là Container chứ ko phải dạng ảo hóa như KVM


### Issue

Còn 1 vấn đề chưa rõ
...