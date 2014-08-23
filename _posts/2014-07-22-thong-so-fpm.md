---
layout: post
title:  "Các thông số cấu hình của php-fpm"
author: xluffy
date:   2014-07-22
tags: php-fpm, linux, config
description: "Các thông số cấu hình của php-fpm"
---

#### pm = dynamic: 

Số lượng tiến trình con (child process) được thiết lập tự động dựa trên các thông số pm.max\_children, pm.start\_servers,
pm.min\_spare\_servers, pm.max\_spare\_servers.

Khác với pm = static: Số lượng tiến trình con sẽ được fix cứng, đó là giá trị pm.max_children

#### pm.max\_children = 10

Đối với pm = static, nó là số lượng các tiến trình con được tạo ra. Đối với pm =dynamic, nó là số lượng tiến trình
con tối đa được tạo ra. Tùy chọn này đặt ra một giới hạn về số lượng yêu cầu được phục vụ đồng thời tương đương với MaxClients ở mpm\_prefork.

Con số này thường được tính toán như sau: 

```
	pm.max_children = (Tổng số RAM - số RAM sử dụng bởi các dịch vụ khác) / số RAM trung bình của một process php-fpm.
```

#### pm.start_servers = 4

Số lượng tiến trình con được tạo ra khi khởi động, giá trị này chỉ sử dụng khi pm = dynamic. Giá trị mặc định

```
	start_servers = min_spare_servers + (max_spare_servers - min_spare_servers) / 2.
```

#### pm.min\_spare\_servers = 2

Số lượng tối thiểu process khi server ở trạng thái nhàn rỗi => nghĩa là trong tình trạng lượng truy cập thấp, 
việc tồn tại các `idle worker` sẽ gây tốn RAM một cách không cần thiết (RAM còn sử dụng cho service khác), thiết lập con số này sẽ giới hạn số `worker` tránh lãng phí RAM.

#### pm.max\_spare\_servers = 6

Số lượng tối đa mong muốn khi server ở trạng thái nhàn rỗi

#### pm.max_requests

Số lượng request mỗi tiến trình con nên thực thi (xử lý) trước khi hồi sinh.