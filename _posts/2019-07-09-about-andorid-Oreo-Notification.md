---
layout: post
title: Http data change in Android9.0(API=28)
date: 2019-07-09 00:00:00 +0800
categories: note
tag: android
---
今天在使用HttpUrlConnection使，AS报错：
```shell
    java.io.IOException: Cleartext HTTP traffic to www.bing.com not permitted
```
查了一下才知道android9.0（API=28）中限制了android应用发送明文的数据，解决方案如下：

1.改用https

2.在AndroidManifest文件中的application标签中添加如下属性
```java
    android:usesCleartextTraffic="true"
```