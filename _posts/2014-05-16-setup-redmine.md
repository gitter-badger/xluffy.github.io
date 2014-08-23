---
layout: post
title:  "Cài đặt redmine làm bug tracker và project management"
author: xluffy
date:   2014-05-09
tags: redmine, bug, linux
description: "Cài đặt redmine làm bug tracker và project management"
---

# 1. Chép source và import db

Dowload mã nguồn tại [đây](http://svn.redmine.org/redmine/branches/2.3-stable) thông qua svn

```bash
	~$ cd /home/redmine
	~$ svn co http://svn.redmine.org/redmine/branches/2.3-stable redmine
```
Config database connection:

```
	~$ cp config/database.yml.example config/database.yml
	~$ vim config/database.yml
```

Install pre-requires


```bash
	~$ yum install ruby ruby-devel rubygems ImageMagick ImageMagick-devel postgresql postgresql-devel -y
	~$ gem install bundler
	~$ bundle install --without development test
```
Install/Upgrade database

```ruby
	rake db:migrate RAILS_ENV=production 
	rake redmine:plugins:migrate RAILS_ENV=production
```

Config to run with Apache+mod_fastcgi


```bash
	~$ yum install mod_cgi mod_fastcgi fcgi fcgi-devel
	~$ gem install fcgi
	~$ cd public/
	~$ mv htaccess.fcgi.example .htaccess
	~$ mv dispatch.fcgi.example dispatch.fcgi
	~$ chmod 755 dispatch.fcgi
	~$chown apache:apache -R *
```

Tạo file VirtualHost cho Redmine với nội dung sau:

```
	Listen *:80

	<VirtualHost *:80>
			DocumentRoot /home/redmine/public
			ErrorLog logs/redmine_error_log

			#MaxRequestLen 20000000

			<Directory "/home/redmine/public">
					Options Indexes ExecCGI FollowSymLinks
					Order allow,deny
					Allow from all
					AllowOverride all
			</Directory>
	</VirtualHost>
```

Restart lại apache để cập nhật

```bash
	~$ /etc/init.d/httpd restart
```

Plugin để đăng nhập bằng Google Account: [https://github.com/twinslash/redmine\_omniauth\_google](https://github.com/twinslash/redmine_omniauth_google)


### Sử dụng nginx và Passenger


Sử dụng Nginx + Passenger để chạy Redmine


Việc cài đặt Passenger yêu cầu rebuild lại nginx, cấu hình như sau:


```ruby
	passenger_root /usr/local/lib/ruby/gems/2.0.0/gems/passenger-4.0.42;
	passenger_ruby /usr/local/bin/ruby;
	passenger_pool_idle_time  0;
	passenger_max_pool_size   30;
	passenger_max_instances_per_app 2;
```

Cấu hình redmine

```
server {
  listen *:80;

  server_name projects.abc.xyz;
  add_header sid redmine;
  server_tokens off;

  root /home/redmine/public;
  passenger_enabled on;
  passenger_user nginx;
  passenger_load_shell_envvars off;
  access_log  /var/log/nginx/redmine_access.log;
  error_log   /var/log/nginx/redmine_error.log;
}
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

