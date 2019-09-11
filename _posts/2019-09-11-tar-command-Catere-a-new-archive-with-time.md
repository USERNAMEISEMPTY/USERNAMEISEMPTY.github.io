---
layout: post
title: tar command _Create a new archive with time
date: 2019-09-11 00:00:00 +0800
categories: note
tag: linux command
---
```shell
        tar -czvf book@`date +%Y-%m-%d`.tar.gz book/
```
-c或--create 建立新的备份文件。

-z或--gzip或--ungzip 通过gzip指令处理备份文件。

-v或--verbose 显示指令执行过程。

-f<备份文件>或--file=<备份文件> 指定备份文件。