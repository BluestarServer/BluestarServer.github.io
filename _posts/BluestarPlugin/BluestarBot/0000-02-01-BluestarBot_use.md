---
title: 服主手册
date: 2022-05-05 10:00:00 +0800
categories: []
tags: []
author: Lanzhi
math: true
---

> 此插件要求Java版本不低于$Java9$

{: .prompt-tip }

## 使用方法

### 安装插件:

- 下载此[插件](https://github.com/lanzhi6/BluestarBot/releases/latest)
- 放入`plugins`文件夹
- `重启服务器`或`reload`或使用`plugman`

> 因为插件的加载原因,此插件可以热加载,但是热卸载后可能出错。不应该尝试热卸载/重载

{: .prompt-warning }

### 一些使用方法:

就像在索引中所说,插件本身提供的直接使用的功能不多,在此进行列举

- 绑定功能: 绑定功能允许绑定我的世界内的玩家和QQ账号,从而方便进行服务器和QQ的互动。具体用途取决于依赖此功能的插件，本身没有其他功能。

  使用`/bluestarbot addbind <Name> <QQId>`可以手动添加绑定

  使用`/bluestarbot bind`可以打开绑定管理,能查看已有的所有绑定,还可以进行删除操作

- 登录机器人:

  手动登录: `/bluestarbot login <qqid> <password>`

  退出登录: `/bluestarbot logout <qqid>`

  查看已登录列表: `/bluestarbot list`

  自动登录: 自动登录会在插件加载时自动登录,也可以方便的进行登录

  - 添加自动登录机器人: `/bluestarbot autologin add <qqid> <password>`
  - 移除自动登录机器人: `/bluestarbot autologin remove <qqid>`
  - 查看自动登录机器人列表: `/bluestarbot autologin list`
  - 登录所有自动登录的机器人: `/bluestarbot autologin loginall`

- 发送消息:

  - 发送好友消息: `bluestarbot sendfriend <bot> <friend> <message>`
  - 发送群消息:  `bluestarbot sendgroup <bot> <group> <message>`
  - 发送群临时会话消息: `bluestarbot sendmember <bot> <group> <member> <message>`

## 登录验证

### 滑块验证

滑块是最常见也是最麻烦的验证方式。

首先打开给出的URL链接，已`EDGE`浏览器为例，按下`F12`键打开开发者工具,在最上面一栏选中网络

![](BluestarBot_F12.png)

之后完成滑块验证,按照下图所示选择后会看到`ticket`一栏。图片中`ticket`是空的，正常是有值的，将值复制（不包括双引号）。使用`/bluestarbot verify <机器人Id> <ticket>`完成验证

![](BluestarBot_ticket.png)

### 图片验证码

图片验证码会被存入`plugins/BluestarBot/verify-images/机器人id-verify.png`文件,需要服主打开文件查看。然后输入`/bluestarbot verify <机器人Id> <验证码>`进行验证

### 短信验证码

短信验证码需要使用对应的手机号查看验证码,然后使用`/bluestarbot verify <机器人Id> <验证码>`进行验证。

说明：此功能目前未经测试，若有问题请尽快报告

### 设备锁

设备锁验证会给出一个URL链接，打开链接后按提示扫描完成确认。然后输入`/bluestarbot verify <机器人Id>`完成认证