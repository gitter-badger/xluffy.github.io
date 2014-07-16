---
layout: post
title:  "Tạo môi trường làm việc ảo với virtualenv"
author: xluffy
date:   2014-02-10
tags: virtualenv, python
description: "Tạo môi trường làm việc ảo với virtualenv"
---

### Tạo môi trường làm việc ảo với virtualenv

Virtualenv là một công cụ để xây dựng môi trường độc lập khi phát triển với Python. 
Đó là một cách tuyệt vời để nhanh chóng kiểm tra các thư viện mới mà không làm lộn xộn 
các package có sẵn của bạn hoặc chạy nhiều dự án trên cùng một máy phụ thuộc vào một 
thư viện đặc biệt nhưng không phải cùng một phiên bản của thư viện đã có.

Ví dụ bạn có thể cài đặt phiên bản X của thư viện trong một môi trường và phiên bản Z 
của cùng một thư viện trong môi trường khác, mà không gây ảnh hưởng đến cái khác. Mỗi 
môi trường cung cấp thực thi Python riêng của nó và thư mục của site-package 
(không được chia sẻ giữa các môi trường).

Để cài đặt virtualenv, chỉ cần sử dụng easy_install với lệnh sau đây:

    `easy_install virtualenv`

Một khi virtualenv được cài đặt, bạn có thể sử dụng lệnh virtualenv để tạo ra môi trường ảo. 
Các lệnh sau đây sẽ tạo ra một môi trường được gọi là "working":

    `virtualenv working`

Bạn có thể kích hoạt các môi trường bằng cách chạy script kích hoạt nó, nằm trong 
thư mục bin của môi trường:

    `source working/bin/activate`

Nếu bạn cài đặt một package trong môi trường ảo của bạn, bạn sẽ thấy script thực thi được đặt 
trong bin/working/ và egg trong working/lib/python2 .X/site-package/

Như bạn thấy, môi trường ảo sẽ kế thừa các gói từ thư mục site-packages mặc định của hệ thống. 
Điều này đặc biệt hữu ích khi dựa vào những package có sẵn, vì vậy bạn không cần phải cài đặt 
lại chúng trong mọi môi trường.

Tôi cố gắng giữ cho thư mục site-package của hệ thống tối giản và chỉ cà đặt các package mà 
tôi sử dụng trong gần như mọi dự án. Nó bao gồm giao diện làm việc với cơ sở dữ liệu như MySQLdb 
hoặc psycopg2 cũng như setuptools, tất nhiên là cả virtualenv.

Tuy nhiên, bạn cũng có thể thay đổi điều này bằng cách truyền --no-site-packages khi tạo ra một 
môi trường ảo. Điều này sẽ tạo ra một môi trường sạch sẽ mà không kế thừa các gói từ 
site-package của hệ thộng:

    `virtualenv --no-site-packages working`

Để thoái khỏi một môi trường, chỉ cần chạy câu lệnh:

    `deactivate`

Như vậy với virtualenv chúng ta có thể tạo ra những môi trường làm việc độc lập, thoải mái có thể 
thử nghiệm các package và chạy các ứng dụng lặt vặt khác. Điều này rất hữu ích cho việc học Django 
nói riêng và học Python nói chung.
