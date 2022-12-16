---
title: 开发者手册
date: 2022-12-16 10:00:00 +0800
categories: []
tags: []
author: Lanzhi
math: true
---

## 添加依赖

### Maven

```xml
<repositories>        
    <repository>
        <id>mvn-repo</id>
        <url>https://raw.githubusercontent.com/lanzhi6/maven-repository/main</url>
    </repository>
</repositories>
```

```xml
        <dependency>
            <groupId>me.lanzhi</groupId>
            <artifactId>BluestarBot</artifactId>
            <version>1.4.0</version>
            <scope>provided</scope>
        </dependency>
```

### Gradle

```cmake
allprojects 
{
    repositories
    {
    	maven { url 'https://raw.githubusercontent.com/lanzhi6/maven-repository/main' }
    }
}
```

```cmake
dependencies 
{
    implementation 'me.lanzhi:BluestarBot:version'
}
```

## 功能说明

#### 查看[JavaDoc](https://bluestarbot.lanzhi.me/javadoc)

插件的所有类都在[me.lanzhi.bluestarbot](https://bluestarbot.lanzhi.me/javadoc/me/lanzhi/bluestarbot/package-summary.html)包内,其中[me.lanzhi.bluestarbot.api](https://bluestarbot.lanzhi.me/javadoc/me/lanzhi/bluestarbot/api/package-summary.html)内的部分是为开发者提供的,其他部分属于内部实现.

创建、获取机器人，进行Minecraft-QQ的绑定等功能通过[BluestarBot](https://bluestarbot.lanzhi.me/javadoc/me/lanzhi/bluestarbot/api/BluestarBot.html)类进行处理

联系人相关的类请查看[me.lanzhi.bluestarbot.api.contact](https://bluestarbot.lanzhi.me/javadoc/me/lanzhi/bluestarbot/api/contact/package-summary.html)包

消息处理相关内容请查看[me.lanzhi.bluestarbot.api.event.message](https://bluestarbot.lanzhi.me/javadoc/me/lanzhi/bluestarbot/api/event/message/package-summary.html)

监听QQ内的事件请查看[me.lanzhi.bluestarbot.api.event](https://bluestarbot.lanzhi.me/javadoc/me/lanzhi/bluestarbot/api/event/package-summary.html)

> 所有事件都是[BluestarBotEvent](https://bluestarbot.lanzhi.me/javadoc/me/lanzhi/bluestarbot/api/event/BluestarBotEvent.html)的子类
{: .prompt-tip }

> 注意: 事件包中的接口事件,例如[被动接收消息事件MessageReceivedEvent](https://bluestarbot.lanzhi.me/javadoc/me/lanzhi/bluestarbot/api/event/MessageReceivedEvent.html),这些事件是接口,因为`Bukkit`的事件系统的原因,接口无法被监听.所以需要直接监听[BluestarBotEvent](https://bluestarbot.lanzhi.me/javadoc/me/lanzhi/bluestarbot/api/event/BluestarBotEvent.html)
>
> 示例: 监听所有被动接收消息事件并广播
>
> ```java
> public final class TestListener implements Listener
> {
>     @EventHandler
>     public void onMessage(BluestarBotEvent event)
>     {
>         if (!(event instanceof MessageReceivedEvent))
>         {
>             return;
>         }
>         MessageReceivedEvent messageEvent=(MessageReceivedEvent)event;
>         Bukkit.getServer().broadcastMessage(messageEvent.getMessageAsString());
>     }
> }
> ```
{: .prompt-warning }

### 一些代码示例

让id是`12345`的机器人向id是`54321`的qq群发送`Hello world`

```java
Bot bot=BluestarBot.getBot(12345);
Group group=bot.getGroup(54321);
group.sendMessage("Hello world");
```

让id是`12345`的机器人向id是`54321`的好友发送`Hello world`

```java
Bot bot=BluestarBot.getBot(12345);
Friend friend=bot.getFriend(54321);
friend.sendMessage("Hello world");
```

让id是`12345`的机器人向它所有好友发送`Hello`

```java
        for (Friend friend:BluestarBot.getBot(12345).getFriends())
        {
            friend.sendMessage("Hello");
        }
```

当好友发送了`你好`之后回复一个`你好`

```java
    @EventHandler
    public void onFriendMessage(FriendMessageEvent event)
    {
        if (event.getMessageAsString().equals("你好"))
        {
            event.reply("你好");
        }
    }
```

其他的自己探索吧,哈哈哈