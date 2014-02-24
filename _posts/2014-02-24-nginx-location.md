---
layout: post
title:  "Nginx location"
author: xluffy
date:   2014-02-24
tags: linux, nginx
description: "Nginx location"
...

### Định nghĩa

location ~ nghĩa là nginx sẽ phân biệt các ký tự hoa thường:
Điều này có nghĩa là nếu:

    location ~ /home/MyDir/MyFile.pdf

Một request tới /home/mydir/myfile.pdf sẽ khồng phù hợp, bởi vì phân biệt hoa thường.

location ~* có nghĩa là nginx sẽ không phân biệt ký tự hoa thường.
Điều này có nghĩa là nếu:

    location ~* /home/MyDir/MyFile.pdf

Một request tới /home/mydir/myfile.pdf sẽ phù hợp.

location = tìm kiếm một sự phù hợp chính xác
ví dụ:

    location = /home/MyDir/MyFile.pdf

Sẽ chỉ phù hợp với chuỗi URL được liệt kê, tương tự vậy, nếy URL vừa vặn, việc search sẽ dừng lại.

### Ví dụ 1:

When I try "http://www.example.com/1234", I want to rewrite "http://www.example.com/v.php?id=1234" 
and want to get "http://www.example.com/1234" in browser.

```
	location ~ /[0-9]+ {
		rewrite "/([0-9]+)" /v.php?id=$1 break;
	}
```

### Ví dụ 2:

http://example.com/isse1 --> http://example.com/shop/issues/custom_isse_name1

```
	location /issue {
		rewrite ^/issue(.*) http://$server_name/shop/issues/custom_issue_name$1 permanent;
	}
```	






