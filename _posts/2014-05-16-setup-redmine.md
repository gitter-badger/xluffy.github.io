# 1. Chép source và import db


Chạy bundle install


thiếu gói


cài


	
	yum install postgresql-devel
	
	yum install ImageMagick-devel



# 4. Troubleshooting


## a. Passenger config.ru permission denied


Lí do là do thư mục root của redmine (hiện tại để tại /home/redmine) không có quyền execute và thư mục document root chưa có quyền 777, file config.ru chưa có quyền write cho own user


	
	~$ chmod g+x,o+x /home/redmine
	
	~$ chmod 777 /home/redmine/redmine (ko có option -R)
	
	~$ chmod 777 /home/redmine/redmine/config.ru



## b. server_names_hash_bucket_size


Restarting nginx: nginx: [emerg] could not build the server_names_hash, you should increase server_names_hash_bucket_size: 32


Lý do mặc định nginx chỉ để 32 bytes cho việc đặt server_name, tăng trong cấu hình lên.


	
	server_names_hash_bucket_size 64;



## c. 500 error trang my/page


Khi chép mới hoặc import db mới về.


	
	~$ vim config/environments/production.rb
	
	`<<config.logger.level = :debug>`>
	
	-> bật debug log



	
	bundle exec rake db:migrate


