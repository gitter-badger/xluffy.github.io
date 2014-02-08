---
layout: post
title: FastCGI sent in stderr: "Primary script unknown"
author: xluffy
date:   2014-02-08
tags: nginx, system
description: FastCGI sent in stderr: "Primary script unknown"
---

### Vấn đề:

Bạn setup nginx và php dưới dạng nginx+php-fpm. Nghĩa là khi có một request được gửi tới web server (nginx)
nó sẽ tạo một kết nối bằng TCP hoặc Unix socket tới Application Server (FastCGI Server)
