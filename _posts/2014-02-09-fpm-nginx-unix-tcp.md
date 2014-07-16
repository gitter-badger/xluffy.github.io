---
layout: post
title: PHP-FPM, Nginx sử dụng Unix socket hay TCP
author: xluffy
date:   2014-02-09
tags: nginx, system, socket, tcp, php, php-fpm
description: 
---

### Vấn đề:

Trước đây có một thời gian mình dùng ab để benchmark ứng dụng Web chạy Apache +FastCGI thì 
xảy ra tình huốngkhi tăng số lượng request (-c) lên cao thì thấy bắn ra rất nhiều các error 
dạng như sau:

```connect() to unix:/var/run/php5-fpm.sock failed or **apr_socket_recv: Connection reset by peer (104)**```

Phải nói là lúc đó mình đã mất 2 ngày để search về vấn đề này nhưng vẫn không có tí kết quả
nào là tại sao lại có lỗi như vậy.

Gần đây nhân làm việc nhiều hơn với Nginx, PHP-FPM và như bài trước nói về cách biên dịch và sử 
dụng PHP với Web service mình mới hiểu ra vấn đề và viết sơ lại theo __suy đoán__ của mình

Như trong bài trước mình có nói việc sử dụng FastCGI để quản lý các tiến trình PHP. Tức là giữa
web server và FastCGI server sẽ phải có một phương thức để kết nối. Cụ thể ở đây hoặc là Unix socket
hoặc là TCP. Lỗi trên theo ngu ý của mình thì xảy ra khi FastCGI client (Apache) gửi __quá nhiều__
connection tới FastCGI Server, vượt quá một con số mặc định cho phép nào đó của Kernel Linux.

Unix socket thì mình chưa rõ là tham số nào quy định số lượng connection này, TCP thì có vẻ là các 
tham số trong tập tin `sysctl.conf` quản lý.

Mình đang tìm hiểu xem nên sử dụng Unix socket hay TCP trong mô hình Nginx, PHP-FPM, lợi điểm của từng
phương pháp.

Hiện tại nếu sử dụng TCP thì theo ý mình cần tweak các tham số trong `synctl.conf` thì lỗi trên khả 
năng sẽ giảm hoặc không xảy ra nữa.
