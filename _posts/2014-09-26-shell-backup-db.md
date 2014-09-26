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
có rất nhiều công cụ khác như `mysqlhotcopy`, MySQL backup enterprise - trả tiền
hay `xtrabackup`, các công cụ này có những ưu và nhược điểm riêng. Ở đây chủ yếu 
nói về `mysqldump` và `xtrabackup`

* Nếu csdl của bạn lớn, việc backup sẽ tốn nhiều thời gian
* Nếu bạn dùng `mysqldump` thì cũng nên biết là trong quá trình dump dữ liệu sẽ
bị lock row hoặc lock table tùy loại storage engine. User của bạn sẽ không thể
cập nhật gì trên website của bạn. Nếu dữ liệu của bạn nhỏ, backup tốn 1-2ph thì 
không vấn đề gì, những nếu bạn tốn đến tầm 1h thì đó là chuyện rất LỚN.
* Ưu điểm của `mysqldump` là dữ liệu xuất ra nhỏ, có thể restore cho mọi version,
và có thể restore và nhiều schema khác nhau
* Tốt nhất là chạy backup dữ liệu tự động và làm vào ban đêm, lập lịch bằng cách 
sử dụng `crontab`

Crontab là một công cụ trên Linux cho phép đặt các lịch thực thi một cái gì đó
tự động. Ví dụ

```bash
	~$ crontab -e
	0 2 * * * /home/xluffy/backupdb.sh
```

Có nghĩa là 2h00 mỗi đêm, sẽ chạy shell-script backupdb




