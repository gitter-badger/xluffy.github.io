---
layout: post
title:  "Các thông số time out của MySQL"
date:   2014-03-29
tags: timeout, mysql, linux, 
description: "Các thông số time out của MySQL"
---

### Các thông số

Để show các thông số time-out của mysql ta đăng nhập vào cửa sổ dòng lệnh của mysql và gõ 
`show variables like "%timeout%";`

```
	show variables like "%timeout%";
	+----------------------------+-------+
	| Variable_name              | Value |
	+----------------------------+-------+
	| connect_timeout            | 10    |
	| delayed_insert_timeout     | 120   |
	| innodb_lock_wait_timeout   | 50    |
	| innodb_rollback_on_timeout | OFF   |
	| interactive_timeout        | 28800 |
	| lock_wait_timeout          | 60    |
	| net_read_timeout           | 60    |
	| net_write_timeout          | 60    |
	| slave_net_timeout          | 3600  |
	| wait_timeout               | 28800 |
	+----------------------------+-------+
	10 rows in set (0.00 sec)
```

Tại sao có nhiều tham số timeout của MySQL đến vậy và vai trò của nó ứng với các trường hợp như thế nào?

MySQL sử dụng nhiều tham số timeout khác nhau cho các trạng thái hoạt động khác nhau. Khi bắt đầu kết nối giữa client và MySQL Server, `connection_timeout` được sử dụng. Khi server chờ một query nào đó được gởi 
đến nó thì `wait_timeout` được sử dụng. Nếu query bắt đầu được đọc xử lý xong và gởi ngược lại cho ứng dụng thì `net_read_timeout` và `net_write_timeout` được sử dụng.

`innodb_lock_wait_timeout` được sử dụng khi các table có engine là Innodb bắt đầu bị lock (ở chế độ lock rows). `delayed_insert_timeout` được sử dụng khi query insert bị delay . `slave_net_timeout` được sử dụng trong trường 
hợp replication khi slave đang đọc dữ liệu từ master.

### Các tham số timeout MySQL và ý nghĩa của nó
#### connect_timeout:

Thời gian (tính bằng giây) mà MySQL server chờ đợi cho các gói tin kết nối trước khi trả về Bad handshake . Từ MySQL 5.1.23, mặc định giá trị này là 10, các phiên bản trước giá trị này là 5. Tăng giá trị `connect_timeout`
có thể giúp được trong trường hợp ứng dụng tốn nhiều thời gian để kết nối với MySQL (MySQL và ứng dụng không cùng chung trên một server, cách xa nhau) hoặc bị lỗi Lost connection to MySQL server .

#### delayed\_insert\_timeout:

Tham số timeout trong trường hợp sư dụng cú pháp INSERT DELAYED . Ví dụ `INSERT DELAYED INTO table (id) VALUES (123)`. Với cú pháp INSERT DELAYED, MySQL sẽ không chờ đến khi insert xong mới response về cho client mà sẽ response ngay lập
tức. Lúc này câu lệnh này sẽ được lưu vào hàng đợ trên RAM và sẽ được thực thi khi đến lược. Nếu quá thời gian `delay_insert_timeout` mà truy vấn tại hàng đợi vẫn chưa được thực thi, truy vấn sẽ bị huỷ.

#### innodb\_lock\_wait\_timeout:

Tham số timeout chỉ dành cho các table sử dụng engine Innodb. Tham số này là thời gian tối đa mà các transaction Innodb có thể chờ khi row bị lock. Giá trị mày mặc định là 50 giây. Một transaction Innodb cố gắng truy cập vào một row đang ở
trạng thái lock bởi một Innodb transaction khác sẽ bị huỷ nếu quá thời gian timeout chờ đợi này, lúc đó lỗi trả về sẽ là:

```
	ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
```

#### interactive_timeout:

Thời gian tối đa (theo giây) mà server chờ đợi cho các hoạt động trên một kết nối tương tác (interactive connection) trước khi đóng chúng. Một client tương tác (interactive client) là client sử dụng tuỳ chọn CLIENT\_INTERACTIVE) cho mysql\_real\_connect().

#### net\_read\_timeout:

Thời gian tối đa(tính theo giây) mà server chờ đợi dữ liệu khi đã kết nối trước khi ngắt. MySQL phiên bản trước 5.1.41, timeout này chỉ áp dụng cho timeout của kết nối TCP/IP, không áp dụng cho Unix socket, named pipes hay shared memory.

#### net\_write\_timeout:

Khi server trả dữ liệu về client, `net_write_timeout` là giá trị để kiểm soát khi nào ngắt kết nối trả về. Tương tự như `net_read_timeout`, trước phiên bản MySQL 5.1.41 timeout này chỉ áp dụng cho timeout của kết nối TCP/IP, không áp dụng cho Unix socket, named pipes hay
shared memory.

#### slave\_net\_timeout:

Thời gian timeout mà server slave chờ đợi dữ liệu từ master trước khi nết nối bị đóng.

#### wait_timeout:

Thời gian timeout mà server chờ đợi các hoạt động của kết nối không tương tác (noninteractive) trước khi đóng chúng. Timeout này chỉ áp dụng cho kết nói TCP/IP và Unix socket, không áp dụng cho kết nối tạo ra sử dụng named pipes hoặc shared memory.

Cấu hình tham số này quá thấp có thể sẽ gây nên việc ngắt kết nối quá sớm của server đến client. Cấu hình tham số này quá cao có thể dẫn đến tồn tại nhiều kết nối từ phía client đến MySQL server cùng lúc, tốn tài nguyên server. Tóm lại tuỳ theo đặc điểm của máy chủ và ứng 
dụng mà tinh chỉ timeout cho phù hợp

