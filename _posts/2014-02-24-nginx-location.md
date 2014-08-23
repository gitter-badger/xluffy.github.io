---
layout: post
title:  "Location trên nginx"
author: xluffy
date:   2014-02-24
tags: linux, nginx
description: "Location trên nginx"
---

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

### Ví dụ 3:

```
	location / { }
	location /images/ { }
	location /blog/ { }
	location /planet/ { }
	location /planet/blog/ { }
```

These first five examples are literal string matches, which match any part of an HTTP request that comes after the host segment. So, for example, if someone requests:

Request: http://example.com/

Returns: Assuming that there is a server_name entry for example.com, the "location /" directive will determine what happens with this request.

nginx always fulfills request using the most specific match. So, for example:

Request: http://example.com/planet/blog/ or http://example.com/planet/blog/about/

Returns: This is fulfilled by the "location /planet/blog/" setting because it is more specific, even though "location /planet/" also matches this request.

### Ví dụ 4:

```
	location ~ IndexPage\.php$ { }
	location ~ ^/BlogPlanet(/|/index\.php)$ { }
```

When a location directive is followed by a tilde (~), nginx performs a regular expression match. These matches are always case-sensitive. So, IndexPage.php would match the first example above, but indexpage.php would not. In the second example, the regular expression ^/BlogPlanet(/|index\.php)$ will match requests for /BlogPlanet/ and /BlogPlanet/index.php, but not /BlogPlanet, /blogplanet/, or /blogplanet/index.php. Nginx uses Perl Compatible Regular Expressions (PCRE).


### Ví dụ 5:

```
	location ~* \.(pl|cgi|perl|prl)$ { }
	location ~* \.(md|mdwn|txt|mkdn)$ { }
```

If you want matches to be case-insensitive, use a tilde with an asterisk (~*). The examples above all specify how nginx should process requests that end in a particular file extension. In the first example, any file ending in: .pl, .PL, .cgi, .CGI, .perl, .Perl, .prl, and .PrL (among others) will match the request.

### Ví dụ 6:

```
	location ^~ /images/IndexPage/ { }
	location ^~ /blog/BlogPlanet/ { }
```

Adding a caret and tilde (^~) to your location directives tells nginx, if it makes a match for a particular string, to stop searching for more specific matches and use the directives here instead. Other than that, these directives work like the literal string matches in the first group. So, even if there's a more specific match later, if a request matches one of these directives, the settings here will be used. See below for more information about the order and priority of location directive processing.







