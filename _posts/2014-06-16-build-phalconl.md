---
layout: post
title:  "Biên dịch Phalcon với PHP"
date:   2014-06-16
tags: phalcon, PHP
description: "Biên dịch Phalcon với PHP"
---

### Tình huống

PHP được biên dịch với các module tùy chọn và prefix khác với prefix mặc định, cụ thể

```bash
	~$ ./configure --prefix=/usr/local/php
```

Điều này dẫn tới thư mục chứa các module của php cũng sẽ thay đổi từ `/usr/lib64/php/module/` thành /usr/local/php/lib/php/extensions/no-debug-non-zts-20121212/

Vấn đề là giờ nếu ta biên dịch Phalcon, mặc định `phalcon.so` sẽ không chứa trong thư mục module mới, và phalcon có rất ít option, document hướng dẫn về vấn đề này

### Giải quyết

Để giải quyết chúng ta sẽ `hack` một chút vào các php biên dịch một module.

Theo tài liệu cách biên dịch như sau:

```bash
	~$ git clone --depth=1 git://github.com/phalcon/cphalcon.git
	~$ cd cphalcon/build
	~$ sudo ./install
```

Cách biên dịch rất khác các biên dịch các phần mềm khác trên Linux, mở thử file install ra xem có cái gì?

```bash
	~$ phpize && ./configure --enable-phalcon && make && make install && echo -e "\nThanks for compiling Phalcon!\nBuild succeed: Please restart your web server to complete the installation"
```

Chà, PHP có 2 cách chính để cài đặt một module, 1 đó là cài qua `pecl`, 2 là phpize, chúng ta sẽ không cài bằng script của phalcon mà tự cài tay vậy

B1: Comment dòng phía trên trong file install và chạy `./install`

B2: Vào thư mục cphalcon/ext

B3: 

```bash
	~$ /usr/local/php/bin/phpize
	~$ ./configure --with-php-config=/usr/local/php/bin/php-config (sử dụng config của php mới build)
	~$ make && make install
```

Trả ra output

```bash
	Installing shared extensions: /usr/local/php/lib/php/extensions/no-debug-non-zts-20121212/
```

Vậy là cài thành công