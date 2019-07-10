---
layout: post
title: Notification create change in Android8.0(API>=26)
date: 2019-06-23 00:00:00 +0800
categories: note
tag: android
---
在android8.0以上想要发送通知（send Notification）要通过通知渠道（NotificationChannel）来发送

[Build Notification-android 官网](https://developer.android.google.cn/training/notify-user/build-notification.html)

创建通知渠道：
```java
        private String createNotificationChannel() {
        // Create the NotificationChannel, but only on API 26+ because
        // the NotificationChannel class is new and not in the support library

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            CharSequence name = "channel1";
            String description = "This is a NotificationChannel";
            String CHANNEL_ID="100";
            int importance = NotificationManager.IMPORTANCE_DEFAULT;
            NotificationChannel channel = new NotificationChannel(CHANNEL_ID, name, importance);
            channel.setDescription(description);
            // Register the channel with the system; you can't change the importance
            // or other notification behaviors after this
            NotificationManager notificationManager = getSystemService(NotificationManager.class);
            notificationManager.createNotificationChannel(channel);
            return CHANNEL_ID;
        }
        return null;
    }
```
已创建的通知渠道NotificationManager.createNotificationChannel(NotificationChannel_INSTANCE)不会执行任何操作，所以重复调用此方法是安全的

创建通知并发送：

```java
    NotificationManagerCompat notificationManager = NotificationManagerCompat.from(this);
        NotificationCompat.Builder builder = new NotificationCompat.Builder(this, createNotificationChannel())
                .setContentTitle("My notification")
                .setSmallIcon(R.drawable.ic_launcher_background)
                .setContentText("Much longer text that cannot fit one line...")
                .setStyle(new NotificationCompat.BigTextStyle()
                        .bigText("Much longer text that cannot fit one line..."))
                .setPriority(NotificationCompat.PRIORITY_DEFAULT);
        // notificationId is a unique int for each notification that you must define
        notificationManager.notify(10, builder.build());
```
在android8.0以下的设备中，setPriority()，设置优先级，决定了Norification的侵入程度，而在android8.0以上要设置通知渠道(NotificationChannel)的重要性

优先级与重要性见常量值[android官方文档](https://developer.android.google.cn/training/notify-user/channels.html?hl=zh-cn#importance)