---
layout: post
title:  "Một số mẹo sử dụng git từ nhóm ruby-vn"
author: xluffy
date:   2014-08-23
tags: git, ruby-vn
description: "Một số mẹo sử dụng git từ nhóm ruby-vn"
---

Mẹo Gít cuối tuần:

Nếu bạn có nhiều files mới trong repo của bạn và những files này không có trong git history, thay vì xoá bằng tay thì có thể dùng

```bash
	~$ git clean -df
```

Mẹo Git của tuần:

Muốn xem khác biệt giữa các file đã đc add vào staging (không phải là staging branch nha) thì dùng lệnh:

```bash
	~$ git diff --staged
```