---
layout: post
title:  "Error reading top line, Logrotate"
date:   2014-02-18
tags: log, nginx, apache, linux
description: "Error reading top line, Logrotate"
---

### Giới thiệu:

Nếu chúng ta không quản lý những server có mức truy cập cao thì có lẽ sẽ không mấy quan tâm đến
dịch vụ này. Dịch vụ này cho phép chúng ta làm các chức năng sau:

- Gom log theo từng ngày, tuần, tháng.
- Nén chúng lại với phần suffixes tùy chọn, ví dụ ngày-tháng-năm của log đó, dạng `luser-access_log.20140218.gz`
- Xóa log sau một khoảng thời gian chỉ định

Ví dụ bạn có một website với lượng truy cập lớn, log sẽ sinh ra hàng ngày và cần lưu trữ lại, nếu để im thì log 
của mọi ngày sẽ đổ vào 1 file và sau 1 tháng file đó có thể nặng tới hàng GB. Dẫn đến vô ích trong việc mở và debug
nếu có lỗi. Hoặc để debug theo ngày lại phải dùng `grep` để lọc log thì sẽ rất tốn công sức, hoặc log chỉ cần lưu trữ
trong vòng 1 tháng là đủ chẳng hạn.


If you get an error saying

```
    /etc/cron.daily/logrotate:
    error: error reading top line of /var/lib/logrotate.status
```

delete `/var/lib/logrotate.status`, and then run logrotate once with -f flag, like

```
    ~$ logrotate -f /etc/logrorate.d/syslog
```

This should initialize the status file and the error should not be repeating.
