---
layout: post
title:  "Quản lý mã nguồn và phương pháp deloy thông qua git"
author: xluffy
date:   2014-08-16
tags: git, deployment, fabric, automation
description: "Quản lý mã nguồn và phương pháp deloy thông qua git"
---

### Bài toán

Team phát triển một ứng dụng web, sử dụng git để quản lý mã nguồn. Mong muốn là deploy code lên server
một cách tự động và có thể deploy ra nhiều server trong trường hợp scale.

### Các cách cũ

- Dùng FTP để deploy: làm thủ công, giao thức không mã hóa, tốn công sức nếu làm nhiều server
- Dùng SCP: tương tự như FTP, có thêm ưu điểm là mã hóa
- Dùng rsync over ssh: Phải setup dịch vụ rsync-daemon trên các node, phải viết script, thao tác nhiều
- Dùng git pull: Phải ra lệnh trên nhiều node, web-root chưa nhiều file dạng `.git`, phải loại bỏ cấu hình, rủi ro.

### Cách giải quyết

- Đánh mã cho từng phiên bản được release
- Nhánh master luôn stable, ko merge trực tiếp
- Chỉ giải nén những file của web-root, những thứ như README.md, LICENSE.md, logs, var, composer không cần thiết phải giải nén ra
- Web-root có cài sẵn một số thư viện bằng composer, thư mục vendor, ko cần overwrite thư mục này

#### Làm thủ công bằng tay

Giả sử mã nguồn được đặt tại [http://github.com/xluffy/web-app.git](http://github.com/xluffy/web-app.git)

Tóm tắt là ở máy của người deloy, đóng gói mã nguồn thành một file nén, gửi lên server, giải nén vào một thư mục tạm,
đổi quyền cho giống với quyền của web-root và sync các file cần thiết từ thư mục tạm qua web-root. Cuối cùng xóa file rác
reload, clear-cache.

B1: Đánh mã cho từng phiên bản release bằng tag cho mỗi lần deploy (có thể dùng lệnh và push lên hoặc tạo từ giao diện github)

B2: Liệt kê tất cả tag trên remote server và đóng gói thành 1 file lưu trữ

- Liệt kê

```bash
	~$ git ls-remote --tags http://github.com/xluffy/web-app.git
	v0.1beta
```

- Đóng gói và nén (cho nhẹ và truyền 1 file), trực tiếp trên server hoặc tại local rồi chuyển lên server

```bash
	~$ git archive --format=tar --remote=git@http://github.com/xluffy/web-app.git v0.1beta> /output/v0.1beta.tar
```

B3: Giải nén

- Giải nén tập tin đóng gói vào thư mục tam, chỉ giải nén các thư cần thiết, cụ thể ở đây là

```bash
	~$ cd /output
	~$ tar --wildcards -xf  0.7.tar apps common index.html public
	apps 
	common 
	index.html 
	public
```

Lệnh trên có ý nghĩa là chỉ giải nén các file cần thiết và liên quan đến mã nguồn của web

B4: Đổi quyền và sync

- Thay đổi quyền của thư mục tạm vừa giải nén là quyền của user website

```bash
	~$ chown -R web-root: /output
```

- Rsync + delete các file

```bash
	~$ rsync -rap --delete --exclude={vendor/,*.tar,common/config,CHANGE.md,composer.*,logs,README.md} /output/ /var/www/web-root
```

Giải thích các option

- rap: r: cả các thư mục con, a: archive, p: giữ lại quyền
- delete: xóa file ở src → xóa luôn ở dest
- exclude: bỏ qua các file trong danh sách, nghĩa là ở src có tồn tại hay không thì cũng không sync qua dest, 
LÍ DO: file config, file không bao giờ thay đổi (markdown), file nén, file log, thư viện vendor)

B5: Xóa, clear cache sau khi deploy xong

- Xóa file tạm

```bash
	~$ rm -rf /output/*
```

- Reload Server, Clear Opcode-Cache

```
	~$ /etc/init.d/php-fpm reload
```

#### Tự động các thao tác trên bằng Fabric

Dùng fabric, code như sau:


```python
	#!/usr/bin/env python

	from fabric.api import *
	from fabric.contrib.console import confirm

	env.colors = True

	@task
	def web_prod():
		"""
		Web Production
		"""
		env.hosts = ['10.100.0.5', '10.100.0.6']
		env.user = 'root'
	 
	@task 
	def web_stag():
		"""
		Web Staging 
		"""
		env.hosts = ['10.200.0.5']
		env.user = 'root'

	@task
	def update(tag):
		run('git archive --format=tar --remote=git@http://github.com/xluffy/web-app.git %s > /output/%s.tar' % (tag, tag))

	@task
	@parallel
	def setup(tar):
		if(confirm('Deploy tag %s to Production?' % tar) is False):
			abort('Graooo!!! No deploy for u')
		with cd('/output'):
			run('tar --wildcards -xf %s apps common index.html public' % tar)
			run('chown -R web-root: /outputt')
			run('rsync -rap --delete --exclude={vendor/,*.tar,common/config,CHANGE.md,composer.*,logs,README.md} /output/ /var/www/web-root')
			run('rm -rf /output/*')
		print('tag %s deployed' % tar)

	@task
	@parallel
	def clear_cache():
		run("/etc/init.d/php-fpm reload")
```

Các bước chạy để deploy tag 0.7 trên stag

```bash
	~$ fab web_prod update:v0.1beta
	~$ fab web_prod setup:v0.1beta
	~$ fab web_prod clear_cache
</code>












