---
title: How to Roll Back to A Specific Commit?
date: 2024-01-05 15:30:13
tags: Git
---

Sometimes even smart ones like me may make mistakes in coding. Luckily, we can repeat the past by using Git to roll back to previous versions of content!

#### 1) List all the versions of commit

`git log`

Here you will get your commit history like this:

![git-log.png](https://github.com/ChengZirao/my-blog-deployment/blob/master/blog_img/git-log.png?raw=true)

#### 2) Find the commit ID you want to roll back

Suppose you want to roll back to the second commit, the commit ID is '210289e5a15c3d05f981eedb9b6ab14091f0ecc9'. Then type

`git reset 210289e5a15c3d05f981eedb9b6ab14091f0ecc9`

#### 3) Push the commit

`git push https://github.com/ChengZirao/my-blog-deployment`

### Well done!!!🎉