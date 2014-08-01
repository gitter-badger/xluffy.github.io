---
layout: post
title:  "Các command-line thường sử dụng của Git"
author: xluffy
date:   2014-07-21
tags: git, developer, cmd
description: "Các command-line thường sử dụng của Git"
---

### 1. Fork

`Fork` thực chất không phải là command-line của git, nó là một tính năng được đề xuất của Github, 
và sau này cả bitbucket cũng sử dụng. Thao tác `fork` được thực hiện trên giao diện web, không phải
là command-line.

### 2. Clone

Cloneing nghĩa là bạn copy một remote repository vào trên ổ cứng của bạn và làm việc. Thông thường clone
repo mà bạn fork chứ không clone repo chính.

```bash
	~$ cd working_hard/
	~$ git clone git@github.com:xluffy/bike.git or
	~$ git clone https://github.com:/xluffy/bike.git
```

### 3. Remote

Ta có local và remote repositories. Local repository là repository trên ổ cứng của bạn. Remote repository là repository được
lưu trữ ở một nơi nào đó, ví dụ git server. Bạn tạo ra các thay đổi trên local repository trên máy tính của bạn và `push` những
thay đổi đó lên remote repository.

Theo quy ước, có 2 remote repositories được sử dụng thường xuyên.

+ `origin`: là repository bạn clone về trên máy tính từ `fork repo`.
+ `upstream`: là official repository bạn cần theo dõi và track nó.

#### 3.1 remote add

Dùng để add một repo ở xa và theo dõi nó

```bash
	~$ git remote add <remote_name> <remote_git_url>
```

### 3.2 remote list

Dùng để liệt kê tất các repo ở xa mà bạn đang theo dõi trên repo ở local:

```bash
	~$ git remote -v
```

#### 3.3 remote update

Dùng để thay đổi/cập nhật lại URL của một repo ở xa:

```bash
	~$ git remote set-url <remote_name> <new_url> <old_url>
```

#### 3.4 remote delete

Bạn không muốn theo dõi repo ở xa nữa, xóa nó đi:

```bash
	~$ git rm <remote_name>
```

### 4. Branch

#### 4.1 Branching off

Khi có một issue, bạn sẽ cần làm việc, đầu tiên cần tạo một working-branch mới để tạo các thay đổi

Ví dụ, tôi làm việc trên issue #1 với tiêu đề: “Cleaning up the project”:

```bash
	~$ cd working_hard/
	~$ git fetch upstream
	~$ git checkout upstream/develop -b enhance_1_project_clean_up
	~$ git push origin enhance_1_project_clean_up
```
	
Các lệnh trên có ý nghĩa gì và làm gì? Đầu tiên, chúng ta cần chuyển vào working repository. Sau đó lấy code mới nhất về từ upstream repository 
(official repository). 

Sau đó branch off nội dung (copy) branch upstream/develop  vào local branch với tên là enhance\_1\_project\_clean\_up. Sau đó push branch này 
lên origin repository (full permission repository).

#### 4.2 Listing branches

Liệt kê tất cả các branch ở local và cả remote (-a):

```bash
	~$ git branch
	~$ git branch -v
	~$ git branch -a
```

Đại loại bạn có thể thấy một số kết quả như sau:

```bash
	* docs_10_useful_most_used_git_commands
	docs_5_workflow
	enhance_4_simpler_entry_access
	feature_1_basic_deployment_virtual_machine_deps_4
	feature_8_ssh_keys_configuration_virtual_machine
	master
	origin/enhance_4_simpler_entry_access
```

Với ký hiệu (*) chỉ branch mà bạn đang đứng làm việc.

3. Switching branches

Để chuyển qua một branch khác:

```bash
	~$ git checkout branch_name
```

### 5. Fetch

Fetching là một lệnh được sử dụng cực kỳ thường xuyên, mục đích để lấy code mới từ remote repository và bạn có thể rebase hoặc 
merge những update đó vào working-branch hiện tại bạn đang làm việc ở local.

```bash
	~$ git fetch upstream
	~$ git merge upstream/develop
```

### 6. Status

Một lệnh khác cũng được thường xuyên sử dụng để xem các file/folder nào đã được thay đổi trên working-branch hiện tại và suggest các
lệnh như add/remove/discard những thay đổi đó.

```bash
	~$ git status
```

### 7. Diff

Để hiển thị sự khác nhau trước và sau thay đổi của bạn:

```bash
	~$ git diff
```

Nếu thay đổi của bạn trên file/folder đã được add và danh sách commit, điều đó có nghĩa là bạn có sử dụng `git add`, sử dụng lệnh dưới:

```bash
	~$ git diff --cached
```

Lưu ý, nên bật color-mode cho git, cho dễ nhìn

```bash
	~$ git config --global color.ui true
```

### 8. Commit

Khi tạo ra thay đổi trên local-repo, những thay đổi đó cần phải được theo dõi và xác nhận. Xem thay đổi bằng
`git status` và xác nhận thay đổi đó bằng lệnh đưới:

```bash
	~$ git add .
	~$ git commit -a
```

Lệnh `git add .` hoặc ` git add -A` là lệnh cho phép add tất cả các thay đổi của file/folder vào danh sách chờ 
xác nhận. Nếu chỉ muốn xác nhận một vài file/folder, bạn có thể sử dụng lệnh sau:

