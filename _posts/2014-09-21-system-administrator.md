---
layout: post
title:  "System Administrator"
date:   2014-09-21
tags: system, sa, dev, devops
description: "System Administrator"
---

Giới thiệu về công việc System Administrator aka Operators Engineer

System Admin, Sysadmin (bạn bè trong giới hay gọi đùa là sh!t @ssmin, chỉ người
dọn sh!t) hay Operators Engineer (trên thế giới hay gọi thế). Chỉ người làm 
nhiệm vụ bảo trì, cấu hình, vận hành một hệ thống gồm nhiều máy tính, đặc biệt
là các máy chủ hệ thống cũng như vận hành và bảo trì một hệ thống mạng kèm theo

Mục đích cuối cùng là đảm bảo cho hệ thống máy tính, hệ thống mạng LUÔN LUÔN chạy

## 1. Mô tả công việc

* Cài đặt, cấu hình, tối ưu hóa các máy chủ Linux
* Quản lý, giám sát đảm bảo máy chủ hoạt động ổn định suốt 24h
* Triển khai, vận hành hệ thống máy chủ tại Data Center
* Kiểm tra, bảo trì, backup dữ liệu hệ thống định kỳ.
* Ghi chép, lưu giữ nhật ký hoạt động của hệ thống.
* Nghiên cứu, đề xuất giải pháp và công nghệ mới

## 2. Những tố chất và kỹ năng cần có của một System Admin

Thực ra chẳng có một mô tả cứng hoặc cụ thể nào về những tố chất và những kỹ 
năng cần thiết của một system admin. Những thứ liệt kê ở dưới chỉ giúp cho công
việc của bạn trở nên dễ thở, vận hành hệ thống trơn tru và tự phát triển những
kỹ năng của chính bản thận bạn

* Kỹ năng giải quyết vấn đề một cách logic, khoa học
* Hứng thú với việc viết tài liệu và chia sẻ cho người khác
* Lười biếng, lười biếng và lười biếng
* Thuật toán, mã hóa, các thứ về bảo mật
* Sử dụng command-line, shell một cách thành thạo

## 3. Tìm tài liệu và giải quyết vấn đề như thế nào?

Liệt kê một số nơi mà bạn có thể tìm được câu trả lời cho những vấn đề của bạn

