---
layout: post
title: Broadcast in Android-8.0-Oreo(API>=26)
date: 2019-05-9 00:00:00 +0800
categories: note
tag: android
---
在Oreo中，为了进一步提升用户体验，进一步节省功耗，对应用在后台运行时可以执行的操作又进一步施加了限制。

- **后台服务限制**：处于空闲状态时，限制应用的后台服务。例如：通过静态注册接收开机广播（假设你的设备没做定制，能收到~），并在onReceive方法中启动一个Service，在API 26上，是不允许且会报错的。当然，对于前台服务，这种限制是不存在的。官方说法是：前台服务更容易引起用户注意。

- **广播限制**：除了有限的例外之外，应用无法使用清单注册（静态注册）的方式来接收隐式广播。

    1. 但对于这些隐式广播，可以通过运行时注册（动态注册）的方式注册。
    2. 对于显式广播，则依然可以通过清单注册（静态注册）的方式监听

**Android第一行代码**书中通过在intent中添加action的隐示Broadcast无法在静态注册的BroadcastReceiver中接收到,_故_ 将隐示Intent改称显示Intent:

在A应用发送显示广播,B应用静态注册BroadcastReceived接收:

A应用发送广播:

```java
Intent intent =new Intent("com.example.broadcasttest.MY_BROADCAST");
intent.setComponent(new ComponentName("com.example.broadcasttest2","com.example.broadcasttest2.AnotherBroadcastReceiver"));
sendBroadcast(intent);
```

[ComponentName()官方文档](https://developer.android.com/reference/android/content/ComponentName.html#ComponentName(java.lang.String,%20java.lang.String))
public ComponentName (String pkg, String cls)
- pkg	String：组件所在的包的名称。不能为null。绝不能是这个值null。
- cls	String：实现组件的pkg内部类的名称。不能为空。绝不能是这个值null。

B应用静态注册Broadcast:

```xml
 <intent-filter>
    <action android:name="com.example.broadcasttest.MY_BROADCAST"/>
</intent-filter>
```
