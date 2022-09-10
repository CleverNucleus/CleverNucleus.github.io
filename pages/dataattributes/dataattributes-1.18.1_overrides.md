---
title: "Overrides"
keywords: dataattributes data attributes 1.18.1 overrides
tags: [data_attributes, 1.18.1]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.18.1_overrides.html
summary: Json overrides can be used to either override pre-existing attributes, either vanilla or modded, or if non-existant a new attribute is created. Therefore, new entity attributes can be created using just json files.
---

{% include important.html content="This section is now outdated and unsupported. Click [here](dataattributes-1.18.2_home) for the latest release." %}

## Overview

Entity attributes have four data points that can be override using json files; see the following:

- Minimum Value `minValue`: the minimum value that this attribute's instance is clamped to.
- Maximum Value `maxValue`: the maixmum value that this attribute's instance is clamped to.
- Fallback Value `defaultValue`: the value that, should something go wrong, this attribute's instance defers to (this almost never has any actual impact in-game, other than setting initial values if they are not otherwise provided).
- Translation Key `translationKey`: the key that gets the display name of the attribute.
- Stacking Behaviour `stackingBehaviour`: determines how different sources of this attribute's value add together (either `FLAT` or `DIMINISHING`).'

### 1. Example 1

Minecraft's armor attribute has the following:

- Default Value is 0.0.
- Minimum Value is 0.0.
- Maximum Value is 30.0.
- Translation Key is attribute.name.generic.armor.

Say we want to change the maximum armor value to 100.0. We need to know the attribute's registry key, which is comprised of a `namespace` and a `path`. In this case, the registry key is `minecraft:generic.armor`. In the directory `data/minecraft/attributes/overrides` we create the file `generic.armor.json`. Note how the registry key's namespace matches our directory's namespace, and the registry key's path matches our file name.

We add the following to `generic.armor.json`:

{% highlight json %}

{
    "defaultValue": 0.0,
    "minValue": 0.0,
    "maxValue": 100.0,
    "translationKey": "attribute.name.generic.armor",
    "stackingBehaviour": "FLAT"
}

{% endhighlight %}

That's it. By using a datapack containing `data/minecraft/attributes/overrides/generic.armor.json`, the maximum value for armor is 100. 

### 2. Example 2

Similarly to example 1, the same methodology can be used to create entirely new attributes. Say we have a mod with modid `examplemod`, and we want to add the attribute `max_mana`. We create the directory `data/examplemod/attributes/overrides/` and create the file `max_mana.json`. Inside the json file add the following:

{% highlight json %}

{
    "defaultValue": 0.0,
    "minValue": 0.0,
    "maxValue": 1000.0,
    "translationKey": "examplemod.attribute.name.max_mana",
    "stackingBehaviour": "FLAT"
}

{% endhighlight %}

We now have an attribute registered to the game. In our language file we can add the translation key entry like so: 

{% highlight json %}

"examplemod.attribute.name.max_mana": "Max Mana"

{% endhighlight %}

Although our attribute is registered to the game, it won't be present on any living entity yet. The next step is to attach it to an entity's attribute container - see [Entity Types](dataattributes-1.18.1_entity_types).

{% include links.html %}
