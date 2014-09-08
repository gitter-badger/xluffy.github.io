---
layout: post
title:  "Những lệnh monitor hệ thống"
date:   2014-09-08
tags: vmstat, dstat, mpstat
description: "Những lệnh monitor hệ thống"
---


Mô tả một số lệnh hữu ích trong việc phân tích trạng thái của server

Một số lệnh dễ xem như top, htop, free và các thông số kèm theo sẽ không mô tả ở đâu

## vmstat

Lệnh này là một lệnh khá thú vị và được dùng khá nhiều. Nó mô tả trạng thái của hệ thống tại một thời điểm

Cú pháp: 

```bash
	~$ vmstat [interval] [count]
```

  * interval: Khoảng thời gian report- default là 30s count
  * count Số lần report

Ví dụ:

```bash
	~$ time vmstat 10 2
	procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu-----
	 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
	 3  0  35500 575800 2999768 20569296    0    0     4    86    0    0  5  0 95  0  0
	 0  0  35500 440660 2999744 20683472    0    0 12001  3436 40687 26786 12  2 86  0  0

	real    0m10.002s
	user    0m0.001s
	sys     0m0.001s
```

như trên thì có 2 report được xuất ra, và tốn tổng cộng 10s để xuất 2 report đó, lấy report 10s/ lần, và chỉ lấy 2 report

Mặc định sẽ xuất ra dạng Kilobyte, để xuất ra Megabyte sử dụng -S M

```bash
	~$ vmstat 5 2 -S M
	procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu-----
	 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
	 5  0     34    300   2929  20304    0    0     4    86    0    0  5  0 95  0  0
	 8  0     34    265   2929  20304    0    0    42  1610 40385 26602 20  2 78  0  0
```

## procs

Dữ liệu procs báo cáo số processing jobs chờ để run và cho phép bạn xác định xem có các process làm cho hệ thống của bạn bị chậm lại hay ko?

    * Cột r hiển thị tổng số processing jobs chờ để truy cập vào processor. Nếu quan sát nhiều lần và thấy giá trị này vào khoảng = 2 lần CPU  có hiện tượng thắt cổ chai
    * Cột b hiển thị tổng số processes trong trạng thái ‘sleep’

Nếu các giá trị của các cột r và b thường xuyên cao (=2%%*%%CPU), điều đó có nghĩa hệ thống không có đủ cpu hoặc memory hoặc i/o bandwidth.

## mpstat

Thống kê chi tiết tình trạng sự dụng CPU

```bash
	~$ mpstat
	Linux 2.6.32-358.18.1.el6.x86_64 ()  09/08/2014      _x86_64_        (24 CPU)

	01:50:58 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest   %idle
	01:50:58 PM  all    4.53    0.00    0.34    0.05    0.00    0.13    0.00    0.00   94.94
```

    * CPU: mã số của từng core trên CPU, All là giá trị trung bình
    * %usr: phần trăm sử dụng CPU ở mức user level (applicaton)
    * %nice: phần trăm sử dụng CPU ở mức user level với độ ưu tiên
    * %sys: phần trăm sử dụng CPU ở mức hệ thống, kernel space
    * %iowait:
    * %irq:
    * %idle:

Hiện thị thống kê sử dụng tất cả các core CPU tại một thời điểm

