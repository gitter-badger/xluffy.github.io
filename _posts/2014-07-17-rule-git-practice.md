---
layout: post
title:  "Những quy tắc cần nhớ khi sử dụng git"
author: xluffy
date:   2014-07-17
tags: git, developer
description: "Những quy tắc cần nhớ khi sử dụng git"
---

Mô tả những quy tắc __NÊN__ thực hiện khi sử dụng git trong quá trình phát triển phần mềm

Đối tượng: Developer, Coder

Mức độ quan trọng: Cao nhất

### 1. Chỉ commit những thay đổi có liên quan

Một thói quen xấu khi sử dụng svn đó là developer thường commit những thứ không dính dáng gì đến nhau chung vô một commit. 
Ví dụ, fix bug checkout nhưng commit kèm sửa css cho landing-page.

Một commit nên phù hợp với những thay đổi có liên quan đến nó. Fix 2 bug khác nhau thì buộc phải commit với 2 message khác 
nhau. Những commit nhỏ sẽ giúp cho các developer khác dễ dàng hiểu những thay đổi đó và dễ sửa nếu sai.

### 2. Commit thường xuyên 

Ngoài ra việc commit thường xuyên sẽ giúp bạn chia sẽ mã nguồn của bạn với các developer khác trong team một cách liên tục. 
Việc chia sẻ mã nguồn liên tục cực kỳ có lợi trong việc giải quyết các xung đột. Các developer khác có thể pull từng commit 
nhỏ và chủ động resolve các conflict. (sẽ cực kỳ tệ hại nếu bạn commit một commit cực lớn với vô số file và thay đổi, và sẽ 
càng tệ hại hơn nếu xảy ra conflict).

### 3. Không commit khi mới chỉ hoàn thành 1/2 công việc 

Một thói quen xấu của developer khi sử dụng SVN đó là commit lên server để tránh mất code, tuy nhiên điều này lại gây ra một 
vấn đề là ngày hôm sau nếu quên task đó dẫn tới người khác không hiểu commit lên với mục đích gì và không thể sửa được.

Chỉ nên commit mã nguồn khi một tính năng, một bug đã được hoàn thành. Điều đó không có nghĩa là phải hoàn thành toàn bộ một 
chức năng lớn rồi mới được commit. Hoàn toàn ngược lại: chia nhỏ việc thực hiện tính năng lớn thành nhiều phần nhỏ dựa vào 
logic và NHỚ commit mọi thứ một cách đơn giản và thường xuyên.

**HÃY NHỚ**, không commit một vài thứ trước khi rời khỏi văn phòng vào cuối ngày, nếu muốn giữ các commit sạch sẽ, sử dụng 
tính năng Stash của Git.

### 4. Test cẩn thận trước khi commit 

Lại một thói quen xấu khác, đó là không tin tưởng vào môi trường developer, ít khi test trên local mà thường đẩy lên dev 
hoặc test để test và sửa liên tục  điều này làm code sẽ rất lộn xộn và phải commit rất nhiều lần

Bạn phải chống lại một cám dỗ vô cùng to lớn đó là commit khi “**nghĩ**” mọi thứ đã hoàn thành. Phải đảm bảo test và chắc 
chắn nó đã hòan thành và không phát sinh một lỗi phụ nào đó.

### 5. Viết một good commit message 

Một thói quen cực kỳ xấu đó là viết một message tệ, các developer mới tham gia dự án sẽ không thể hiểu được việc commit này 
làm cái gì, làm như thế nào, fix bug số bao nhiêu…

Xem thử một bad commit message

Vậy viết thế nào cho tốt:

Bắt đầu viết một message với một đoạn tổng kết ngắn về những thay đổi (khỏang 50 ký tự như guideline). Sau đó tách phần mô tả
và phần body bằng một dòng trắng. Phần body phải trả lời cho các câu hỏi sau: Động cơ của việc thay đổi này là gì? Sự khác 
nhau với lần thực hiện trước đó là gì?

[http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html|http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html|http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)

[http://web-design-weekly.com/2013/09/01/a-better-git-commit/|http://web-design-weekly.com/2013/09/01/a-better-git-commit](http://web-design-weekly.com/2013/09/01/a-better-git-commit/|http://web-design-weekly.com/2013/09/01/a-better-git-commit)

Việc viết một commit message tốt cực kỳ quan trọng, xem qua một cao thủ viết message như thế nào. 

Về bản chất thì guide vẫn là như nhau, chỉ cập nhật thêm một số điểm mà tôi thấy khá hay.

Về format của một message: Một commit cần chứa 1 **header**, 1 **body** và 1 **footer**. Header là phần đặc biệt gồm 1 **type** và 1 **subject** 

```bash
	<type>: <subject>

	<BLANK LINE>

	<body>

	<BLANK LINE>

	<footer>
```

Một khuyến cáo là bất kỳ dòng nào trong một commit msg cũng không nên dài quá 100 ký tự (tốt nhất là 72 ký tự). Điều này cho phép một thông điệp có thể dễ dàng đọc trên github hoặc các git tool.

__Type:__ Phải là một trong các thứ sau đây

- feat: Một tính năng mới
- fix: Một bug được fix
- docs: Thay đổi về tài liệu (document)
- style: Thay đổi nhưng không ảnh hưởng đến code (white-space, formatting, missing semi-colons, etc)
- refactor: A code change that neither fixes a bug or adds a feature
- perf: Một đoạn mã cải thiện performance
- test: Adding missing tests
- chore: Thay đổi quá trình thực hiện, các tool phụ trợ, các thư viện.

__Subject:__ Subject là phần chứa mô tả ngắn gọn về msg, tuân theo 

- Bắt buộc sử dụng thì hiện tại
- Không viết hoa chứ cái đầu
- Không sử dụng dấu `.` (dot) cuối subject

__Footer__

```bash
	Fixes #5784
	Closes #5785
```


### 6. Không sử dụng VCS như một hệ thống backup 


VCS tất nhiên không phải là một hệ thống backup, vì thế không sử dụng nó cho mục đích lưu trữ file (kiểu như đẩy lên để khỏi quên), 
chỉ đẩy file nên khi bạn có một message rõ ràng, khi đã hoàn thành công việc.

### 7. Học cách sử dụng branches và sử dụng thường xuyên 

Branches là một tính năng vô cùng mạnh mẽ của Git, và nó không có bất cứ một tại nạn hay sự nguy hiểm nào, nhanh chóng và dễ dàng. 
Branches là một công cụ hoàn hảo giúp cho việc tránh lẫn lộn giữa các đường phát triển phần mềm. Và nên sử dụng branches cho tất 
cả các development workflows như: tính năng mới, fix bug, ideas…

### 8. Chấp nhận những rule trên 

Có một cám dỗ vô cùng lớn đó là cảm giác “sung sướng” khi commit và push. Bạn buộc phải chấp nhận workflow và tránh khỏi cám dỗ đó
bằng mọi cách. Again, đừng commit khi công việc chưa hoàn thành, đừng commit khi chưa test, đừng commit khi bạn biết thừa nó có cả 
tá lỗi, đừng commit chỉ với mục đích để đó sau làm tiếp.
