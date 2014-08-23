---
layout: post
title:  "Hiểu output của lệnh Top tên Linux"
author: xluffy
date:   2014-05-09
tags: tips, top, linux
description: "Hiểu output của lệnh Top tên Linux"
---

Lệnh #top là một lệnh khá hữu ích và phổ biến trên Linux nhằm mục đích theo dõi các resource của hệ thống.
Nó cung cấp khá nhiều thông tin, vậy những thông tin đó có ý nghĩa như thế nào và cách đọc các thông số đó
để đoán được tình trạng máy chủ của bạn.

```bash
	top - 22:33:34 up 5 days,  1:52,  2 users,  load average: 0.54, 0.52, 0.75
	Tasks: 153 total,   2 running, 151 sleeping,   0 stopped,   0 zombie
	%Cpu(s):  5.8 us,  1.7 sy,  2.9 ni, 89.4 id,  0.2 wa,  0.0 hi,  0.1 si,  0.0 st
	KiB Mem:   5974680 total,  5044324 used,   930356 free,   163672 buffers
	KiB Swap:  4000148 total,        0 used,  4000148 free.  2342656 cached Mem

	  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
	  346 root      19  -1  259412  84904  58844 S   6.3  1.4 139:02.54 X
	  351 xluffy    25   5 1109780 112240  29092 S   2.0  1.9   1:34.27 chromium
	 8274 xluffy    25   5 1109996 105416  27048 S   1.7  1.8   0:05.41 chromium
	11019 xluffy    20   0 1208788 198780  28192 S   1.0  3.3   0:15.00 chromium
	24460 xluffy    25   5 1752396 476248  32704 S   1.0  8.0   7:16.01 chromium
	 5065 xluffy    23   3  983920 202724  54920 S   0.7  3.4  64:50.91 chromium
	 6134 xluffy    25   5 1253536 169748  32496 S   0.7  2.8   1:57.78 chromium
		7 root      20   0       0      0      0 S   0.3  0.0   4:04.44 rcu_preempt
	   14 root      20   0       0      0      0 S   0.3  0.0   0:28.81 ksoftirqd/1
	  357 xluffy    20   0   71540   7552   3352 S   0.3  0.1   0:05.33 uim-xim
	 5188 xluffy    25   5 1093772  93832  26200 S   0.3  1.6   0:33.70 chromium
	 7903 xluffy    23   3   20536   2716   1344 S   0.3  0.0   4:47.67 tmux
	 9196 xluffy    25   5 1129884 125724  27212 S   0.3  2.1   0:08.79 chromium
	11600 root      20   0       0      0      0 S   0.3  0.0   0:00.41 kworker/0:2
	12673 xluffy    23   3   17452   1636   1136 R   0.3  0.0   0:00.01 top
		1 root      20   0   25784   3344   1844 S   0.0  0.1   0:01.33 systemd
		2 root      20   0       0      0      0 S   0.0  0.0   0:00.02 kthreadd
		3 root      20   0       0      0      0 S   0.0  0.0   0:45.32 ksoftirqd/0
		5 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/0:0H
		8 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcu_sched
		9 root      20   0       0      0      0 S   0.0  0.0   0:00.00 rcu_bh
	   10 root      rt   0       0      0      0 S   0.0  0.0   0:00.47 migration/0
	   11 root      rt   0       0      0      0 S   0.0  0.0   0:00.31 watchdog/0
	   12 root      rt   0       0      0      0 S   0.0  0.0   0:00.29 watchdog/1
	   13 root      rt   0       0      0      0 S   0.0  0.0   0:00.44 migration/1
	   16 root       0 -20       0      0      0 S   0.0  0.0   0:00.00 kworker/1:0H
	   17 root      rt   0       0      0      0 S   0.0  0.0   0:00.29 watchdog/2

```

Phía trên là một phần output của lệnh `top` được copy ra, giờ chúng ta sẽ phân tích nó.

### Dòng đầu tiên

```bash
	top - 22:33:34 up 5 days,  1:52,  2 users,  load average: 0.54, 0.52, 0.75
```

- Tên của lệnh top
- Thời gian hiện tại
- Uptime hệ thống cho biết hệ thống đã bật được bao lâu
- 2 user: số lượng user login vào hệ thống, ở đây có 2 user xluffy login đồng thời tạo ra 2 tiến trình qua lệnh `w` như sau

