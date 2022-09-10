---
title: "Attribute Functions"
keywords: dataattributes data attributes 1.17.1 attribute functions
tags: [data_attributes, 1.17.1]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.17.1_attribute_functions.html
summary: "Data Attributes introduces two entirely new ideas to Minecraft's entity attributes: functions and properties. This section covers attribute functions by presenting a series of examples."
---

{% include important.html content="This section is now outdated and unsupported. Click [here](dataattributes-1.18.2_home) for the latest release." %}

## What and Why

What is an attribute function? Why has Data Attributes implemented them? 

The premise for these questions resides in a commonly found aspect of RPGs. Often, their attribute systems are structured with a few primary attributes (think Constitution, Strength, Dexterity for example) and then secondary attributes (think Max Health, Damage, Evasion for example). Increasing the value of a primary attribute increases the value of secondary attributes as well. 

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

The solution is to append references to the secondary attributes to the primary attribute itself. An attribute is simply a small data-object that acts as a key. We simply expand the amount of data this object holds to include: a collection of the attributes that should be changed when it changes; how each attribute should change; and the amount each attribute should change. This data is stored in a map, and is dubbed an Attribute Function. Moreover, configurability is ensured by exposing Attribute Functions as json resources in datapacks.

## Structure

There are three items to a function:

- `attribute` refers to the registry key of the attribute that will be added to.
- `behaviour` refers to *how* the attribute will be modified (this can currently either be `FLAT` or `DIMINISHING`). This is a relatively big topic, and supplements the dedicated [Adding Behaviour](dataattributes-1.17.1_adding_behaviour.html) section. 
- `multiplier` refers to a multiplier applied to a modifier's value before it is applied. 

## Behaviour

The `DIMINISHING` type allows us to specify an addition type with *diminishing returns*. This type of addition is found in other games with advanced attribute systems: in Dota 2, Evasion is an attribute with a diminishing stacking bonus. If I have an entity with +20% evasion and the entity has an item that provides +30% evasion, one could expect a final value of +50% evasion - but this is wrong. The final value is actually about +38% evasion. It should be noted that this is assuming a max value of 100% evasion (a limit). 

## How it Works

There are some things that should be noted about attribute functions:

- *"added to"* refers to any change in the *value* of said attribute.
- *"attribute"* in this context refers to *attribute instances*, which contains the current and base value of their attribute.
- Therefore, a change in the value of an attribute instance implies the addition or removal of an entity attribute modifier:
  - Entity attribute modifiers are an object containing a UUID, a name, a value and an operation (ADDITION, MULTIPLY_BADE or MULTIPLY_TOTAL).

Previously, we talked about how one might have the Consitution attribute, and increasing it would also increase the Max Health attribute. This sounds fine, but it is not necessarily intuitive, and involves some behind-the-scenes action.

Lets say an attribute modifier of the ADDITION type with a value of 4.0 is applied to Constitution. An attribute modifier of the ADDITION type with a value of 4.0 will also be applied to Max Health - furthermore, it will have the same UUID and name. Because it has the same UUID, modders should think carefully about how they apply their attribute modifiers (i.e. use different UUID's for different attribute instances); just because modifiers carry a unique id, that does not mean that they are unique *globally*, it only means that they are unique *locally* to a given attribute instance.

So far so good. An entity exists with 4 Constitution. What if we apply another modifier? This modifier will have another UUID, name, a value of 0.5 and be of the MULTIPLY_TOTAL type. Once applied, our entity will have a Consitution of 6. How does this affect our Max Health?

You could be forgiven for thinking that our Max Health is also increased by 50%. Instead, the following happens:

A new attribute modifier is created, containing the same UUID, the same name, but it has a value of 2 and is of the ADDITION type. This is applied to our MAX HEALTH. Note that the *value* of the attribute modifier is equal to the *difference* in original and current values of Constitution (from 4 to 6, giving a difference of 2). This difference is *added* to Max Health, and a similar order of events occurs for MULTIPLY_BASE.

The important points to take away from this wall of text are:

- Attribute modifiers resulting from attribute functions are *always* of the ADDITION type (this means that +30% Constitution would not necessarily mean +30% Max Health).
- Upon the removal or modifiers, all modifiers applied as a result of attribute functions are removed as well.
- Modifiers are *slightly* more dynamic; referencing the above scenario, if we increased our value of Constitution from 4 to 8, our +50% would increase the amount added to 4, giving us a total of 12. Likewise, this would increase the amount added to Max Health (from +2 to +4).

## Json Example

{% highlight json %}

{
    "values": {
        "examplemod:constitution": [
            {
                "attribute": "minecraft:generic.max_health",
                "behaviour": "FLAT",
                "multiplier": 1.0
            },
            {
                "attribute": "minecraft:generic.armor_toughness",
                "behaviour": "FLAT",
                "multiplier": 0.25
            }
        ]
    }
}

{% endhighlight %}

That's all that is needed; note how an attribute (in this case Constitution) can have any number of functions attached - however, each function must be unique in that you cannot have two functions with the same `attribute` tag.

## Recursion

The keen eye among you may have noticed that there is an opportunity for recursion/infinite attributes. Lets show how this might be done:

{% highlight json %}

{
    "values": {
        "examplemod:constitution": [
            {
                "attribute": "minecraft:generic.max_health",
                "behaviour": "FLAT",
                "multiplier": 1.0
            }
        ],
        "minecraft:generic.max_health": [
            {
                "attribute": "examplemod:constitution",
                "behaviour": "FLAT",
                "multiplier": 1.0
            }
        ]
    }
}

{% endhighlight %}

Looking at our `functions.json` file, we can see that when we add a point to Constitution, a point is added to Max Health; but when a point is added to Max Health, a point is added to Constitution - *it would seem we have a vicious cycle on our hands*. One could even see how a huge chain of attributes could accidentally become recursive. Constitution adds to Max Health adds to Strength adds to Armor adds to Knockback Resistance adds to Constitution... Note to mention that since each attribute can have many functions attached, the tree of possible combinations resulting in recursion can be extensive.

Therefore, Data Attributes implements protections against this. When the `functions.json` files are read and assembled, the functions tree is scanned for loops. Any loops found are broken by removing the first function found that results in a loop. The specific function that would be removed for a specific set of circumstances is unreliable, just that it is impossible for loops to form. This is done so that checks do not have to be made *in game*, which could cause lag.

That being said, developers and pack creators alike should still be careful not to create loops.

{% include links.html %}
