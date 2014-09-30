---
layout: post
title:  "Những lệnh ngắn, hữu ích khi quản trị"
date:   2014-09-07
tags: cmd, command line, linux
description: "Những lệnh ngắn, hữu ích khi quản trị"
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

Ví dụ bạn cần tạo một file với kích thước 10GB để kiểm tra, cách đơn giản là làm 
sao đẩy dữ liệu vào đủ 10GB, hoặc tải 1 bộ film 10GB. Nhưng các cách đó thì quá 
lâu, đơn giản ta có thể sử dụng lệnh sau để tạo ra các file có kích thước lớn.

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

### Xóa các dòng trống `blank line`

```
	~$ grep -v "^#" /etc/ssh/sshd_config | sed '/^$/d'
```

Diễn giải: lệnh sed sẽ tìm các dòng bắt đầu `^` và kết thúc `$` không có ký tự nào
 rồi xóa `d`

### Find

Có một đám log nén bỏ lung tung các thư mục, giờ muốn gom về một chỗ để lưu trữ

```bash
	~$ find . -name php-fpm*.tgz -exec mv {} archive/ \;
```

Có một đống các thư mục tên `log` nằm rải rác trong các thư mục con, giờ muốn xóa đi

```bash
	~$ find . -name "log" -exec rm -rf {} \;
```

### Kill một session đang login

Ví dụ bạn lên DataCenter, login vào và quên logout ra, bạn remote vào và thấy kết 
quả như sau

```bash
	~$ w
	23:58:07 up  6:23,  2 users,  load average: 0.00, 0.02, 0.01
	USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
	root     tty1     -                17:35    5:47m  0.19s  0.19s -bash
	root     pts/1    10.100.9.1       18:12    0.00s  0.03s  0.02s w
```

Để ý thì thấy tty1 chính là session login ở trên, giờ cần kill nó đi để lỡ ai có 
cắm Monitor vào cũng ko thể login được

```bash
	~$ ps -fu root| grep [b]ash
	root      1218  1195  0 17:35 tty1     00:00:00 -bash
	root     13123 13120  0 18:12 pts/1    00:00:00 -bash
```

Và biết `pid` của session đó là `1218` và giờ chỉ cần `~$ kill -9 1218` là xong

### Lọc bằng grep, AND, OR

OR
```bash
	~$  cat access.log | grep '115.78.228.58\|118.69.34.213
```

AND
```bash
	~$  cat access.log | grep 115.78.228.58 | grep 118.69.34.213
```

### Umount lazy

Bạn muốn umount như nó cứ báo

```bash
	~$ umount /mnt
	umount.nfs: /mnt: device is busy
	umount.nfs: /mnt: device is busy
```

```bash
	lsof | grep /mnt/                                                                                                                                         
	bash      16625       xluffy  cwd       DIR               0,20       4096  109314049 /mnt/abc (x.x.x.x:/mnt)

```

Lười quá

```bash
	~$ umount -f -l /mnt
```
