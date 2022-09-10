---
title: "Attribute Properties"
keywords: dataattributes data attributes 1.18.1 attribute properties
tags: [data_attributes, 1.18.1]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.18.1_attribute_properties.html
summary: "Data Attributes introduces two entirely new ideas to Minecraft's entity attributes: functions and properties. This section covers attribute properties by presenting a series of examples."
---

{% include important.html content="This section is now outdated and unsupported. Click [here](dataattributes-1.18.2_home) for the latest release." %}

## Overview

Apart from attribute functions, the second thing added added to entity attributes are attribute properties. These are, in this context, simply extra bits of information appended to entity attributes. They are structured as a map; where both key and value are Strings.

## Use Cases

This feature offers little use for pack creators, aside from editing mods attribute's properties. For mod developers however, this has the potential for more use. Perhaps you are creating a Trinkets addon, and you want to have your trinkets provide attribute bonuses like +30% Damage or +4 Max Health. You need a rarity system for attributes, and rather than creating one from scratch, you can use attribute properties as the framework for this application.

This also serves me personally as a *"Oh no I forgot to add this"* feature. It allows me to just add a datapack to patch future mods. Finally, this serves as a configuration feature: by using datapacks, any mod built with Data Attributes can be configured without the need for funky configuration magic - and this configuration can be solely server side as this is powered by datapacks.

## Example 1

Say we have our examplemod, and we want ot assign a `rarity` value to each attribute that would range from 0.0 to 1.0. We go to the directory `data/examplemod/attributes/` and create the json file `properties.json`:

{% highlight json %}

{
    "values": {
        "minecraft:generic.max_health": {
            "rarity": "0.5"
        },
        "minecraft:generic.armor": {
            "rarity": "0.6"
        },
        ...
    }
}

{% endhighlight %}

Where `...` would be the rest of the attributes. Here we are adding the `rarity` property to Max Health and Armor and assigning each property a relevant value. Note how the value is a string: this is so that floating point numbers can be parsed without a loss in precision, but also so that other data can be appended to attributes - not just numbers.

## Example 2

In this example we just want to assign a `special` boolean to a custom attribute `examplemod:magic`. If an attribute has teh special property we can say it is true, if the attribute does not have the special property we can say it is false. Since it is just a case of *having* the property or not, the value doesn't matter and can just be an empty string:

{% highlight json %}

{
    "values": {
        "examplemod:magic": {
            "special": ""
        }
    }
}

{% endhighlight %}

{% include note.html content="Attribute Properties are immutable. They can only be accessed and checked for." %}

{% include links.html %}
