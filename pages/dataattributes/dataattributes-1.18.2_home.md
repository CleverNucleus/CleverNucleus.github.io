---
title: "Home"
keywords: home homepage dataattributes data attributes 1.18.2 1.19 1.19.1 1.19.2
tags: [introduction, getting_started, data_attributes, 1.18.2, 1.19, 1.19.1, 1.19.2]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.18.2_home.html
summary: "Data Attributes is a Minecraft mod, initially released for Minecraft 1.17.1 using the Fabric ecosystem. The mod does two things: overhauls Minecraft's entity attribute system to be more dynamic and to include follow on attributes (something found in many other games); and exposes entity attributes to datapack manipulation - allowing servers/pack makers easy customisation of every aspect of the entity attribute system."
---

<img src="https://github.com/CleverNucleus/Data-Attributes/blob/main/img/logo.png?raw=true" alt="Data Attributes" height="50%" />

<img src="https://img.shields.io/github/license/CleverNucleus/Data-Attributes?style=flat-square&color=367DBB" alt="License" />
<img src="https://img.shields.io/badge/dynamic/json?color=EC1F52&label=Applicable Versions&prefix=%20&query=2.releases&url=https://raw.githubusercontent.com/CleverNucleus/Data-Attributes/main/versions.json&style=flat-square" alt="Applicable Versions" />

## Welcome

This wiki covers the following:

- Integrating Data Attribues with your project.
- Data Attribute's specific capabilities.
- Using datapacks to configure Data Attributes, including:
  - Registering new attributes and overriding existing ones.
  - Adding attributes to entities or modifying their attribute containers.
  - Using attribute functions to create parent-child relations between attributes.
  - Using attribute properties to add extra data to attributes.
  - Relevant json formatting.
- API contents and usage.

## Integrating Data Attributes

Data Attributes has a [Curseforge](https://www.curseforge.com/minecraft/mc-mods/data-attributes) and [Modrinth](https://modrinth.com/mod/data-attributes) page. To add Data Attributes to your project, insert the following into your `build.gradle`:

<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#modrinth" data-toggle="tab">Modrinth</a></li>
    <li><a href="#curseforge" data-toggle="tab">Curseforge</a></li>
</ul>
<div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="modrinth">

{% highlight gradle %}

repositories {
    maven {
        name = "Modrinth"
        url = "https://api.modrinth.com/maven"
        content {
            includeGroup "maven.modrinth"
        }
    }
}

{% endhighlight %}

{% highlight gradle %}

dependencies {
    modImplementation "maven.modrinth:data-attributes:<version>"
}

{% endhighlight %}

</div>
<div role="tabpanel" class="tab-pane" id="curseforge">

{% highlight gradle %}

repositories {
    maven {
        name = "Curseforge"
        url = "https://www.cursemaven.com"
    }
}

{% endhighlight %}

{% highlight gradle %}

dependencies {
    modImplementation "curse.maven:dataattributes-514734:<fileId>"
}

{% endhighlight %}

The last number after the colon refers to the file id; you should always try to use the latest file.

</div>
</div>

{% include note.html content="Data Attributes depends on Fabric API, so your `build.gradle` must accommodate this." %}

## Specific Capabilities

Data Attributes lets you configure absolutely everything to do with attributes using datapacks. This is done by detatching attributes from the game instance and making them world-specific. That is to say, until a world is loaded, the game's attributes will remain unloaded and ambiguous. To really explain the significance of this, an understanding of how Minecraft implements attributes is needed.

### 1. Attributes in Vanilla Minecraft

Firstly, entity attributes (of class `EntityAttribute`), henceforth referred to as *attributes*, are just small data structures containing a translation key, fallback value, minimum value and maximum value - all of which are immutable. Attributes are registered (added) to the game statically when it is first loaded; registered here means that a *key* (of class `Identifier`) is mapped to the *value* that is the attribute. After all attributes are loaded, the static instances are used in-game as keys themselves. 

Attribute instances (of class `EntityAttributeInstance`) are a wrapper data structure that holds three relevant objects: an attribute, which acts as a key or identifier for the attribute instance; a value, which is the mutable *current* value that the attribute instance represents; and a map, which contains attribute modifiers that determine the value.

Attribute modifiers (of class `EntityAttributeModifier`) are another data structure that contains: a name; a uuid, which is used as a key/identifier for the modifier; an immutable value; and an operation (of class `EntityAttribteModifier$Operation`) that determines how the modifier is to use its value. The three operations are:

| Operation | Result |
| :-------- | :----- |
| <span id="redCode">**ADDITION**</span> | $$x+v$$ |
| <span id="redCode">**MULTIPLY_BASE**</span> | $$x+(x_1\times v)$$ |
| <span id="redCode">**MULTIPLY_TOTAL**</span> | $$x\times(1+v)$$ |

Where $$x$$ and $$v$$ are the attribute instance and attribute modifier values, respectively. Also of note is $$x_1$$, which represents the attribute instance's base value and any `ADDITION` modifiers; otherwise `MULTIPLY_BASE` and `MULTIPLY_TOTAL` would be the same.

Default attribute containers (of class `DefaultAttributeContainer`) are another data structure that holds a map of attributes and attribute instances. They are immutable and mapped to entity types (of class `EntityType`). The purpose of default attribute containers is to provide a collection of attributes and default attribute values different entities. When an entity (of class `LivingEntity` in this case) is first created, it is representative of an entity type, and therefore has a default attribute container assigned to it. This sets the entity's default/starting health, for example.

However, default attribute containers are immutable, as previously mentioned. This is not suitable for changing attribute values i.e. modifiers. Therefore, Minecraft provides the attribute container (of class `AttributeContainer`). This is a wrapper structure that holds a default attribute container as a fallback, as well as its own map of attributes and attribute instances. This map is mutable, and allows modifiers to be applied. 

{% include note.html content="Where *\"of class\"* is written, the class name is dependent on the mappings used." %}

### 2. The Attribute Hierarchy

<div style="text-align: center">

{% include image.html file="attribute_hierarchy.png" alt="attribute hierarchy" caption="Diagram of how components relate to create the attributes system." max-width="400" %}

</div>

### 3. Changes and Implementation

Although all attributes are as functional as each other in-game, some are added differently under the hood. Data Attributes splits attributes into two types: data driven, and hard coded. The latter refers not just to vanilla attributes, but to all modded ones as well (so long as they are, well, hard coded). Data driven attributes refers to those added through datapacks (json files). The difference between these is that the hard coded attributes remain loaded for every world, and are not ever *unregistered*. This means that when using an attribute object reference in the code, the object existing in the registry is the same as the object in any given attribute container, or attribute instance etc. Whereas the data driven attribute is world-dependent and can be unregistered and resynced, meaning that the object existing in the registry is not always the same as the object existing in any given attribute container or similar.

To get around this issue, Data Attributes uses identifiers (`Identifier`) to hold attributes in attribute containers or attribute instances etc. While the attribute *object* may change, the attribute *identifier* does not, even on reload and resync. This is at the core of what Data Attributes does: it simply changes the attributes system to use the attribute *identifier* instead of the attribute *object*, thereby allowing a fully dynamic, data driven attributes system.

{% include links.html %}
