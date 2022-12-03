---
title: 使用Use
date: 2022-05-05 10:00:00 +0800
categories: [BluestarPlugin,BluestarCraft]
tags: []
author: Lanzhi
math: true
---

使用插件注册合成表拥有极强的拓展和更多功能!如有需要请查阅[开发者Developer](https://www.bluestarmc.top/posts/BluestarCraft_dev/)

Use plugin to register recipe has strong expansion and more functions!If necessary, please refer to[开发者Developer](https://www.bluestarmc.top/posts/BluestarCraft_dev/)

# 打开高级工作台Open crafting table
 1.制作高级工作台(4个钻石一个工作台)

 1.Make a advanced crafting table(Four diamonds and one crafting table)

  ![高级工作台](https://user-images.githubusercontent.com/90564167/169694365-e72b5f19-9288-47c7-b20a-e960bebc9388.png)

 2.用高级工作台点击工作台,即可打开高级工作台界面

 2.click crafting tables whith the advanced crafting table to open the advanced crafting table GUI

  ![工作台GUI](https://user-images.githubusercontent.com/90564167/169694661-9025859d-e52a-44a8-b14e-92c2f19dd183.gif)

 3.像普通工作台那样合成

 3.Use it like common crafting table

# 配置文件内注册合成表Register in configuration
 打开```plugins\BluestarCraft\recipe.yml```
 Open```plugins\BluestarCraft\recipe.yml```
```yaml
#这是默认的文件This is the default
shaped: #有序的合成表shaped recipe
  recipe1: #合成表名称,不能重名Recipe's name,cannot duplicate name
    shape: #5*5的合成表,可以不满5行 5*5recipe,can have less than 5
      - '  a'
      - ' aaa'
      - 'aaaaa'
      - ' aaa'
      - '  a'
    result: 'minecraft:diamond_block' #合成结果 The result
    amount: 2 #结果的数量 the amount of result
    ingredient:
      a: 'minecraft:diamond' #合成表中的物品解释 Item of character in result
shapeless: #无序的 shapeless recipe
  recipe2: #名字name
    ingredient: #所需物品和数量 Needed item and amount
      diamond: 20
    result: 'minecraft:diamond_block' #合成结果 The result
    amount: 5 #结果的数量 the amount of result
  abcd: .....
  dcba: .....
```
# 游戏内注册Register in game
在游戏中使用`/bscraft register name shaped/shapeless material/exact `

Use `/bscraft register name shaped/shapeless material/exact ` in game

说明: ```name```是合成表名称.```shaped```为有序,```shapless```为无序.匹配模式,```material```为仅匹配类型,```exact```要求nbt匹配(```exact```仅能用于有序合成,原因请查阅问题一栏),在后面加上```toBukkit```且配方大小小于等于3*3配方将被同时注册到原版(原版工作台),要求版本大于1.14

explain: ```name``` is the recipe name.```shaped```means shaped recipe,```shapeless```means shapeless recipe.matcher mode```material```means only match items type,```exact```Also includes NBT(shapeless not support ```exact```,You can see the ```problem```),add  ```tobukkit``` after it, and the Recipe with a size less than or equal to 3*3 will be registered to the vanilla (vanilla workbench) at the same time!(Minimum version 1.14)

之后,会打开GUI

After that, the GUI opens

![)WCW4_RB72~872FS5O 02_8](https://user-images.githubusercontent.com/90564167/169697294-73e12977-5885-498e-98ec-11065897595a.png)

在左边摆放对应的合成表,右边放结果,左边的数量无所谓,右边的数量将会是合成出的结果的数量.之后点击绿色玻璃板完成注册,如果你直接关闭,什么都不会发生.

Put the recipe on the left and the results on the right. The number on the left doesn't matter. The number on the right will be the number of results Then click on the green glass plate. If you close it directly, nothing will happen