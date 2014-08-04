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
merge những update đó vào working-branch hiện tại bạn đang làm việc ở local. Bạn nên lưu ý là featching chỉ lấy các thay đổi 
mới nhất từ kho xa về kho local của bạn chứ chưa thay đổi trên nhánh mà bạn đang làm việc, bạn cần phải merge từ kho local vào
nhánh bạn đang làm việc để cập nhật những thay đổi đó.

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

Nhấn [Enter] để cuộn trang và [q] để thoát.

#### 9.2 Search

Để search một commit-log khớp với một pattern cụ thể nào đó, sử dụng:

```bash
	~$ git log -g --grep=<pattern>
```

### 10. Push

Sau  một và xác nhận, bạn có thể muốn push nó lên origin repository, làm như sau:

```bash
	~$ git push origin enhance_1_project_clean_up
```

Một vấn đề có thể xảy ra khi bạn đẩy dữ liệu lên kho xa đó là đồng nghiệp có thể cũng đang đẩy "đồng thời" nên một nhánh của cùng một kho.
Nếu bạn là người thực hiện sau một chút, thì git không cho phép bạn ghi đè nên kết quả của người kia. Git sẽ dùng git log trên server ở xa
kiểm tra xem kho local của bạn đã quá hạn chưa. Nếu quá hạn, git sẽ từ chối và bạn cần fetch về và trộn lại trước khi đẩy lên kho xa.

```bash
	~$ git push origin master
	To git@github.com:xluffy/bike.git
	 ! [rejected]        master -> master (non-fast-forward)
	error: failed to push some refs to 'git@github.com:xluffy/bike.git'
	To prevent you from losing history, non-fast-forward updates were rejected
	Merge the remote changes before pushing again.  See the 'Note about
	fast-forwards' section of 'git push --help' for details.
```

Để sửa

```bash
	~$ git fetch origin
	~$ git merge origin/master
```

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

Khái niệm này không được giới thiệu bởi git mà bởi github, nó không phải là một lệnh trong git. Pull Requesst là một hành động trên web UI 
của github (bitbucket) để thông báo với upstream, bạn đã hoàn thành công việc và muốn merge vào upstream repo 

### 14. Reset

Reset có nghĩa là bạn muốn thiết lập working-branch về một commit-history cụ thể nào đó và cả các thay đổi 

```bash
	~$ git reset HEAD~<index>
```
hoặc

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

stash là một ngăn xếp (stack) và nó rất thường được sử dụng khi bạn muốn lưu những thay đổi một cách tạm thời từ working-branch mà không
nhất thiết phải commit để chuyển qua một branch khác, stash hoạt động giống như stack. Thường, bạn cần lưu tạm lại các thay đổi

```bash
	~$ git add .
	~$ git stash
```

#### 15.2 stash list

Xem tất cả danh sách stash:

```bash
	~$ git stash list
```
#### 15.3 show it

Xem thay đổi của một stash cụ thể:

```bash
	~$ git stash show stash@{<index>}
```

#### 15.4 apply it

Khi switch lại repository có chứa stash, bạn có thể lấy lại các thay đổi từ stash và làm việc tiếp.

Để lấy lại nội dung stash cuối cùng và apply các thay đổi đó vào working-branch hiện hành:

```bash
	~$ git stash apply
```

Để apply thay đổi của một stash cụ thể vào working-branch hiện hành
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

Bạn có thể học thêm nhiều lệnh git bằng cách sử dụng lệnh sau để mở Help hoặc

```bash
	~$ git --help
	~$ git <command> --help
	~$ man git-<command>
```

