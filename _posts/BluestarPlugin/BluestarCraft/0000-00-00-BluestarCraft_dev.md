---
title: 开发者Developer
date: 2022-05-05 10:00:00 +0800
categories: [BluestarPlugin,BluestarCraft]
tags: []
author: Lanzhi
math: true
---



# 依赖depend

[![](https://jitpack.io/v/lanzhi6/BluestarCraft.svg)](https://jitpack.io/#lanzhi6/BluestarCraft)

```xml
<repositories>
	<repository>
		<id>jitpack.io</id>
		<url>https://jitpack.io</url>
	</repository>
</repositories>
<dependency>
	<groupId>com.github.lanzhi6</groupId>
	<artifactId>BluestarCraft</artifactId>
	<version>version</version>
</dependency>
```
```
allprojects 
{
	repositories 
	{
		maven { url 'https://jitpack.io' }
	}
}
dependencies 
{
	implementation 'com.github.lanzhi6:BluestarCraft:version'
}
```

请仅使用这里说明的类和方法,使用其他的类和方法可能造成错误
​
Please only use the classes and methods described here. Using other classes and methods may cause errors

```java
package me.lanzhi.bluestarcraft.api;

import me.lanzhi.bluestarcraft.api.recipe.Recipe;
import org.bukkit.entity.Player;
import org.jetbrains.annotations.NotNull;
import org.jetbrains.annotations.Nullable;

import java.util.List;

public final class BluestarCraft
{
    // 所有的Recipe都是me.lanzhi.bluestarcraft.api.recipe.Recipe而不是Bukkit中的
    // All the Recipe is me.lanzhi.bluestarcraft.api.recipe.Recipe,Not Recipe in Bukkit

    public static void addRecipe(@NotNull Recipe recipe);//注册一个合成Register recipe
    public static boolean containsRecipe(@Nullable Recipe recipe);
    public static boolean containsRecipe(@Nullable String name);
    public static boolean removeRecipe(@NotNull Recipe recipe);
    public static Recipe removeRecipe(@NotNull String name);
    @NotNull
    public static List<Recipe> getRecipes();//所有合成all the recipe
    @NotNull
    public static List<String> getRecipeNames();//所有合成的名字all the recipe's name
    public static void openCraftGui(@NotNull Player player);//打开合成界面open crafting gui
}
```
```java
package me.lanzhi.bluestarcraft.api.recipe;

import org.bukkit.inventory.ItemStack;
import org.jetbrains.annotations.NotNull;
//合成表,也就是GUI中左侧5*5的区域
//5*5 area on the left in Gui
public final class CraftInventory implements Cloneable
{
    /*y 0  1  2  3  4 
    x   
    0   0  1  2  3  4
    1   5  6  7  8  9
    2   10 11 12 13 14
    3   15 16 17 18 19
    4   20 21 22 23 24*/
    @NotNull
    public ItemStack getItem(int index);//0-24

    @NotNull
    public ItemStack getItem(int x,int y);//0-5,0-5

    public ItemStack[] getItems();//按顺序排列的所有物品All items in order

    @Override
    public CraftInventory clone();
}
```
```java
package me.lanzhi.bluestarcraft.api.recipe;

import org.bukkit.configuration.serialization.ConfigurationSerializable;
import org.bukkit.inventory.ItemStack;

public interface Recipe extends ConfigurationSerializable,/*如果你确定你的合成表永远不需要保存,你的实现可以随便写.If you are sure that your recipe will never need to be saved, you can write it as you like*/
                                Cloneable
{
    String getName();//合成表的名字recipe's name

    //合成表是否要保存,如果为true,插件将再卸载时保存,加载插件时重新加载
    //Is this recipe need to save?If it's true,it well save ,and load it when plugin load
    boolean needSave();

    //我的插件只会调用此方法,如果你直接重写这个方法,另外两个方法就无所谓写什么了
    //My plugin will only call this method. If you rewrite this method directly, the other two methods don't matter what to write
    default ItemStack getResult(CraftInventory inventory)
    {
        if (match(inventory))
        {
            return getResult();
        }
        return null;
    }

    boolean match(CraftInventory inventory);

    ItemStack getResult();
}
```
```java
package me.lanzhi.bluestarcraft.api.recipe;
import me.lanzhi.bluestarcraft.api.recipe.matcher.ItemMatcher;
import org.bukkit.Material;
import org.bukkit.inventory.ItemStack;

import java.util.*;

public final class ShapedRecipe implements Recipe
{
    public ShapedRecipe()//不要使用这个!!! Do not use this!!!


    //这和Bukkit的ShapedRecipe很像
    //This is very similar to Bukkit's ShapedRecipe
    public ShapedRecipe(String name,ItemStack result,String... lines);//默认为不保存 default needSave is false

    public ShapedRecipe(String name,ItemStack result,boolean needSave,String... lines);

    public void setIngredient(char s,Material material);

    public void setIngredient(char s,ItemMatcher itemMatcher);


    @Override
    public String getName();

    @Override
    public boolean needSave();

    @Override
    public boolean match(CraftInventory inventory);

    @Override
    public ItemStack getResult();

    @Override
    public ShapedRecipe clone();
}
```
```java
package me.lanzhi.bluestarcraft.api.recipe;

import org.bukkit.Material;
import org.bukkit.inventory.ItemStack;

public final class ShapelessRecipe implements Recipe
{

    public ShapelessRecipe()//不要使用这个!!! Do not use this!!!


    public ShapelessRecipe(String name,ItemStack result)//默认为不保存 default needSave is false

    public ShapelessRecipe(String name,ItemStack result,boolean needSave);

    public void setMaterial(Material material,int cnt);

    //在原有基础上增加Increase on the original basis
    public void addMaterial(Material material,int cnt);

    //在原有基础上减少Reduce on the original basis
    public void removeMaterial(Material material,int cnt);



    @Override
    public String getName();

    @Override
    public boolean needSave();

    @Override
    public boolean match(CraftInventory inventory);

    @Override
    public ItemStack getResult();

    @Override
    public ShapelessRecipe clone();
}
```
```java
package me.lanzhi.bluestarcraft.api.recipe.matcher;

import org.bukkit.configuration.serialization.ConfigurationSerializable;
import org.bukkit.inventory.ItemStack;

public interface ItemMatcher extends ConfigurationSerializable, Cloneable
{
    boolean match(ItemStack itemStack);
}
```
```java
package me.lanzhi.bluestarcraft.api.recipe.matcher;

import org.bukkit.inventory.ItemStack;

//包括NBT Including NBT
public final class ExactMatcher implements ItemMatcher
{
    public ExactMatcher();//不要使用这个!!! Do not use this!!!

    public ExactMatcher(ItemStack... itemStacks);

    @Override
    public boolean match(ItemStack itemStack);

    @Override
    public ExactMatcher clone();
}
```
```java
package me.lanzhi.bluestarcraft.api.recipe.matcher;

import org.bukkit.Material;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

//匹配物品种类 matcher type
public final class MaterialMatcher implements ItemMatcher
{

    public MaterialMatcher();//不要使用这个!!! Do not use this!!!

    public MaterialMatcher(Material... materials);

    @Override
    public boolean match(ItemStack itemStack);

    @Override
    public MaterialMatcher clone();
}
```