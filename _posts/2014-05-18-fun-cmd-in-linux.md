---
layout: post
title:  "Một vài lệnh thú vị trên Linux"
date:   2014-05-18
tags: cmd, command line
description: "Một vài lệnh thú vị trên Linux"
---

Việc sử dụng Linux và thao tác công việc với các cmd của Linux là khá thường xuyên, tuy nhiên
không phải ai cũng biết sức mạnh và sự thú vị của các câu lệnh trên Linux.

Dưới đầy là vài câu lệnh tôi hay sử dụng và vài câu khá thú vị muốn chia sẻ.

### Liệt kê file mới chỉnh sửa gần đây nhất.

Trên Linux, lệnh `ls` được sử dụng để liệt kê file, ta hay dùng các option như `-a` để hiển thị file
ẩn, `-l` để liệt kê theo cột, `-h` để liệt kê với kích thước có thể đọc dễ dàng. Tuy nhiên tôi hay áp d
ụng lệnh này.


```
    ~$ ls -larth
```

Thêm hai option `-t` để liệt kê file theo thời gian modify, file mới sửa gần nhất sẽ nằm trên cùng, tuy nhiên nếu có quá nhiều file thì lại mất công cuộn chuột lên, `-r` sẽ đảo ngược lại, nghĩa là file mới sửa
gần nhất sẽ nằm dưới cùng.

### Không lưu history

Mọi thao tác lệnh trên Linux sẽ được ghi lại, tùy thuộc vào kích thước history ta thiết lập, tuy nhiên ta có
thể chạy một lệnh mà không lưu lại history bằng cách thêm một dấu `<space>` trước lệnh đó

```
    ~$ <space>ll
    ~$ history
```

### Chạy lệnh cuối cùng như root

```
    ~$ sudo !!
```