* Trước tiên, chính là nơi bạn viết, Wiki này hoặc chính blog này 
* Google, google và google
* Loạt website hỏi đáp như [Stackoverflow](http://stackoverflow.com) chuyên
môn: DBA (MySQL) [DBA](http://dba.stackexchange.com), [ServerFault](http://serverfault.com), 
[SuperUser](http://superuser.com)
* Các vấn đề chuyên sâu  trang chủ, trang docs, forum của các ứng dụng, giải
pháp, phần mềm đó
* [Github](http://github.com) -> có rất nhiều dự án thú vị hoặc
các list công nghệ rất thú vị ví dụ [awesome-sysadmin](https://github.com/kahun/awesome-sysadmin)
* Các trang dạng News như: [New Hacker](https://news.ycombinator.com/)
-> nhiều công nghệ mới, [tinsang](http://tinsang.net/), hoặc Twitter → twitter đặc biệt hữu ích 
khi tìm hiểu về trend của giới công nghệ hiện nay.

## 4. A lazy sysadmin is the best sysadmin

* Rule #1: Viết script cho các công việc lặp đi lặp lại, một cách lười biếng 
thông minh giúp anh ta trở lên master khi sử dụng bash, sed, awk, grep, ack …
* Rule #2: Backup everything, viết một script backup khi start một dự án, kiểm
tra nó sau một khoảng thời gian để đảm bảo nó chạy đúng
* Rule #3: Command line master
* Rule #4: Document everything, từ cách cài đặt, cách sửa lỗi, cách triển khai,
các vấn đề trong tương lai, những thứ mà có thể chia sẻ cho team.
* Rule #5: Cấu hình mọi thứ với độ sẵn sàng cao
* Rule #6: Chủ động trong mọi tình huống. Một sysadmin lười biếng không phải là
một người luôn ngồi chơi khi rảnh rỗi. Anh ta ghét phải thức dậy lúc nửa đêm chỉ 
vì một lỗi rất nhỏ. Cái anh ta cần làm là dùng thời gian rảnh rỗi để nghiên cứu, 
để đặt một vài cảnh báo, để dự đoán rủi ro, để dự đoán các vấn đề, lỗi, sự tăng 
trưởng của hệ thống trong tương lai để có thể phản ứng trước khi lỗi, vấn đề xảy
ra.
* Rule #7: Học từ sai lầm. Một sysadmin sẽ rất ghét phải lặp lại các sai lầm tương
tự nhau. Chẳng ai muốn gặp phải các vấn đề không mông muốn. Nhưng một sysadmin giỏi
là người khi gặp phải vấn đề bất ngờ, ngoài việc giải quyết nó nhanh chóng, anh ta
CÂN dành thời gian để tìm hiểu tại sao lại xảy ra lỗi đó, nguyên nhân, cách khác phục,
cách dự đoán, để đảm bảo lần sau sẽ không bao giờ lặp lại sai lầm đó nữa.
* Rule #8: Học công nghệ mới. Nếu team của bạn đang sử dụng svn, không có nghĩa là bạn
không nên tìm hiểu git.

## 5. Nguyên tắc tối thiểu

Một nguyên tắc tôi học được từ một cao thủ của diễn đàn HVAOnline mà bản thân tôi thấy
vô cùng hữu ích cho công việc quản trị, bảo mật hệ thống của tôi, đó là "Nguyên tắc tối
thiểu".

Nguyên tắc này giúp đạt ít nhất 3 giá trị sau

* Về hệ thống, đạt hiệu suất
* Về bảo mật, giảm thiểu rủ ro về các vấn đề bảo mật, exploit phần mềm
* Về con người, quản trị dễ dàng

Giải trình nguyên tắc này ntn?

* Cái gì cần thiết thì cài, cái gì không cần thiết thì không cài
* Cài gì cần thiết thì cấu hình, cái gì không cần thiết thì không cần cấu hình
* Dịch vụ nào cần thiết thì mở, dịch vụ nào không cần thiết thì tắt hoặc xóa đi

Ví dụ về nguyên tắc tối thiểu

OS: Bản phân phối CentOS cung cấp một bản cài đặt gọi là Minimum, cài đặt tối thiểu các 
gói cần thiết cho một server. Nếu dùng làm server chẳng có lý do gì cần cài giao diện 
Desktop cả vì phần Xorg, Gnome2 tốn tài nguyên vô ích  vì thêm các ứng dụng như Xorg, 
Firefox, Gnome đồng nghĩa nhiều phần mềm, đồng nghĩa khả năng crash, đụng độ giữa các 
gói phần mềm càng cao

Apache: Apache là một gói phần mềm lớn, gồm nhiều module khác nhau, ví dụ nếu chỉ cần 
vài module tối thiểu cho việc setup web-server cho Wordpress thì chẳng có lý do gì cần 
cài đặt full từ repo vì càng bự thì càng tốn tài nguyên, biên dịch lại Apache làm gói 
nhỏ hơn, tiêu tốn ít tài nguyên hơn  Xóa bỏ các phần config ko cần thiết, giúp dễ 
control được Apache và tránh các lỗi về bảo mật do cấu hình nhầm (quá nhiều comment)

Tương tự với PHP, chỉ cần cài các module cần thiết

Các dịch vụ khác và iptables

Nếu sử dụng server làm web-server thì chẳng có lý do gì cần các dịch vụ như samba, 
smtp, telnet, dns …, các gói đó có thể gỡ bỏ.

Đối với iptables thì nguyên tắc tối thiểu thể hiện ở phương diện Deny All, Allow Select. 
Ví dụ đối với webserver thì Drop tất cả các port, chỉ open duy nhất port http (80), 
có thể open port ssh (22) cho một lớp mạng local.

## 6. Mindset về bảo mật

Người làm SA không phải là một chuyên gia bảo mật (vì lĩnh vực này cực kỳ rộng) nhưng 
anh ta có thể biết một vài khía cạnh của bảo mật, điển hình nhất là vận hành các hệ 
thống Firewall, honeypot, IDS, IPS, các giao thức bảo mật, một số hình thức tấn công,
khai thác lỗi phần mềm…

Anh ta cần phải "tỉnh táo" và chống lại các cám dỗ về __"sự tiện lợi"__, vì sự tiện 
lợi chính là kẻ đối đầu với các nguyên tắc bảo mật. Có một nguyên tắc như sau:

* Bảo mật đi kèm với nguyên tắc tối thiểu
* Bảo mật chống lại sự tiện lợi của user
* Nếu muốn tiện lợi, dễ dàng thì KHÔNG THỂ bảo mật

Ví dụ:

Bạn muốn hệ thống của mình tránh bị brute-force mật khẩu, nhưng bạn lại muốn 
mật khẩu của bạn phải dễ nhớ, dễ copy  NEVER có chuyện đó  vui lòng đặt 
password có độ dài ít nhất là 15 ký tự, gồm chữ thường, chữ HOA, ký tự đặc 
biệt và lưu trữ vào các hệ thống lưu trữ mật khẩu tập trung có mã hóa  
ví dụ __YbUlqz2Yns9ls8/0+JdZ__

Bạn muốn dùng FTP vì tiện lợi, nhưng lại muốn bảo mật giao thức này  NEVER 
có chuyện đó, vì đó là giao thức không có MÃ HÓA khi truyền các thông tin 
data, user, pass trên đường truyền  Sử dụng các giao thức có mã hóa như 
SCP hoặc FTP over SSL/TLS

* Bạn vẫn còn muốn dùng giao thức Telnet ??? → làm ơn, sử dụng SSH
* Bạn không muốn dùng VPN thì tốc độ chậm và vẫn muốn bảo mật  quên đi, làm 
ơn dùng VPN dùm
* Bạn muốn truy cập remote từ public vì qua VPN phức tạp  bạn muốn dễ dàng 
và bảo mật ư  quên đi
* Bạn muốn mở nhiều port để tiện sử dụng → thôi quên chuyện bảo mật luôn đi, 
bạn đang muốn sự tiện lợi chứ không phải bảo mật
* Bạn muốn quản trị MySQL dễ dàng, thôi thì GRANT ALL PRIVILEGES cho khỏe → 
tại sao cần quyền cấp user trong khi không cần thiết → bạn lười thì thôi 
chịu thua, ko bảo mật được
* MD5 là gì, à hash à, ồ cái này có vẻ dễ xài hơn SHA đó, xài thôi bạn nên 
biết những nhược điểm của MD5, và nên biết ng ta đã brute-force được kha 
khá các password được hash bằng MD5 rồi đó
* Xài iptables khó quá, thôi thì để default rule là ACCEPT cho dễ → dễ thì
bó tay với bảo mật
* Hưm, tôi là user, máy tính của tôi tôi muốn cài những gì tôi muốn, policy 
của anh phức tạp quá  chẹp, vậy rủi ro khi cài các phần mềm độc hại là có 
đó nhá cấp quá nhiều quyền và sự tiện lợi cũng không thể bảo mật
* Hưm, anh đặt cái password wifi gì đâu mà khó nhớ và phức tạp thế, lại còn 
dùng WPA2 nữa chứ, sao ko dùng WEP cho "nhanh" → m(

## 7. Vui nhưng không vui

Trang này có rất nhiều gif vui liên quan đến công việc của Dev, SysAdm, DevOps 
→ [link](http://devopsreactions.tumblr.com)

Senior vs junior sysadmin during an outage

![Senior vs junior sysadmin](http://i.imgur.com/hhouPMw)

The way of solving most problems 

![The way of solving most problems](http://i.imgur.com/tfWC9nI)

Friday deployments (…and leaving afterwards) 

![Friday deployments](http://i.imgur.com/R0H5SJh.gif)