---
layout: post
title: So sánh các cách sử dụng PHP cùng với web service
author: xluffy
date:   2014-02-08
tags: nginx, system, apache, php, fastcgi
description: So sánh các cách sử dụng PHP cùng với web service
---

### Sơ lược:

Về cơ bản với Web server là Apache ta có thể có 3 cách biên dịch và thực thi các mã .php như sau:

- Biên dịch PHP như một static module của Apache.
- Biên dịch PHP như một dynamic module của Apache.
- Biên dịch PHP như một CGI interprete (cụ thể ở đây là FastCGI)

### Biên dịch PHP như một static module của Apache.

Với cách này thì PHP module sẽ được biên dịch chung với Apache và không thể remove cũng như cài đặt lại PHP
nếu không biên dịch lại Apache.

Lợi điềm của cách này là tốc độ nhanh (vì PHP sẽ được load ngay khi start Apache) tuy nhiên nhược điểm là nếu cần cập 
nhật PHP thì buộc phải complie lại cả Apache.

Option để biên dịch dạng static module với source PHP như sau

`--with-apache[=DIR]
    Build a static Apache module. DIR is the top-level Apache build directory, defaults to /usr/local/apache.`

### Biên dịch PHP như một dynamic module của Apache.

Về cơ bản thì static và dynamic là như nhau, chỉ khác nhau ở cách hoạt động. Nghĩa là khi bạn sẽ cần load thêm module
php trong config của Apache. Và khi có một request nào gửi tới Web server thực thi một script PHP thì module mới __thực
sự__ được load lên (không giống như dạng static là load sẵn).

Option để biên dịch dạng dynamic module:

`--with-apxs2[=FILE]
    Build shared Apache 2.0 module. FILE is the optional pathname to the Apache apxs tool; defaults to apxs`

Lợi điểm của cách biên dịch này là ta có thể dễ dàng biên dịch, cập nhật lại PHP mà không ảnh hưởng tới Apache

__Nhược điểm của cả 2 cách trên__ là PHP module hoạt động như một phần của Apache, nghĩa là các process sinh ra khi thực thi
một script PHP nào đó sẽ có UserID là user chạy Apache luôn, điều này làm ảnh hưởng đến tính bảo mật của hệ thống.

__Ưu điểm của cả 2 cách trên__ là PHP module được sinh ra sẽ như một __theard__ của một process cha Apache, nên sẽ lightweight
và tiết kiệm RAM hơn.

__Tổng kết:__ Dù biên dịch static hay dynamic thì cơ chế xử lý của PHP trong mod_php là như nhau. Nghĩa là khi có request tới thì

sẽ vẫn đá qua module PHP để xử lý (tương tự như cách xử lý ở các module khác). Điều khác biệt chỉ ở lần thực thi script PHP đầu tiên (static thì 

load sẵn, dynamic thì khi có request đầu tiên mới load) tuy nhiên ở những lần thực thi sau thì cả 2 đều như nhau, không cần phải load
lại module PHP nữa.

### Biên dịch PHP như một CGI interprete (cụ thể ở đây là FastCGI)

Lưu ý là FastCGI là một chuẩn giao thức liên lạc (communication protocol). 

FastCGI là một dạng _server_ tách rời với Apache (thực chất là trên cùng một server). Khi có một request đi vào, Apache sẽ connect 
và chuyển request đó cho FastCGI server (Apache lúc này là FastCGI client). FastCGI và Apache giao tiếp với nhau qua kết nối 
TCP hoặc Unix socket.

Điểm lợi của phương pháp này là Apache (FastCGI client) và Application Server (FastCGI server) sẽ chạy thông qua những user độc lập.
Nghĩa là khi có request thực thi một script PHP, Apache sẽ đẩy qua cho FastCGI server xử lý, FastCGI sẽ sinh ra một process xử lý
và trả về kết quả cho Apache, xong xuôi thì process đó cũng được stop. 

Tóm lại là với cách này, ta sẽ có những process của Apache và những external process. Chính điều này làm cho FastCGI sẽ không thể 
lightweight như cách biên dịch PHP module được.

Đối với Web server là Apache ta có thể sử dụng mod\_fastcgi hoặc mod\_fcgi

Kết nối ở dạng Unix Socket

`FastCgiExternalServer /var/www/php5.external -socket /var/run/php5-fpm.sock`

Kết nối ở dạng TCP

`FastCgiExternalServer /var/www/php5.external -socket /var/run/php5-fpm.sock`