```bash
	USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
	xluffy   tty1      Sun20    5days 25:58   0.00s xinit /home/xluffy/.xinitrc --\
	/etc/X11/xinit/xserverrc :0 -auth /tmp/serverauth.km2tbUYBHP
	xluffy   pts/1     Wed20    2days  0.00s  0.00s /bin/bash

```

- load average: thể hiện 3 con số là độ tải trung bình của hệ thống trong 1, 5, 15 phút.

### Dòng thứ hai 

```bash
	Tasks: 153 total,   2 running, 151 sleeping,   0 stopped,   0 zombie
```
Thể hiện tổng số tiền trình và trạng thái của chúng.

### Dòng thứ ba

```bash
	%Cpu(s):  5.8 us,  1.7 sy,  2.9 ni, 89.4 id,  0.2 wa,  0.0 hi,  0.1 si,  0.0 st
	KiB Mem:   5974680 total,  5044324 used,   930356 free,   163672 buffers
	KiB Swap:  4000148 total,        0 used,  4000148 free.  2342656 cached Mem
```

Hiện thị chi tiết việc sử dụng CPU

- 5.8 us: là user-processes đang sử dụng 5.8%
- 1.7 sy: là system-processes đang sử dụng là 1.7%
- 89.4 id: phần trăm số CPU còn ở trạng thái rỗi
- 0.2 wa: là thời gian CPU chờ từ I/O

Khi phân tích các thông số của CPU, đầu tiên là xem số phần trăm CPU ở trạng thái rỗi là bao nhiêu. Nếu %id thấp thì cần xem tiếp
các thông số %us, %sy, %wa để xác định CPU đang làm gì là %id lại thấp.

### Dòng số 4 và số 5

```bash
	KiB Mem:   5974680 total,  5044324 used,   930356 free,   163672 buffers
	KiB Swap:  4000148 total,        0 used,  4000148 free.  2342656 cached Mem
```

Mô tả việc sử dụng bộ nhớ. Những con số này có thể gây nhầm lầm. 5974680 total là tổng số bộ nhớ của hệ thống, 5044324 used là một phần
bộ nhớ RAM hiện đang chứa thông tin. 930356 free là một phần bộ nhớ RAM đang không chứa thông tin. 163672 buffers và 2342656 cached là phần 
đệm và cache data cho I/O.

Câu hỏi là số RAM còn free cho các chương trình sử dụng là bao nhiêu.
Câu trả lời là:  free + (buffers + cached)

930356 + 163672 + 2342656 = 3436684 ~ 3.3GB 

Và bao nhiêu bộ nhớ RAM đang được sử dụng bởi các chương trình:

Câu trả lời là: used – (buffers + cached)

5044324 - (163672 + 2342656) = 2537996 ~ 2.4GB

### Thông tin các process

```bash
	PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
	346 root      19  -1  259412  84904  58844 S   6.3  1.4 139:02.54 X
	351 xluffy    25   5 1109780 112240  29092 S   2.0  1.9   1:34.27 chromium
```

- PID – process ID của một tiến trình
- USER – User nào đang chạy proces đó
- PR – Độ ưu tiên của process đó 
- NI – Nice là một giá trị của process, (càng cao nghĩa là độ ưu tiên càng thấp)
- VIRT – Tổng số lượng bộ nhớ ảo được sử dụng 
- RES – Resident task size
- SHR – Số lượng bộ nhớ chia sẻ được sử dụng
- S – Trạng thái của tiến trình. S là (sleeping), D là (uninterruptible sleep), R là (running), Z là (zombies), và T (stopped or traced)
- %CPU – Phần trăm CPU sử dụng
- %MEM – Phần trăm RAM sử dụng
- TIME+ – Tổng số thời gian CPU đc sử dụng.
- COMMAND – Lệnh sử dụng.

### Học cách sử dụng lệnh `top`

- M – Sắp xếp theo số RAM sử dụng 
- P – Sắp xếp theo số CPU sử dụng
- T – Sắp xếp theo thời gian tích lũy 
- z – Hiển thị màu 
- k – Kill một tiến trình
- q – thoát

Nếu bạn muốn kill một tiến trình với PID là 3161, nhấn "k" và điền vào số PID đc yêu cầu.

### Tùy chọn

- d – Số s sẽ refresh để cập nhật
- p – Chi tiết một process có pid mà muốn monitor
- n – Cập nhật sau bao lâu và thoát
