---
layout: post
title: 删除MIUI系统自带软件
date: 2019-04-5 00:00:00 +0800
categories: note
tag: android
---

android手机出场时自带多种预设应用，致使手机系统看起来比较臃肿,记录卸载系统自带应用过程

要用到的是Android SDK下的一个工具：ADB（Android Debug Bridge）, 使用ADB可以直接操作管理Android模拟器或者真实的Andriod设备。

ADB的主要功能有以下:

- 在Android设备上运行Shell(命令行)
- 管理模拟器或设备的端口映射
- 在计算机和设备之间上传/下载文件
- 将电脑上的本地APK软件安装至Android模拟器或设备上

---

**遂：以下是卸载方法**

操作环境：ubuntu-18.04

首先将手机与电脑连接

安装adb：
```shell
    $ sudo apt install adb
```
查看是否连接正确
```shell
    $ adb devices
    List of devices attached
    5d89a474	device
```
获取root权限
```shell
    $ adb root
```
挂载读写权限
```shell
    $ adb remount
```
进入adb shell 模式
```shell
    $ adb shell
```
列出软件包名，找到软件包于系统对应目录，删除目标应用（Browser），然后重启
```shell
    sagit:/ # pm -l
    sagit:/ # pm path com.miui.browser
    sagit:/ # cd system/priv-app/
    sagit:/system/priv-app # rm -rf Browser
    sagit:/system/priv-app # reboot
```
这样一来就删除了MIUI自带浏览器(Browser)