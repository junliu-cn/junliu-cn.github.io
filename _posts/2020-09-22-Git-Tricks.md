---
title: 'Git Tricks'
date: 2020-09-22
permalink: /posts/2020/09/Git-Tricks/
tags:
  - Git
---

git tricks for better experience.

### git 丢弃本地未提交修改
- git checkout .

### How to storage credentials for github repository
Run the following command to enable credentials storage in your Git repository:
- git config credential.helper store

To enable credentials storage globally, run:
- git config --global credential.helper store

### Git blame找出文件的历史改动信息
- git blame -L 100,200 <filename>

### Git submodule
If you already have cloned a repository and now want to load it’s submodules you have to use submodule update.
- git submodule update --init

if there are nested submodules: 
- git submodule update --init --recursive

##### add submodule
- git submodule add link


###  Git branch-a如何从branch-b分支merge单个或部分文件
- git checkout branch-a
- git checkout branch-b filename1 filename2 ...

###  Git lfs
#### install on mac
- brew install git-lfs
- git install lfs
- git clone some repository

###  pip install from git repo
- pip install git+url

###  Git 同步A分支上某次提交B到分支
- 使用git log 查看提交的信息,记住commit id.

- git checkout 要修改的分支

- git cherry-pick 某个commit id   // 把某个commit id的提交合并到当前分支.


###  Git stash usage
saving current content
- git stash
 
show stash list
- git stash list

recover stash content
- git stash apply
- git stash pop

###  git clone --recursive
- With version 1.9 of Git and later, you can even download the submodules simultaneously:

- git clone --recursive -j16 git://github.com/foo/bar.git
cd bar

###  git "查找字符串"
- git grep -n "要查找的字符串"
- git grep --count "要查找的字符串" # 显示文件名称以及字符串出现的次数
- git grep -nW "要查找的字符串" # 同时显示上下文

###  git change remote repo url
1. amend url directly
- git remote set-url origin [url]
2. delete old url and then add new one 
- git remote rm origin
- git remote add origin [url]
- git pull --allow-unrelated-histories
- git branch --set-upstream-to=origin/master master
2. 在推送的同时，设置upstream
- git push -u origin master

### # git change commit message
- git commit --amend

###  git merge two commit 
- git rebase -i commit_id (commit id is hash value which is should not be merged)


###  [Push local Git repo to new remote including all branches and tags](https://stackoverflow.com/questions/6865302/push-local-git-repo-to-new-remote-including-all-branches-and-tags)
To push all your branches, use either (replace REMOTE with the name of the remote, for example "origin"):

- git push REMOTE '\*:\*'
- git push REMOTE --all

To push all your tags:

- git push REMOTE --tags

Finally, I think you can do this all in one command with:

- git push REMOTE --mirror

However, in addition --mirror, will also push your remotes, so this might not be exactly what you want.

###  git undo git add
- git reset
- git reset <file>


###  Quick setup — if you’ve done this kind of thing before
#### …or create a new repository on the command line
- echo "# ImsightLabelKit" >> README.md
- git init
- git add README.md
- git commit -m "first commit"
- git remote add origin https://github.com/chrisluu/ImsightLabelKit.git
- git push -u origin master
### …or push an existing repository from the command line
- git remote add origin https://github.com/chrisluu/ImsightLabelKit.git
- git push -u origin master
### …or import code from another repository
- You can initialize this repository with code from a Subversion, Mercurial, or TFS project.

###  How do you create a remote Git branch?
1. First, you create your branch locally:
- git checkout -b your_branch
2. After that, you can work locally in your branch, when you are ready to share the branch, push it. The next command push the branch to the remote repository origin and tracks it
- git push -u origin your_branch
3. Teammates can reach your branch, by doing:
- git fetch
- git checkout origin/your_branch
- git fetch <remote> --prune # Delete multiple obsolete tracking branches

###  Delete Local Branch
- git branch -d branch_name
- git branch -D branch_name

### Delete remote Branch
- git push [远程名] :[分支名]
- git push origin :branchName

###  Tag
### 
- git tag             #列出所有标签
- git tag -l '1.0.*'  #搜索符合模式的标签

### Add tag
- git tag 1.0.1 -light     #创建轻量标签
- git tag -a 1.0.1         #创建一般的附注标签
- git tag -a 1.0.1 -m '.'  #创建带附注的附注标签
- git tag -a 1.0.11.180411 -m "message"

### Delete tag
- git tag -d 1.0.1        #删除标签

### Checkout tag
- git checkout 1.0.1      #跳转到某标签
- git show 1.0.1          #查看标签版本信息

### Push tag to server
- git push origin 1.0.1   #push origin的标签到服务器
- git push origin --tags  #所有tag都提交

###  Replace all file which have source string with target string 
1. git ls-files | xargs sed -i 's#Lung#LabelKit-DR#g'
2. sed -i 's/Lung/LabelKit-DR/g' $(git ls-files)
