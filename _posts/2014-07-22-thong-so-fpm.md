---
layout: post
title:  "Các thông số cấu hình của php-fpm"
author: xluffy
date:   2014-07-22
tags: php-fpm, linux, config
description: "Các thông số cấu hình của php-fpm"
---

+ pm = dynamic: Changes how PHP-FPM wil spin up PHP workers. 

+ pm.max_children = 10: Thiết lập số `PHP worker` tối đa.

+ pm.start_servers = 4: Thiết lập số `PHP worker` được activat khi PHP-FPM starts up

+ pm.min\_spare\_servers = 2: Thiết lập số `idle worker` nhỏ nhất mà PHP-FPM will try to keep standing by. => nghĩa là trong tình trạng lượng 
truy cập thấp, việc tồn tại các `idle worker` sẽ gây tốn RAM một cách không cần thiết (RAM còn sử dụng cho service khác), thiết lập con số 
này sẽ giới hạn số `worker` tránh lãng phí RAM.

+ pm.max\_spare\_servers = 6: Sets the maximum number of idle workers PHP-FPM will allow to remain alive before it starts terminating processes.