```bash
	~$ mpstat -P ALL
	Linux 2.6.32-358.18.1.el6.x86_64 ()  09/08/2014      _x86_64_        (24 CPU)

	01:59:54 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest   %idle
	01:59:54 PM  all    4.53    0.00    0.34    0.05    0.00    0.13    0.00    0.00   94.94
	01:59:54 PM    0   20.96    0.00    0.98    0.58    0.00    0.24    0.00    0.00   77.25
	01:59:54 PM    1   11.43    0.00    0.77    0.13    0.00    0.19    0.00    0.00   87.47
	01:59:54 PM    2    8.27    0.00    0.69    0.08    0.00    0.22    0.00    0.00   90.74
	01:59:54 PM    3    7.68    0.00    0.68    0.05    0.00    0.25    0.00    0.00   91.33
	01:59:54 PM    4    8.37    0.00    0.73    0.04    0.00    0.31    0.00    0.00   90.55
	01:59:54 PM    5    9.13    0.00    0.75    0.03    0.00    0.36    0.00    0.00   89.72
	01:59:54 PM    6    5.08    0.00    0.20    0.04    0.00    0.03    0.00    0.00   94.65
	01:59:54 PM    7    1.04    0.00    0.07    0.02    0.00    0.01    0.00    0.00   98.86
	01:59:54 PM    8    0.34    0.00    0.04    0.01    0.00    0.01    0.00    0.00   99.60
	01:59:54 PM    9    0.20    0.00    0.03    0.00    0.00    0.01    0.00    0.00   99.75
	01:59:54 PM   10    0.15    0.00    0.02    0.00    0.00    0.00    0.00    0.00   99.82
	01:59:54 PM   11    0.13    0.00    0.01    0.00    0.00    0.00    0.00    0.00   99.85
	01:59:54 PM   12    1.76    0.00    0.31    0.09    0.00    0.04    0.00    0.00   97.80
	01:59:54 PM   13    4.53    0.00    0.42    0.04    0.00    0.20    0.00    0.00   94.80
	01:59:54 PM   14    5.35    0.00    0.47    0.03    0.00    0.25    0.00    0.00   93.90
	01:59:54 PM   15    6.61    0.00    0.54    0.03    0.00    0.30    0.00    0.00   92.51
	01:59:54 PM   16    7.97    0.00    0.63    0.03    0.00    0.39    0.00    0.00   90.98
	01:59:54 PM   17    8.73    0.00    0.67    0.03    0.00    0.43    0.00    0.00   90.14
	01:59:54 PM   18    0.58    0.00    0.06    0.00    0.00    0.00    0.00    0.00   99.36
	01:59:54 PM   19    0.19    0.00    0.03    0.00    0.00    0.00    0.00    0.00   99.78
	01:59:54 PM   20    0.11    0.00    0.02    0.00    0.00    0.00    0.00    0.00   99.86
	01:59:54 PM   21    0.09    0.00    0.02    0.00    0.00    0.00    0.00    0.00   99.89
	01:59:54 PM   22    0.08    0.00    0.02    0.00    0.00    0.00    0.00    0.00   99.90
	01:59:54 PM   23    0.07    0.00    0.01    0.00    0.00    0.00    0.00    0.00   99.91
```

Tương tự như vậy để hiển thị core thứ 5

```bash
	~$ mpstat -P 5
	Linux 2.6.32-358.18.1.el6.x86_64 ()  09/08/2014      _x86_64_        (24 CPU)

	02:00:16 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest   %idle
	02:00:16 PM    5    9.13    0.00    0.75    0.03    0.00    0.36    0.00    0.00   89.72
```

Hiển thị 5 report, mỗi 2s một lần của core thứ 5

```bash
	~$ mpstat -P 5 2 5
	Linux 2.6.32-358.18.1.el6.x86_64 ()  09/08/2014      _x86_64_        (24 CPU)

	02:00:54 PM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest   %idle
	02:00:56 PM    5    5.03    0.00    0.00    0.00    0.00    0.50    0.00    0.00   94.47
	02:00:58 PM    5   36.68    0.00    0.50    0.00    0.00    0.00    0.00    0.00   62.81
	02:01:00 PM    5   12.50    0.00    0.50    0.50    0.00    0.00    0.00    0.00   86.50
	02:01:02 PM    5   11.00    0.00    1.50    0.00    0.00    0.00    0.00    0.00   87.50
	02:01:04 PM    5   14.14    0.00    1.52    0.00    0.00    0.00    0.00    0.00   84.34
	Average:       5   15.86    0.00    0.80    0.10    0.00    0.10    0.00    0.00   83.13
```

Một vài tip

    * Nếu %usr cao, có nghĩa là ứng dụng của bạn (user-space, php-fpm) đang tiêu thụ một lượng lớn CPU và đang trở lên quá tải
    * Nếu %sys cao, có nghĩa là máy chủ của bạn đang tiêu thụ một lượng tài nguyên lớn bởi các ứng dụng từ hệ thống (kernel)


