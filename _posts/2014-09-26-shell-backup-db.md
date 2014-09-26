---
layout: post
title:  "Backup cơ sở dữ liệu tự động"
date:   2014-09-26
tags: backup, crontab, linux, dabtabase
description: "Backup cơ sở dữ liệu tự động"
---

Bài toán: Hệ thống của bạn sử dụng cơ sở dữ liệu MySQL (giả sử v5.5), bạn có nhu
cầu backup dữ liệu hàng ngày để đề phòng rủi ro mất dữ liệu

Backup chung:

* Nếu không backup dữ liệu, dữ liệu bị hỏng, bạn sẽ mất thông tin, tệ hại hơn 
bạn có thể làm mất thông tin khách hàng, tài chính ảnh hưởng đến uy tín cty.
* Nếu bạn backup dữ liệu và đặt dữ liệu đó trên chính server đó -> nếu server 
đó bị hỏng ổ cứng -> việc backup coi như vô nghĩa
* Thường có 2 loại backup, fullbackup và incremental backup. Full backup nghĩa 
là mỗi lần backup sẽ backup *tất cả mọi thứ*. Incremental backup nghĩa là backup
gia tăng, lần đầu thì fullbackup, các lần sau chỉ backup những phần dữ liệu thay 
đổi.
* Giữ lại quá nhiều bản backup sẽ nhanh chóng làm đầy đĩa, nên giữ lại trong một 
khoảng thời gian bạn cho là an toàn.

Backup CSDL MySQL:

Có nhiều cách để backup MySQL, phổ biến và dễ dùng nhất là `mysqldump`, ngoài ra
có rất nhiều công cụ khác như `mysqlhotcopy`, MySQL Enterprise Backup phiên bản 
phải trả tiền hay `xtrabackup`, các công cụ này có những ưu và nhược điểm riêng. 
Ở đây chủ yếu nói về `mysqldump` và `xtrabackup`

* Nếu csdl của bạn lớn, việc backup sẽ tốn nhiều thời gian
* Nếu bạn dùng `mysqldump` thì cũng nên biết là trong quá trình dump dữ liệu sẽ
bị lock row hoặc lock table tùy loại Storage Engine. User của bạn sẽ không thể
cập nhật gì trên website của bạn. Nếu dữ liệu của bạn nhỏ, backup tốn 1-2ph thì 
không vấn đề gì, những nếu bạn tốn đến tầm 1h thì đó là chuyện rất LỚN.
* Ưu điểm của `mysqldump` là dữ liệu xuất ra nhỏ, có thể restore cho mọi version,
và có thể restore và nhiều schema khác nhau
* Tốt nhất là chạy backup dữ liệu tự động và làm vào ban đêm, lập lịch bằng cách 
sử dụng `crontab` để tránh ảnh hưởng đến user của bạn.

Crontab là một công cụ trên Linux cho phép đặt các lịch thực thi một cái gì đó
tự động. Ví dụ

```bash
	~$ crontab -e
	0 2 * * * /home/xluffy/backupdb.sh
```

Có nghĩa là 2h00 mỗi đêm, sẽ chạy shell-script backupdb

Đơn giản nhất và không phải quan tâm gì nhiều, bạn có thể chỉ cần đặt một crontab
như dưới để backup cơ sở dữ liệu

```bash
	~$ crontab -e
	0 2 * * * /usr/bin/mysqldump -uroot -p'passratngan' github > /home/backup/github.sql
```

Với cách này bạn để lộ password của user root, và mỗi lần backup sẽ đè lại file cũ nên 
bạn có thể làm tốt hơn một xíu bằng cách sửa lại như sau

```bash
	~$ crontab -e
	0 2 * * * /usr/bin/mysqldump -uroot -p'passratngan' github > /home/backup/github_$(date +"%Y-%m-%d").sql
	~$ ls -larth /home/backup/
	github_2014-09-26.sql
	github_2014-09-27.sql
	github_2014-09-28.sql
```

Hoặc làm tốt hơn nữa bằng cách nén dữ liệu lại cho nhỏ bớt.

