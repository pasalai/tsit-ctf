---
title: Git_Frequently_used_commands_Git常用命令
date: 2019-12-21 19:31:36
tags: [笔记 ]
categories: [笔记 ]
---

# Git_Frequently_used_commands_Git常用命令
[![Git_Frequently_used_commands_Git常用命令](https://i.jpg.dog/img/2916b5f9a5bc1845c27a02f3704b74f3.jpg)](https://jpg.dog/i/O7Xe2)

## 遇到的问题及解决方案
* 在使用git 对源代码进行push到gitHub时可能会出错，出现错误的主要原因是github中的某文件不在本地代码目录中，可以通过如下命令进行代码合并[注：pull=fetch+merge]` git pull --rebase origin master`
执行上面代码后可以看到本地代码库中多了README.md文件，此时再执行语句 `git push` 即可完成代码上传到github
