---
layout: post
title:  "How to create blog with git pages and jekyll"
author: xluffy
date:   2014-02-07
tags: tips
description: "Huong dan tao blog cho n~ ke ngheo"
---

### What? 
Bài này hướng dẫn cách tạo blog cho những kẻ nghèo và không có tiền.

###Who?
Ai cũng có thể làm theo được, nhưng tốt nhất là nên biết một chút xíu về lập trình, cơ bản như HTML và có khả năng đọc hiểu là được

###Why?

#### Tại sao lại là git pages?

Ờ thì đơn giản là không có tiền thuê VPS hay hosting, nhưng lại muốn viết cái gì đó vừa để học, để chơi, để nghịch shit.

#### Thế tại sao không tạo blogspot, wordpress hay bất cứ cái gì mà cứ phải là cái này?

Trả lời ngắn: Vì thích thế :v

Trả lời dài: Tác giải đã thử blogspot và không tài nào nhúng một đoạn `code` vào một bài viết được, và một số domain hay cũng hết rồi. Wordpres
thì quá chán, nặng nề, lằng nhằng và nhiều đam mỹ :3.

### Thế tạo blog với git pages, jekyll, markdown có gì hay?

- Có một domain nhìn hay hay dạng _x.github.io_
- Tiện lợi trong việc lưu trữ mã trên github, vừa tiện học cách sử dụng git
- Cấu trúc của jekyll khá đơn giản
- Sử dụng markdown thì không phải lọ mọ như HTML, cũng chả phải để ý xem có quên đóng đám tags hay không
- Cảm giác tự build mọi thứ, rảnh thì có thể thiết kế lại một cái themes khác đẹp hơn.

### Thế có gì không hay?

- Việc viết bài sẽ hơi khó khăn một xíu, dạng viết bằng _markdown_ và dùng jekyll để build ra html
- Tính năng sẽ không đầy đủ như một số blog khác, kiểu như muốn có comment thì thêm _disqus_, icon fb, g+ thì tự làm cả.

### Còn gì khác nữa không?

- Jekyll chỉ là một trong số nhiều static site generator và được viết bằng Ruby. Ngoài Jekyll ra còn có nanoc cũng viết bằng Ruby hay Pelican được viết bằng Python
- Thử search với từ khóa __static site generator__ bạn sẽ có rất nhiều kết quả và nhiều sự lựa chọn hoặc xem ở [đây] (https://wiki.python.org/moin/StaticSiteGenerator)

### Step 1: Tạo một repo trên GitHub

Nếu bạn đã có tài khoản GitHub thì bạn có thể đăng nhập và tạo một repository mới với tên dạng __*xluffy.github.io*__, trong đó
_xluffy_ là tên của bạn (hoặc tên tổ chức) trên GitHub

Nếu phần đầu tiên của repos không chính xác với tên đăng nhập GitHub của bạn, nó sẽ không làm việc, vì vậy hay đảm bảo nó chính xác.

### Step 2: Tạo Pages với __automatic generator__

1. Vào phần __Setting__ của repository mà bạn vừa tạo ra.
2. Click vào nút __Automatic Page Generator__
3. Điền vài thông tin vào trong __Markdown editor__.
4. Click vào nút __Continue To Layouts__.
5. Xem trước phần themes của bạn.
6. Khi đã tìm thấy theme bạn thích, click __Publish__.

### Step 3: Clone repository về máy bạn.

Lưu ý là sau khi thực hiện xong step 2, bạn sẽ thấy một vài file được tạo ra, đó là theme mà GitHub Page tạo ra. Bạn 
có thể sử dụng nó nếu thích, tuy nhiên ở đây ta sẽ sử dụng Jekyll để build và tạo ra layout cho blog của mình.

Tới thư mục mà bạn muốn đặt mã nguồn blog, và clone nó về.

```
    ~$ git clone https://github.com/xluffy/xluffy.github.io
	~$ cd xluffy.github.io
	~$ rm -rf * 
	// xoa het cac template cu cua git pages, 
	// chi giu lai .git/
```

### Step 4: Cài đặt Jekyll

```
	~ $ sudo gem install jekyll
```

### Step 5: Tạo layout và push lại lên kho chứa

```
	~ $ jekyll new ~/xluffy.github.io 
	~ $ git add --all
	~ $ git commit -m"Init Jekyll blog"
	~ $ git push origin master
	~ $ jekyll serve -w
```

Kiểm tra blog tại địa chỉ http://localhost:4000 hoặc http://127.0.0.1:4000


### Step 6: Chỉnh sửa config và viết bài đầu tiên
	
```
	~ $ cd xluffy.github.io
	~ $ vim _config.yaml -> fix anything
	~ $ vim _layouts/default.html -> fix your infomation
	~ $ echo "
			#!/usr/bin/env ruby
			puts "Hello, World!!"
		" _posts/2014-02-06-hello-world.m
```

### Step 7: Biên dịch mã markdown thành html

```
	~ $ cd xluffy.github.io
	~ $ jekyll build
```

Đăng nhập lại vào địa chỉ http://127.0.0.1:4000 để kiểm tra




