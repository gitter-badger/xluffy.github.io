Vài ghi chép các nhân để nhớ những thao tác và khái nhiệm khi sử dụng Docker

- Docker có 2 khái niệm là Container và Images

## thao tác với container

Cài đặt, start trên ArchLinux và kiểm tra

```
  ~$ pacman -S docker
  ~$ sudo systemctl start docker
  ~$ sudo docker info
  ~$ sudo docker version
```

Search, pull

``` 
  ~$ sudo docker search ubuntu
  ~$ sudo docker pull ubuntu
  ~$ sudo docker images 
```

Chạy cmd

```
  ~$ sudo docker run ubuntu/12.04 echo "hello world"
  ~$ sudo docker run ubuntu/12.04 apt-get install -y ping
```

Sau đó có thay đổi gì ví dụ như cài cắm thêm gì thì có thể commit và push

Docker có một chỗ gọi là `hub.docker.com` cho phép đăng ký, và tạo free 1 repo, có thể tạo riêng một images  cài, commit, push lên chính repo của mình. 

## thao tác với images

Search qua thì thấy có khá nhiều cách tạo, dùng febootstraps để tạo imagee cho CentOS/RHEL, debbootstrap để tạo cho  Debina/Ubuntu, một cách khác nữa là dùng Dockerfile

Tài liệu ở đây: https://docs.docker.com/userguide/dockerimages/#creating-our-own-images

B1: Tạo một Dockerfile đơn giản cho việc tạo iamge cho Ubuntu 

```
  #Buidl an Image from Dockerfile
  FROM ubuntu:12.04
  MAINTAINER Quang Nguyen <xquang.foss@gmail.com>
```

Build Image

```
  ~$ sudo docker build -t="sg/rBu:v1" . ## Lưu ý là có dấu `.` ở cuối, chỉ thư mục chứa Dockerfile
```

Vậy là xong bước tạo một image, giờ ta có thể tạo một container từ image mới tạo này

## Vòng đời

+ `docker run` tạo một container.
+ `docker stop` stop một container.
+ `docker start` và start nó lại.
+ `docker restart` restarts một container.
+ `docker rm` xóa một container.
+ `docker kill` gửi một SIGKILL tới một container. Has issues.
+ `docker attach` sẽ connect với một container đang chạy.
+ `docker wait` blocks until container stops.
	
Nếu bạn muốn chạy và tương tác với một container, `docker start` và `docker attach`
Nếu muốn có một container tạm thời, chạy `docker run -rm`, nó sẽ xóa container đó khi stop.
Nếu muốn chia sẻ một thư mục từ host tới docker container, chạy `docker run -v $HOSTDIR:DOCKERDIR`

Nếu muốn có một container tạm thời, chạy `docker run -rm`, nó sẽ xóa container đó khi stop.
