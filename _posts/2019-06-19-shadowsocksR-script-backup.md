---
layout: post
title: shadowsocksR(秋水逸冰)backup
date: 2019-06-19 00:00:00 +0800
categories: note
tag: shadowsocksR
---
秋水逸冰--shadowsocksR早期的一键脚本开发者，已宣布结束该脚本的开发

[https://teddysun.com/548.html](https://teddysun.com/548.html)

以下是该大神脚本的备份：

本脚本适用环境：

系统支持：CentOS，Debian，Ubuntu

内存要求：≥128M

关于本脚本：

一键安装 ShadowsocksR 服务端。

请下载与之配套的客户端程序来连接。

（以下客户端只有 Windows 客户端和 Python 版客户端可以使用 SSR 新特性，其他原版客户端只能以兼容的方式连接 SSR 服务器）

默认配置：

服务器端口：自己设定（如不设定，默认为 8989）

密码：自己设定（如不设定，默认为 teddysun.com）

加密方式：自己设定（如不设定，默认为 aes-256-cfb）

协议（Protocol）：自己设定（如不设定，默认为 origin）

混淆（obfs）：自己设定（如不设定，默认为 plain）

使用方法：

使用root用户登录，运行以下命令：

```shell
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2>&1 | tee shadowsocksR.log
```

卸载方法：

使用 root 用户登录，运行以下命令：

```shell
./shadowsocksR.sh uninstall
```

安装完成后即已后台启动 ShadowsocksR ，运行：

```shell
/etc/init.d/shadowsocks status
```

可以查看 ShadowsocksR 进程是否已经启动。

本脚本安装完成后，已将 ShadowsocksR 自动加入开机自启动。

使用命令：

启动：

```shell
/etc/init.d/shadowsocks start
```

停止：

```shell
/etc/init.d/shadowsocks stop
```

重启：

```shell
/etc/init.d/shadowsocks restart
```

状态：

```shell
/etc/init.d/shadowsocks status
```

配置文件路径：

```shell
/etc/shadowsocks.json
```

日志文件路径：

```shell
/var/log/shadowsocks.log
```

代码安装目录：

```shell
/usr/local/shadowsocks
```
多用户配置示例：

```shell
{
"server":"0.0.0.0",
"server_ipv6": "[::]",
"local_address":"127.0.0.1",
"local_port":1080,
"port_password":{
    "8989":"password1",
    "8990":"password2"，
    "8991":"password3"
},
"timeout":300,
"method":"aes-256-cfb",
"protocol": "origin",
"protocol_param": "",
"obfs": "plain",
"obfs_param": "",
"redirect": "",
"dns_ipv6": false,
"fast_open": false,
"workers": 1
}
```

SSR协议和混淆插件说明：

工作原理

C->S方向

浏览器请求（socks5协议） -> ssr客户端 -> 协议插件（转为指定协议） -> 加密 -> 混淆插件（转为表面上看起来像http/tls） -> ssr服务端 -> 混淆插件（分离出加密数据） -> 解密 -> 协议插件（转为原协议） -> 转发目标服务器

其中，协议插件主要用于增加数据完整性校验，增强安全性，包长度混淆等，协议插件主要用于伪装为其它协议

客户端

客户端的协议插件暂无配置参数，混淆插件有配置参数，混淆插件列表如下：

plain：不混淆，无参数

http_simple：简易伪装为http get请求，参数为要伪装的域名，如cloudfront.com。仅在C#版客户端上支持用逗号分隔多个域名如a.com,b.net,c.org，连接时会随机使用其中之一。不填写参数时，会使用此节点配置的服务器地址作为参数。

http_post：与http_simple绝大部分相同，区别是使用POST方式发送数据，符合http规范，欺骗性更好，但只有POST请求这种行为容易被统计分析出异常。参数配置与http_simple一样

tls1.2_ticket_auth：伪装为tls请求。参数配置与http_simple一样

其它插件不推荐使用，在这里忽略

客户端的协议插件，仅建议使用origin,verify_sha1,auth_sha1_v2,auth_sha1_v4,auth_aes128_md5,auth_aes128_sha1，解释如下：

origin：原版协议，为了兼容

verify_sha1：原版OTA协议，为了兼容

auth_sha1_v2：中等安全性，无时间校对的要求，适合路由器或树莓派，混淆强度大

auth_sha1_v4：较高安全性，有宽松的时间校对要求，混淆强度大

auth_aes128_md5或auth_aes128_sha1：最高安全性，有宽松的时间校对要求，计算量相对高一些，混淆强度较大

如不考虑兼容，可无视前两个

服务端

大部分插件都可以通过添加_compatible后缀以表示兼容原版，例如默认的http_simple_compatible或auth_sha1_v4_compatible这样；

服务端的协议插件，仅auth_*系列有协议参数，其值为整数。表示允许的同时在线客户端数量，建议最小值为2。默认值64；

服务端的混淆插件，http_simple或http_post有混淆参数，用逗号分开若干个host，表示客户端仅能使用以上任一个host连接，而留空表示客户端可以使用任意host。tls1.2_ticket_auth有混淆参数，其值为整数，表示与客户端之间允许的UTC时间差，单位为秒，为0或不填写（默认）表示无视时间差。

总结

如不考虑原版的情况下，推荐使用的协议，只有auth_sha1_v4和auth_aes128_md5和auth_aes128_sha1，推荐使用的混淆只有plain,http_simple,http_post,tls1.2_ticket_auth不要奇怪为什么推荐plain，因为混淆不总是有效果，要看各地区的策略的，有时候不混淆让其看起来像随机数据更好。

