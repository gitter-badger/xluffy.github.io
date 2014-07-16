---
layout: post
title:  "Cài đặt redmine làm bug tracker và project management"
author: xluffy
date:   2014-05-09
tags: redmine, bug, linux
description: "Cài đặt redmine làm bug tracker và project management"
---

# 1. Chép source và import db

Chạy `bundle install` thiếu gói cài

```bash
	~$ yum install postgresql-devel	
	~$ yum install ImageMagick-devel
```

# 4. Troubleshooting

## a. Passenger config.ru permission denied

Lí do là do thư mục root của redmine (hiện tại để tại /home/redmine) không có quyền execute và thư mục document root chưa có quyền 777, file config.ru chưa có quyền write cho own user

```bash
	~$ chmod g+x,o+x /home/redmine
	~$ chmod 777 /home/redmine/redmine (ko có option -R)
	~$ chmod 777 /home/redmine/redmine/config.ru
```

## b. server\_names\_hash\_bucket\_size

```bash
	Restarting nginx: nginx: [emerg] could not build the server_names_hash, you should increase server_names_hash_bucket_size: 32
```
Lý do mặc định nginx chỉ để 32 bytes cho việc đặt server_name, tăng trong cấu hình lên.

```bash
	server_names_hash_bucket_size 64;
```

## c. 500 error trang my/page

Khi chép mới hoặc import db mới về.
	
```bash	
	~$ vim config/environments/production.rb
	`<<config.logger.level = :debug>`>
	-> bật debug log
```

```bash
	bundle exec rake db:migrate
```