```bash
	~$ git add [path_to_files]
	~$ git commit -a
```

Và terminal sẽ mở một chương trình editor mặc đinh (vim, nano chẳng hạn), bạn viết __thông điệp xác nhận__. Với
vim nhấn [i] để vào chế độ Insert, sau đó [ESC] để về chế độ view, và :wq để lưu và thoát.

Hoặc có thể add __thông điệp xác nhận__ trực tiếp mà không cần thông qua editor bằng lệnh sau 

```bash
	~$git commit -m "<issue_key>|git commit message"
```

Trong trường hợp bạn commit sai một cái gì đó, có thể sử dụng lệnh sau để thay đổi (lưu ý chỉ lần commit cuối cùng)

```bash
	~$ git add .
	~$ git commit --amend
```

`git commit --amend` cho phép sửa những thay đổi, nhưng nếu chỉ muốn sửa mỗi thông điệp mà vẫn giữ nguyên những file/folder
đã thay đổi thì cũng có thể sử dụng lệnh này.

### 9. Log

#### 9.1 List

Để xem full-list các commit, sử dụng:

```bash
	~$ git log
```

Chỉ xem commit-message, và trên 1 dòng:

```bash
	~$ git log --oneline
```

Nhấn [Enterơ] để cuộn trang và [q] để thoát.

#### 9.2 Search

Để search một commit-log khớp với một pattern cụ thể nào đó, sử dụng:

```bash
	~$ git log -g --grep=<pattern>
```

### 10. Push

After some commits and you would like to push them into the origin repository, do as follows:

```bash
	~$ git push origin enhance_1_project_clean_up
```

Sometimes when there is diversity which means there are difference between git commit list on the local and remote branch, git does not allow you to push. In that case, you need to force push (means that you want to have you local changes put into the origin repository, keep only commit history of local repository.

```bash
	~$ git push origin enhance_1_project_clean_up -f
```

If you want to keep the origin and make the local changes to resolve different commit list, you can use git rebase to make the history be fast-forwarded.

Fast-forward means that your local repository is in sync with remote repository with some additional commits. When you push, the additional commits will be appended into remote history repository.

WARNING: force push can make you lost some commits if you are not careful enough. This is true when you merge work from other branches into your local branch. In such case, you could use git reflog, find the hash commit and git reset --hard <hash> to get back the changes.

Note NEVER EVER force push the official repositories.

### 11. Rebase

rebase means that you want to keep the remote’s commit list as base, any changes of local branch should be reorganized and appending to the remote repository. You usually rebase to get the latest changes from the upstream repository.

```bash
	~$ git fetch upstream
	~$ git rebase upstream/develop
```

If there is any conflict, resolve it, then $ git add . and $ git commit -a. Do it until git says that you are done; Or you could abort the rebase process by $ git rebase --abort. Everything will come back before the rebase process after abort.

### 12. Merge

merge is used to join 2 or more commit histories together (from different branches).

### 13. Pull request
This concept is not introduced by git but github, which means that you do not have any git command here. Pull request is done on the web UI of github (bitbucket) to notify the upstream that your work is great, finished and you want your work to be merged into the upstream repository.

### 14. Reset
Reset means that you could set the working branch to a specific commit history and see all the changes.

```bash
	~$ git reset HEAD~<index>
```
or:

```bash
	~$ git reset <commit_hash_id>
```
This is useful to view all the changes from some commits of your collaborators for an issue.

reset hard

To discardss all the current changes on the working branch:

```bash
	~$ git reset --hard
```

To set the current working branch to a specific commit and discard all the changes, use one of two following commands:

```bash
	~$ git reset --hard HEAD~<index>
```

```bash
	~$ git reset --hard <commit_hash_id>
```
To reset the working branch to a remote branch:

```bash
	~$ git reset --hard <remote_repo>/<branch_name>
```

### 15. Stash

#### 15.1 stash it

stash is a stack and is usually used when you want to store temporarily changes from a working branch instead of committing these changes 
to switch to another branch. stash is a stack like. Usually, you need to store all the changes:

```bash
	~$ git add .
	~$ git stash
```

#### 15.2 stash list

To see all the stashed list:

```bash
	~$ git stash list
```
#### 15.3 show it

To show the changes from a specific stash:

```bash
	~$ git stash show stash@{<index>}
```

#### 15.4 apply it

When switching back the repository having stash, you could get the changes from stash.

To get the latest stashed content and apply changes to the current working branch:

```bash
	~$ git stash apply
```

To apply changes from a specific stash into the current
working branch:

```bash
	~$ git stash apply stash@{<index>}
```

#### 15.5 drop it

To drop a specific stash:

```bash
	~$ git stash drop stash@{<index>}
```

Squash
Warning: Squash is used to rewrite your git history, so use on your own full permission repository ONLY.

squash means you choose one or some commits and amend to its previous commit to be a single commit.

When working, you “commit early, commit often”, and you get a list of commits that is hard for your collaborators to review, and each commit is not atomic itself. Each commit should be an atomic problem solving step, that is the reason why you need squash:

```bash
	~$ git rebase -i HEAD~<index>
```

Use s instead of pick for the commits you want to squash.

### 17. Learn More
You can learn more git commands by using one of the following commands to open the Help page, or ask us, Teraciers, for work and practice.

```bash
	~$ git --help
	~$ git <command> --help
	~$ man git-<command>
```

