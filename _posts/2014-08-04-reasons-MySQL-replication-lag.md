---
layout: post
title:  "Nguyên nhân MySQL Replication Lag"
author: xluffy
date:   2014-08-04
tags: mysql, master-slave, lag, delay
description: "Nguyên nhân MySQL Replication Lag"
---

### Bài toán

Mô hình MySQL Master-Slave là một mô hình nâng cao trong việc setup hệ CSDL, nhằm mục đích chia tải các kết nối tới DB ra nhiều server khác nhau. 
Giải thích ngắn gọn thì sẽ có một MySQL-Master, và một hoặc nhiều MySQL-Slave sync dữ liệu từ Master đó.

Dữ liệu trên Slave sẽ luôn đồng bộ với dữ liệu trên Master trừ trường hỏng relication (đồng nghĩa với con Slave đó ko còn sử dụng đc nữa cho đến
khi sửa xong). Việc sync dữ liệu từ Master tới Slave luôn có một độ trễ, lý tưởng nhất là đỗ trễ bằng 0, nhưng cũng có những lúc đỗ trễ đó tăng cao
dẫn đến việc hỏng relication và Slave không thể sử dụng ở thời điểm đó được nữa. Để check đỗ trễ của Slave so với Master ta có thể gõ lệnh sau:

```bash
	~ mysql>show slave status\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 
                  Master_User: 
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: 
          Read_Master_Log_Pos: 
               Relay_Log_File: 
                Relay_Log_Pos: 
        Relay_Master_Log_File: 
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table:
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 
              Relay_Log_Space: 
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 1
```

Giá trị `Seconds_Behind_Master: 0` chính là độ trễ của Slave đó với Master, trong qua trình triển khai thực tế, nhờ Nagios tôi phát hiện ra là hầu như ngày
nào tôi cũng nhận được email thông báo có độ trễ -> có nghĩa là hệ thống có vấn đề, và nhiệm vụ của system admin là tìm ra nguyên nhân.

### Các nguyên nhân phổ biến

#### Lỗi phần cứng (Hardware Faults)

Lỗi phần cứng có thể làm giảm hiệu suất gây ra ảnh hưởng tới độ lag của Master-Slave. Ví dụ lỗi không nhận ổ cứng RAID có thể là một lỗi phổ biến

#### Thay đổi cấu hình

Thay đổi cấu hình MySQL, cấu hình phần cứng, cấu hình hệ điều hành hoặc nói chung là bất cứ thay đổi gì đều có thể ảnh hưởng đến độ lag của Master-Slave.
Vấn đề phổ biến của relication bao gồm các cấu hình `sync_binlog=1`, `enabling log_slave_updates`, `setting innodb_flush_log_at_trx_commit=1`. Nói chung
cần lưu ý rằng mọi thay đổi đều là không an toàn, kể cả những thay đổi về cấu hình như tăng kích thước bộ nhớ đệm.
