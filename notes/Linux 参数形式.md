---
title: Linux 参数形式
tags: [Misc]
created: '2019-02-26T06:29:32.139Z'
modified: '2019-03-03T02:38:20.760Z'
---

# Linux 参数形式
看一下这些命令参数不同的使用方式：
1. 在linux下有些命令这样使用 `ls -a` (参数前一横)
2. 有些命令这样使用 `cp --help` (参数前两横)
3. 还有一些这样使用 `tar -xzvf` (参数前有一横)
4. 而有些这样使用 `tar xzvf` (参数前没有横)

关于命令的使用区别我们一一解释：
* 第一种：参数用一横的说明后面的参数是字符形式
* 第二种：参数用两横的说明后面的参数是单词形式
* 第三种：参数前有横的是 System V 风格
* 第四种：参数前没有横的是 BSD 风格
