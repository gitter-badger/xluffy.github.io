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

To add a remote repository and to keep track of it.

```bash
	~$ git remote add <remote_name> <remote_git_url>
```

### 3.2 remote list

To list all the remote repositories you tracked on the local repository:

```bash
	~$ git remote -v
```

#### 3.3 remote update

To change/ update a remote repository URL:

```bash
	~$ git remote set-url <remote_name> <new_url> <old_url>
```

#### 3.4 remote delete

Don’t want to keep track of a remote repository, then remove (rm) it:

```bash
	~$ git rm <remote_name>
```

### 4. Branch

#### 4.1 Branching off

When there is an issue that you are going to work, the first thing you need to do is to create a new working branch to make any changes on it.

For example, I am going to work on the issue #1 with the title: “Cleaning up the project” of the enhancement type:

```bash
	~$ cd working_hard/
	~$ git fetch upstream
	~$ git checkout upstream/develop -b enhance_1_project_clean_up
	~$ git push origin enhance_1_project_clean_up
```
	
What do the above commands do? First, we need to cd to the working repository. Then get the latest updates from upstream repository (official repository). 
Then branch off (copy) upstream/develop branch content into local branch with name enhance\_1\_project\_clean\_up. Then we push this branch into the origin 
repository (full permission repository).

#### 4.2 Listing branches

To see the list of branches on your local repository::

```bash
	~$ git branch
	~$ git branch -v
	~$ git branch -a
```
You could see something similar like this:

```bash
	* docs_10_useful_most_used_git_commands
	docs_5_workflow
	enhance_4_simpler_entry_access
	feature_1_basic_deployment_virtual_machine_deps_4
	feature_8_ssh_keys_configuration_virtual_machine
	master
	origin/enhance_4_simpler_entry_access
```
The asterisk symbol (*) shows which branch you are currently working on.

3. Switching branches

To switch to another branch:

```bash
	~$ git checkout branch_name
```

### 5. Fetch

Fetching is usually used to get new updated from a remote repository and you could rebase or merge remote branch’s updates into your 
current working branch on the local repository.

```bash
	~$ git fetch upstream
	~$ git merge upstream/develop
```

### 6. Status

One of the most used command to see which files/folders are changed on the currently working branch and get suggestions command to 
add/ remove/ discard these changes.

```bash
	~$ git status
```

### 7. Diff

To see the differences before and after your changes:

```bash
	~$ git diff
```

If your changed files/folders are already added to the committed list, it means you have used the git add command, use the command below:

```bash
	~$ git diff --cached
```
Note You should enable the color mode of git, it is easier to see the changes with colors.

```bash
	~$ git config --global color.ui true
```

### 8. Commit

When making changes to the local repository, these changes must be tracked and committed. To see the changes, 
use `git status`. To commit the changes, use the following commands:

```bash
	~$ git add .
	~$ git commit -a
```

The git add . or git add -A command allows you to add all changed files/folder to the committed list. If you just
want to commit some of these files/folders, you should use the command below instead:

```bash
	~$ git add [path_to_files]
	~$ git commit -a
```

And the terminal will open a default editor (usually vim on linux, mac), add your commit message, write and quite 
(vim: press [i] to enter edit mode, then [ESC] to go into view mode, then :wq to write changes and quit).

You can add your commit message directly to your commit without the vim editor by using the command below instead of 
git commit -a:

```bash
	~$git commit -m "<issue_key>|git commit message"
```

There are cases when you missed something and you want to add more changes into the latest commit:

```bash
	~$ git add .
	~$ git commit --amend
```

`git commit --amend` will allow you to add more changes into the latest commit and edit the commit message. 
Even if you do not want to add any changes but to edit the latest commit message, you should use this command.

### 9. Log

#### 9.1 List

To see a full list of commits, use:

```bash
	~$ git log
```

To see the list of commit messages only, use:

```bash
	~$ git log --online
```
Press [Enter] to scroll the list till the end or press q to quit.

#### 9.2 Search

To search commit logs matching a specific pattern, use:

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

