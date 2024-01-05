---
title: Git Push Was Rejected
date: 2023-12-22 10:33:23
tags: Git
---

When I try to update the "blog_img" folder, there was an error below💥:

`error: failed to push some refs to 'https://github.com/ChengZirao/my-blog-deployment'`

`hint: Updates were rejected because the remote contains work that you do`
`hint: not have locally. This is usually caused by another repository pushing`
`hint: to the same ref. You may want to first integrate the remote changes`
`hint: (e.g., 'git pull ...') before pushing again.`
`hint: See the 'Note about fast-forwards' in 'git push --help' for details.`

Probably because I just cloned this repo minutes ago and which causes conflicts. But it doesn't matter, solution to this error is to override any checks that Git does by using "force push". Use this command in the terminal:

`git push -f https://github.com/ChengZirao/my-blog-deployment`

'-f' stands for 'force push'.
