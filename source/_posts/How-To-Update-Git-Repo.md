---
title: How to Update Git Repo?
date: 2023-12-21 21:20:38
tags: Git
---

#### 1) Clone the existing repo

(here I use 'https://github.com/ChengZirao/chengzirao.github.io' as an example)

`git clone https://github.com/ChengZirao/chengzirao.github.io`

Then will generate a folder contains all the repo files at your current directory location.

#### 2) Create new folder/file at the cloned repo folder

`mkdir folder_name`

It doesn't matter what way you create this new folder/file, the point is you have to create this folder **INSIDE** your local cloned repo folder.

#### 3) Stage the file for commit to your local repository

`git add folder_name`

*Or simply use `git add .` to stage all the new or modified files (highly recommended)*

#### 4) Commit the file

`git commit -m "write some comments to this update."`

#### 5) Push the changes from your local repo to GitHub

`git push https://github.com/ChengZirao/chengzirao.github.io`

### Well done!!!🎉
