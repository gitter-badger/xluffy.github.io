---
layout: post
title:  "Git work-flow for startup"
author: xluffy
date:   2014-07-17
tags: git, developer, workflow
description: "Git work-flow for startup"
---

Vấn đề đau đầu nhất là work-flow cho một quy trình phát triển phần mềm ổn định, có thể theo dõi, có thể test…
Git là một mô hình mở, nó có thể phù hợp với bất cứ một workflow nào. Vấn đề là work-flow đó có đáp ứng được 
những nhu cầu cụ thể của từng team hay không

Hiện tại có thể thấy có 2 work-flow khá nổi tiếng là Git Flow và Github Flow. Hai work-flow này giúp cho việc 
quản lý project và tối ưu workflow làm việc của team. Hãy điểm qua một vài khác biệt.

Một cách so sách dễ thấy 2 flow này có ảnh hưởng lớn đến như thế nào đó là Github flow tất nhiên đang được Github
sử dụng, git-flow đang được Bitbucket áp dụng. Github và Bitbucket có lẽ là 2 social code nổi tiếng nhất và nhiều
người sử dụng nhất.

Để hiểu về Github-Flow có thể đọc thêm tại

[https://guides.github.com/introduction/flow/index.html](https://guides.github.com/introduction/flow/index.html)

[http://scottchacon.com/2011/08/31/github-flow.html](http://scottchacon.com/2011/08/31/github-flow.html)

Để hiểu thêm về git-flow có thể đọc thêm tại

[https://www.atlassian.com/git/workflows#!workflow-gitflow](https://www.atlassian.com/git/workflows#!workflow-gitflow)

[http://nvie.com/posts/a-successful-git-branching-model/](http://nvie.com/posts/a-successful-git-branching-model/)

Tôi sẽ không nói thêm về 2 work-flow trên nữa, dưới đây là một work-flow hybrid, khá ổn tôi đọc từ blog của một startup.

## 1. Bài toán
viết sau

## 2. Thực hiện

### 2.1 Initializing Working Repositories

Để làm việc trên một dự án, `fork` là điều đầu tiên cần làm. Working-Repository `PHẢI` là clone của repo mà chính bạn fork ra
và đặt ở thư mục `workspace/personal`

Ví dụ Cty tôi có 1 project đặt tại [https://github.com/vietnam/bike](https://github.com/vietnam/bike) với bike là tên của repo. 
Các bước thực hiện sẽ như sau

Step 1: Fork repo đó về account của bạn (tùy: Github, Bitbucket, Gitlab...)

Sau khi `fork` bạn sẽ được 1 repo với URL như sau [https://github.com/xluffy/bike](https://github.com/xluffy/bike). Nghĩa là bất
cứ lập trình viên nào cũng bắt buộc phải `fork` repo chính ra.

Step 2: Clone về để làm việc

Nơi làm việc của mỗi lập trình viên gọi là working-repository, để clone về ta dùng lệnh.

```bash
	~$ cd workspace/personal
	~$ git clone git@github.com:xluffy/bike.git
```

Lưu ý là clone repo bạn `fork` ra chứ không phải clone từ repo chính.

Step 3: Add `upstream` repo là repo lưu trữ chính

```bash
	~$ git remote add upstream git@github.com:vietnam/bike.git
```

Kết quả phải ra được như sau

```bash
	~$ git remote -v
	origin  git@github.com:xluffy/bike.git (fetch)
	origin  git@github.com:xluffy/bike.git (push)
	upstream  git@github.com:vietnam/bike.git (fetch)
	upstream  git@github.com:vietnam/bike.git (push)
```

Với `origin` là remote repo của bạn

Với `upstream` là remote của official repo.

### 2.2 Git Branching Off

viết sau

### 2.3. Working with Git

#### 2.3.1 Workflow in Vietnam

![Workflow in Vietnam](http://i.imgur.com/hME1IY2.png)

Tổng kết thì nó gồm 4 bước chính:

Step 1: Branching-off based on issue

Step 2: Developing with Code/ Commit/ Push

Step 3: Submitting pull-request. Waiting for approval or resolving conflict if any.

Step 4: Cleaning up branch

Let’s get in more detais:


Step 1: Branching-off based on issue

If you do not know what the meaning of “Branching-off” is, please check Git Branching Off.

Working on features

```bash
	~$ git fetch upstream
	~$ git checkout upstream/master -b features/<issue_key>-<concise_title>
	~$ git push origin features/<issue_key>-<concise_title>
```

Working on improvements

```bash
	~$ git fetch upstream
	~$ git checkout upstream/master -b improvements/<issue_key>-<concise_title>
	~$ git push origin improvements/<issue_key>-<concise_title>
```

Working on tasks or sub-tasks

```bash
	~$ git fetch upstream
	~$ git checkout upstream/master -b tasks/<issue_key>-<concise_title>
	~$ git push origin tasks/<issue_key>-<concise_title>
```
Working on bugs

```bash
	~$ git fetch upstream
	~$ git checkout upstream/master -b bugs/<issue_key>-<concise_title>
	~$ git push origin bugs/<issue_key>-<concise_title>
```
Above are the templates Branching off based on an issue’s types.

Step 2: Developing with Code/ Commit/ Push

During your coding, you would make some commit and push, in that case you have to check TWO things:

Quality Checklist
Git Commit Messages
If there are some changes from the remote branch (for example, upstream/master) that you need, you have to rebase your
branch with these updates. It could be done by these commands:

```bash
	~$ git fetch upstream
	~$ git rebase upstream/master
```

By doing this, your branch will be rebased with updates from others. If it has any conflicts, you have to resolve them by:

Editing conflict file.
The sample on a conflict file:

_images/conflict-mark.png
The sample on a resolved-conflict file:

_images/conflict-resolved.png
Adding conflict-resolved-file in git, then continuing to rebase.

```bash
	~$ git add path/to/conflict-resolved-file
	~$ git rebase --continue
```	
After finishing your work, add changed files to commit and push your branch:

```bash
	~$ git add -a
	~$ git commit -m "<issue_key>|git commit message"
	~$ git push origin [your-branch-name]
```

Step 3: Submitting Pull-request

When your issue branch is pushed, submit pull-request for reviewing on your work. There are TWO steps in submitting a pull-request:

Create Pull-request for your code.
Open the Create Pull Request form:
_images/submit-pull-request-code-1.png
Input the neccessary information into the form:

_images/create-pull-request-form.png
Copy the pull request link on the browser’s address bar.

Add Pull-request to your issue.
Open your issue –> Click Workflow –> Click Send Pull Request.

_images/submit-pull-request-issue.png
Paste the pull request link into the Pull Request URL, then click Send Pull Request in the Send Pull Request form.

_images/send-pull-request-form.png
Note After a pull request, you will continue to work on your working branch as normal, just push it and the pull request will be 
updated with your new commits. Ping other Teraciers to help reviewing, comments, suggestions, etc.

When you meet all these long strict requirements, your work will be more welcomed accepted.

Step 4 : Cleaning up branch

After your code get reviewed and approved. It will be merged to the offical repository, so you have to make a Git Branch Cleaning Up
to clean up your local and get ready for the next issue.