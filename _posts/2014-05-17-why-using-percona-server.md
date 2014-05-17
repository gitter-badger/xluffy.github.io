---
layout: post
title:  "Sử dụng percona server thay thế cho mysql có gì hay?"
date:   2014-05-17
tags: percona, mysql, linux, backuơ
description: "Sử dụng percona server thay thế cho mysql có gì hay?"
---

__BÀI VIẾT MANG MỘT SỐ TƯ TƯỞNG VÀ KINH NGHIỆM CÁ NHÂN__

### Giới thiệu

Hiện nay rất nhiều ứng dụng web chạy trên nền tảng Linux, PHP và MySQL. MySQL là một cơ sở 
dữ liệu quan hệ, một phần mềm mã muồn mở đã được Oracle mua lại và phát hành 2 bản đồng thời 
là bản OpenSource và bản Enterprise trả phí.

Ngoài bản MySQL của Oracle còn nhiều dị bản MySQL khác được một số công ty tùy chỉnh lại và phát
hành như Percona Server hoặc bản MySQL của Facebook được phát hành trên Github.

Thông thường các phần mềm trên Linux, ví dụ như Apache được biên dịch nhằm mục đích tùy chọn theo mong muốn
cá nhân, lược bỏ những phần quá nặng nề (vì Apache là một gói rất lớn) nhằm tăng hiệu suất của gói đó. Bạn 
cũng có thể tải mã nguồn của MySQL và biên dịch nó nếu thích. Tuy nhiên theo ý kiến cá nhân của tôi thì
việc biên dịch MySQL ko làm tăng hiệu suất so với việc cài từ một repo nào đó. Nên chỉ cần cài từ repo bất 
kỳ là được, quá trình tăng hiệu suất là vấn đề khác.

### Tại sao lại sử dụng Percona mà không sử dụng MySQL mặc định?

- Nếu bạn có tiền, Percona có thể support cho bạn trong trường hợp dữ liệu của bạn bị hỏng
- Nếu dữ liệu của bạn lớn, server bạn có cấu hình thấp, việc dump và restore DB sẽ là một cực hình.
 
Ví dụ với DB 36GB

- Dump ra sẽ được file 16GB và import lại sẽ mất 10 tiếng với một server cấu hình trung bình (4GB-RAM).

- DumpDB sẽ làm lock row hoặc lock table, làm web-site bạn bị downtime

Percona cung cấp một bộ công cụ cực kỳ mạnh mẽ là percona-toolkit và xtrabackup hỗ trợ cho việc quản trị và backup,restore
dữ liệu.

Kết quả là với cùng dữ liệu trên:

- Thời gian restore sẽ là 1-2h, ko lock row, ko lock table.

- Nhược điểm là dữ liệu backup lớn, chỉ sử dụng cùng một version và không thể restore với database-name khác.