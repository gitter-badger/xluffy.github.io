---
layout: post
title:  "Một vài lệnh thú vị trên Linux"
date:   2014-05-18
tags: cmd, command line
description: "Một vài lệnh thú vị trên Linux"
---

Việc sử dụng Linux và thao tác công việc với các cmd của Linux là khá thường xuyên, tuy nhiên
không phải ai cũng biết sức mạnh và sự thú vị của các câu lệnh trên Linux.

Dưới đầy là vài câu lệnh tôi hay sử dụng và vài câu khá thú vị muốn chia sẻ.

### Liệt kê file mới chỉnh sửa gần đây nhất.

Trên Linux, lệnh `ls` được sử dụng để liệt kê file, ta hay dùng các option như `-a` để hiển thị file
ẩn, `-l` để liệt kê theo cột, `-h` để liệt kê với kích thước có thể đọc dễ dàng. Tuy nhiên tôi hay áp dụng lệnh này.

```bash
    ~$ ls -larth
```

Thêm hai option `-t` để liệt kê file theo thời gian modify, file mới sửa gần nhất sẽ nằm trên cùng, tuy nhiên nếu có quá nhiều file thì lại mất công cuộn chuột lên, `-r` sẽ đảo ngược lại, nghĩa là file mới sửa
gần nhất sẽ nằm dưới cùng.

### Không lưu history

Mọi thao tác lệnh trên Linux sẽ được ghi lại, tùy thuộc vào kích thước history ta thiết lập, tuy nhiên ta có
thể chạy một lệnh mà không lưu lại history bằng cách thêm một dấu `<space>` trước lệnh đó

```bash
    ~$ <space>ll
    ~$ history
```

### Chạy lệnh cuối cùng như root

```bash
    ~$ sudo !!
```

### Tạo một file với kích thước lớn

Ví dụ bạn cần tạo một file với kích thước 10GB để kiểm tra, cách đơn giản là làm sao đẩy dữ liệu vào đủ 10GB, hoặc tải 1 bộ film 10GB. Nhưng các cách đó thì quá lâu, đơn giản ta có thể sử dụng lệnh sau để tạo ra các file có kích thước lớn.

```bash
	~$ truncate -s 10GB bigfile.a
	~$ fallocate -l 10GB bigfile.a
	~$ dd if=/dev/zero of=filename bs=1 count=0 seek=10G
```

### Xem lịch sử login vào hệ thống

```bash
	~$ last
	xquang   pts/0        14.19.16.220   Sun Sep  7 07:30   still logged in
	xquang   pts/0        14.166.136.225     Sat Sep  6 17:30 - 18:01  (00:31)
	xquang   pts/0        14.166.136.225     Sat Sep  6 17:00 - 17:00  (00:00)
	xquang   pts/0        14.166.136.225     Sat Sep  6 16:22 - 16:53  (00:30)
```

```bash
	~$ lastlog
	Username         Port     From             Latest
	root             pts/0    10.200.0.9       Thu Sep  4 00:52:36 +0700 2014
	bin                                        **Never logged in**
	daemon                                     **Never logged in**
```