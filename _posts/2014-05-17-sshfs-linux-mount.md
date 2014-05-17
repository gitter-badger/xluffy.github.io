---
layout: post
title:  "Sử dụng sshfs để mount ổ đĩa"
date:   2014-05-17
tags: ssh, sshfs, linux
description: "Sử dụng sshfs để mount ổ đĩa"
---

Thông thường để chuyển dữ liệu từ một remote server về local machine ta có nhiều các:

- Sử dụng FTP -> phải cấu hình, lâu và ko bảo mật do ko có mã hóa
- Sử dụng scp -> nhanh nhưng chỉ nên dùng chuyển file nhỏ và mỗi lần có nhu cầu truyền file lại phải gõ lệnh
- Sử dụng rsync -> phải có tài khoản ssh nếu muốn rsync over ssh, nếu không phải cấu hình rsync daemon
- Sử dụng NFS -> phải cài đặt cả client và server

Đối với Windows có thể truyền file bằng cách:

- Dùng WinSCP thông qua SSH
- Dùng Samba -> phải cấu hình dịch vụ và tạo account cho mội người.

Nếu có nhu cầu sử dụng một thư mục trên remote server như trên chính máy mình mà ko mất công phải tạo tài khoản
nhiều cũng như bảo mật có thể sử dụng sshfs.

sshfs thực chất là một cách dùng để mount một thư mục về local machine thông qua ssh, nghĩa là có tài khoản ssh là có
thể mount bất cứ thư mục nào (ko như nfs phải cấu hình trong exports để chỉ định thư mục cần chia sẻ)

Trên Arch Linux có thể cài đặt và sử dụng như sau:

```
	~$ pacman -S sshfs
	~$ modprobe fuse
	~$ mkdir ~/backup
	~$ sshfs xquang@remote-server:/shaer ~/backup -p 9876
```

Cũng giống như giao thức ssh, nếu sử dụng một port khác để ssh bạn cũng có thể sử dụng option -p để chỉ định port.

Từ bây giờ có thể thao tác thư mục đó như trên chính máy mình.