---
title: 问题Problem
date: 2022-05-05 10:00:00 +0800
categories: [BluestarPlugin,BluestarCraft]
tags: []
author: Lanzhi
math: true
---

Q: 
 - 为什么删除我删除配方后,配方又出现了? 
 - Why does the recipe appear again after I delete it?

A:
 - 如果你的配方是在```recipe.yml```中的话,需要在文件中删除,否则在加载插件的时候会重新加载
 - If your recipe is in ```recipe.yml```,it needs to be deleted from the file,otherwise it will be reloaded when loading the plugin

Q:
 - 为什么无序配方不能支持ExactMatcher(匹配nbt)? 
 - Why can't shapeless recipes support exactmatcher (match NBT)?

A:
 - 不是我不想,有序配方只需要判断对应的格子是否匹配,而无序配方如果要统计每种物品的数量很容易,如果要用匹配的话有$25!$种组合,这不可能实现
 - It's not that I don't want to.The shaped recipe only needs matcher to match item what is on the mather, while the shapeless recipe is easy to count the number of each item type.If you want to use matcher to match, there are 25 factorial combinations, which is impossible to achieve