---
title: "Attribute Functions"
keywords: dataattributes data attributes 1.18.1 attribute functions
tags: [data_attributes, 1.18.1]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.18.1_attribute_functions.html
summary: "Data Attributes introduces two entirely new ideas to Minecraft's entity attributes: functions and properties. This section covers attribute functions by presenting a series of examples."
---

{% include important.html content="This section is now outdated and unsupported. Click [here](dataattributes-1.18.2_home) for the latest release." %}

## Overview

Attribute functions allow you to relate attributes to each other in a way that is common in other games: Dota 2 has the primary attributes Strength, Agility and Intelligence - where Strength increases the health pool, Agility increases armor and Intelligence increases Mana. PoE is much the same, where Strength increases maximum life, Dexterity grants Evasion and Intelligence also increases Mana.

The implementation of attribute finctions is out of the scope of this wiki, but basically each attribute now has a collection of children and parent attributes. When defining an attribute function, you define an entity attribute (the parent) and assign to it a list of other attributes (children). The resulting behaviour in-game is that whenever the value of a parent attribute changes, the value of all its children changes as well.

{% include note.html content="This is one-directional, a change in the value of a child does not impact the value of its parents." %}

## Why do we need Attribute Functions

The premise for this questions resides in a commonly found aspect of RPGs. Often, their attribute systems are structured with a few primary attributes (think Constitution, Strength, Dexterity for example) and then secondary attributes (think Max Health, Damage, Evasion for example). Increasing the value of a primary attribute increases the value of secondary attributes as well. 

### 1. The Problem

Lets say we have a Minecraft mod that provides this RPG-type attribute system, and there is a nice screen with buttons that let you skill/increase those primary attributes. In code, we may have a method similar to `Button#addAttribute`; we would pass the attribute to increase and the amount to increase it by as arguments:

{% highlight java %}

void onButtonClick(Button button) {
    button.addAttribute(Attributes.CONSTITUTION, 1.0);
}

{% endhighlight %}

This is actually fine so far; there is nothing wrong with doing it this way. However, things get cumbersome when we implement follow-on attributes. Say for every one point in Constitution, we increase our Max Health by one, Knockback Resistance by 0.01 and Damage by 0.5:

{% highlight java %}

void onButtonClick(Button button) {
    button.addAttribute(Attributes.CONSTITUTION, 1.0);
    button.addAttribute(Attributes.MAX_HEALTH, 1.0);
    button.addAttribute(Attributes.KNOCKBACK_RESISTANCE, 0.01);
    button.addAttribute(Attributes.DAMAGE, 0.5);
}

{% endhighlight %}

This is not great. Furthermore, what happens when someone uses the `/attribute` command to increase their Constitution? Or equips an item that increases their Constitution? Or what happens when a player wants to customise what attributes are changed when Constitution is changed? What about configurability? We have to create unwieldy repetitons everywhere any kind of attribute modifying code occurs. It is not feasible to do this.

### 2. The Solution

The solution is described briefly in the [Overview](#overview): we denominate our attributes as either the parent or child; to the parent, we attach a collection of references to its children; and to the child, we attach a collection of references to its parents.

{% include note.html content="Denominations of child and parent are not mutually exclusive. A parent attribute can be a child to another attribute, just as a child attribute can be a parent to another attribute." %}

## Examples

The concept of attribute functions is best explained using examples and scenarios:

### 1. Example 1

Say the player has the attribute Constitution; and for every point added in this attribute, a point is also added to Max Health. This would be done using an attribute function. Functions allow for attributes to influence each other.

### 2. Example 2

Using [Example 1](#1-example-1) as a reference, this example shows how we might implement it. Say we have our examplemod, and we've added the attribute Constitution, but now we want to make it so that for every point added to Constituion, a point is added to Max Health and half a point is added to Armor.

We go to the directory `data/examplemod/attributes/` where we create the json file `functions.json`. Inside this file, we add the following:

{% highlight json %}

{
    "values": {
        "examplemod:constitution": {
            "minecraft:generic.max_health": 1.0,
            "minecraft:generic.armor": 0.5
        }
    }
}

{% endhighlight %}

That's all that is needed. Now whenever a point is added to Constitution, a point is also added to Max Health and half a point is added to Armor. Note that the value assigned is a multiplier. If we added `+5` to Constitution, we would add `+(5 * 1.0)=5` to Max Health and `+(5 * 0.5)=2.5` to Armor.

## Differences between 1.18.1 and 1.17.1

Attribute functions are implemented differently in 1.18.1 and 1.17.1. In 1.17.1 when a modifier was applied to a parent, the same modifier would be applied to the children. Now in 1.18.1 when a modifier is applied to the parent, only the change in value is carried over to the children, meaning that you can apply the same modifier to both parents and children, whereas before you could not.

## Recursion

The keen eye among you may have notices that there is an opportunity for recursion/infinite attributes. Lets show how this might be done:

{% highlight json %}

{
    "values": {
        "examplemod:constitution": {
            "minecraft:generic.max_health": 1.0
        },
        "minecraft:generic.max_health": {
            "examplemod:constitution": 1.0
        }
    }
}

{% endhighlight %}

Looking at our `functions.json` file, we can see that when we add a point to Constitution, a point is added to Max Health; but when a point is added to Max Health, a point is added to Constitution - *it would seem we have a vicious cycle on our hands*. One could even see how a huge chain of attributes could accidentally become recursive. Constitution adds to Max Health adds to Strength adds to Armor adds to Knockback Resistance adds to Constitution… Not to mention that since each attribute can have many functions attached, the tree of possible combinations resulting in recursion can be extensive.

Therefore, Data Attributes implements protections against this. When the `functions.json` files are read and assembled, the functions tree is scanned for loops. Any loops found are broken by removing the first function found that results in a loop. The specific function that would be removed for a specific set of circumstances is unreliable, just that it is impossible for loops to form. This is done so that checks do not have to be made *in game*, which could cause lag.

That being said, developers and pack creators alike should still be careful not to create loops.

{% include links.html %}