```bash
	~$ crontab -e
	0 2 * * * /usr/bin/mysqldump -uroot -p'passratngan' github | gzip > /home/backup/github_$(date +"%Y-%m-%d").sql.gz
	~$ ls -larth /home/backup/
	-rw-r--r--   1 root root  4.9K Sep 26 20:52 github_2014-09-26.sql.gz
	-rw-r--r--   1 root root   43K Sep 26 20:52 github_2014-09-26.sql
```

Về cơ bản như vậy là đủ cho việc backup dữ liệu, nhất là các cơ sở dữ liệu 
có kích thước bé thì chỉ cần như vậy.

Dưới đây là một shellscript nhỏ, vô cùng đơn giản

```bash
	#!/bin/bash
	#Simple shell-script backup

	MyUSER="root"     
	MyPASS="passratngan"
	 
	MYSQL="$(which mysql)"
	MYSQLDUMP="$(which mysqldump)"

	ROOT="/home/backup/custom"

	DATE=`date +"%Y-%m-%d"`
	BKDIR="$ROOT/$DATE"

	DBS="github wordpress drupal"

	if [ ! -d "$BKDIR" ]; then
		mkdir -p $BKDIR
	fi
	 
	for db in $DBS
	do
		$MYSQLDUMP -u$MyUSER -p$MyPASS $db | gzip > $BKDIR/$db.sql.gz
	done

	find $ROOT -maxdepth 1 -mindepth 1 -type d -mtime +15 -exec rm -rf {} \;
```

Shell-script này bản chất với 2 crontab phía trên chỉ có 3 điểm khác

* Thay vì phân biệt bằng ngày, giờ tạo thư mục là ngày và có kiểm 
tra thư mục đó có tồn tại hay không (xem ở dưới)
* Vòng lặp backup từng schema ra file riêng
* Lệnh `find` dưới cùng có tác dụng tìm trong $ROOT, loại file là d (thư 
mục) nếu có thời gian modify +15 (15 ngày) thì xóa đi. Nghĩa là chỉ dữ 
lại cơ sở dữ liệu trong vòng nửa tháng

```bash
	~$ ls -larht /home/backup/custom
	drwxr-xr-x 2 root root 4096 Sep 10 00:00 2014-09-10
	drwxr-xr-x 2 root root 4096 Sep 11 00:00 2014-09-11
	drwxr-xr-x 2 root root 4096 Sep 12 00:00 2014-09-12
	drwxr-xr-x 2 root root 4096 Sep 13 00:00 2014-09-13
	drwxr-xr-x 2 root root 4096 Sep 14 00:00 2014-09-14
	drwxr-xr-x 2 root root 4096 Sep 15 00:00 2014-09-15
	drwxr-xr-x 2 root root 4096 Sep 16 00:00 2014-09-16
	drwxr-xr-x 2 root root 4096 Sep 17 00:00 2014-09-17
	drwxr-xr-x 2 root root 4096 Sep 18 00:00 2014-09-18
	drwxr-xr-x 2 root root 4096 Sep 19 00:00 2014-09-19
	drwxr-xr-x 2 root root 4096 Sep 20 00:00 2014-09-20
	drwxr-xr-x 2 root root 4096 Sep 21 00:00 2014-09-21
	drwxr-xr-x 2 root root 4096 Sep 22 00:00 2014-09-22
	drwxr-xr-x 2 root root 4096 Sep 23 00:00 2014-09-23
	drwxr-xr-x 2 root root 4096 Sep 24 00:00 2014-09-24
	drwxr-xr-x 2 root root 4096 Sep 25 00:00 2014-09-25
	drwxr-xr-x 2 root root 4096 Sep 26 00:00 2014-09-26
```

Sau đó bạn lưu shell-script trên thành 1 file dạng backupdb.sh và bỏ vào
`/home/backup/bin` hoặc ở đâu tùy bạn và thiết lập một crontab như sau

```bash
	~$ crontab -e
	0 2 * * * /home/backup/bin/backupdb.sh 
```








