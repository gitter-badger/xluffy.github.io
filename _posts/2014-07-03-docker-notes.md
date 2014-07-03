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
  
```

