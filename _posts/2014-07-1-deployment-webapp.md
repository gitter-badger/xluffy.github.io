---
layout: post
title:  "Deploy một ứng dụng web"
date:   2014-03-03
tags: deployment, php, web-app
description: "Deploy một ứng dụng web"
---

Bài viết dựa trên slide của tác giả, kèm thêm một số nhận định và kinh nghiệm cá nhân

#### Tổng quan

Deploy là hành vi đưa phần mềm, ứng dụng, website, tính năng, phiên bản tới cho user có thể sử dụng.

Đối với software deployment khá tương đồng với khái niệm release, đặc điểm của software deployment là thời gian
để đưa ra một phiên bản khá là dài, 1 tuần, 1 tháng hoặc lâu hơn.

Đối với webapp deployment là đưa source code mới lên, cập nhật tính năng. Với PHP code, thì việc deploy chỉ là đưa các thay đổi của source code lên chứ không phải là đưa tất cả source lên thay thế code cũ.

#### Mục tiêu
 
- One-click/Continuous deploys
- Anytime/AnyWhere
- AnyOne
- Đáng tin cậy
- Có khả năng rollback
- No-downtime
- Tái sử dụng
- Có khả năng mở rộng

#### Phương thức

- Vim-style
- FTP Upload
- Rsync (daemon, over ssh)
- Source control (svn, git)
- Build tools (ant, phing)
- Specialize tools (fabric, Capistrano)

##### First Time

- Copy file lên server (một hoặc nhiều)
- Cấu hình phía server (connect DB, memcache)
- Load DB
- Install Assets (vendor, images)
- Warm up cache
- Enable Site

##### Sub-scene time

- Copy file lên server
- Apply DB update (migrations)
- Install Assets (vendor, images)
- Warm up cache
- Enable Site

#### Deployment: Challenge

Challenge: Nhanh và đáng tin cậy trong việc copy file

Solutions:
- rsync
- git pull

Challenge: Scalable

Solutions:
- Sử dụng tools cho phép đẩy đồng thời lên 1 hoặc nhiều server-node (fabric)
- pssh cho phép gửi command tới đồng thời nhiều server (parallel)

Challenge: Roll-back

Solutions:
- Test!
- Sử dụng tag để releases
- Delicate branches (master cho production)
- Deploy mỗi phiên bản release trong một thư mục riêng

Challenge: Secure

Solutions:
- Sử dụng ssh
- Không lưu trữ password trong source control
- Lưu trữ các biến nhạy cảm (password) ở biến môi trường trên server.

Challenge: DB Migration

Solutions:

Challenge: Static asset

Solutions:
- Dùng YUICompress để thu nhỏ kích thước các file CSS, JS
- Bật gzip trên webserver
- Add version cho các links static (product?v=1)
- Gộp nhiều file static vào làm một
- Chạy các thao tác nén, gộp trên deploy env hoặc staging env rồi mới deploy

Challenge: Caching

Solutions:

Challenge: File permission conflict

Solutions:
- Chạy nginx/php chung một user
- PHP-FPM thay cho mod_php
- ...








