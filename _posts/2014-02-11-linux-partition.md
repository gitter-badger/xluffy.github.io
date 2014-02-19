---
layout: post
title:  "Phân vùng ổ đĩa khi cài Linux server"
author: xluffy
date:   2014-02-11
tags: linux, partition
description: "Phân vùng ổ đĩa khi cài Linux server"
---

### Vấn đề:

Thời sinh viên, cá nhân tôi cũng cài và chia từng phân vùng cẩn thận. Tuy nhiên thì lúc đó
cũng chưa rõ lắm việc này có hiệu suất như thế nào và chia bao nhiêu là đủ.

Đây là một số kinh nghiệm góp nhặt được trong quá trình làm việc mà tôi ghi lại.

### Giải quyết

Thực ra thì tùy từng server phục vụ cho các mục đích khác nhau thì sẽ chia khác nhau. Tựu chung Linux
cần tối thiểu 2 phân vùng đó là `/root` và swap

Tuy nhiên đối với server ta cần chia rõ ràng hơn như sau

- /root --> chứa tất cả các phần cài đặt. Phần này cần không nhiều, tầm 15GB là đủ.
- swap --> thường x2 với RAM, nhưng với các server có RAM lớn khỏang >16GB thì bằng RAM là đủ.
- /boot --> nên chia để có vấn đề gì bootloader có thể dễ dàng xử lý mà không ảnh hưởng tới các phân vùng khác. Tầm 200MB là đủ.
- /tmp --> nên chia, vì /tmp có chmod là 777, thường thì tầm 1GB-2GB là đủ. Tuy nhiên với DB Server khi query dữ liệu sẽ cần một
vùng tạm nào đó để lưu (thường là /tmp) nên với DB server cần chia lớn hơn, có thể từ 4-8GB.
- /var --> nên chia, log sẽ đầy nên rất nhanh, tầm 16GB
- /home -> Nếu là FTP server, hoặc có ý định đặt Document Root của Web server ở đây thì nên chia to. Thường là phần còn lại của ổ cứng.

Đại ý là như trên, tùy nhu cầu có thể chia khác đi một chút, ví dụ như `/root` có thể lớn hơn để tạo các thư mục `/backup`
ở root chẳng hạn.

Ngoài ra với `/boot` và `swap` thì nên chia ở dạng Standard Partition, còn các phân vùng còn lại nên sử dụng LVM để 
dễ dàng resize sau này.
