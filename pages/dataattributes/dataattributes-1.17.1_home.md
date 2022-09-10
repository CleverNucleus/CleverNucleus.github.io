---
title: "Home"
keywords: home homepage dataattributes data attributes 1.17.1
tags: [introduction, getting_started, data_attributes, 1.17.1]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.17.1_home.html
summary: "Data Attributes is a Minecraft mod, initially released for Minecraft 1.17.1 using the Fabric ecosystem. The mod does two things: overhauls Minecraft's entity attribute system to be more dynamic and to include follow on attributes (something found in many other games); and exposes entity attributes to datapack manipulation - allowing servers/pack makers easy customisation of every aspect of the entity attribute system."
---

{% include important.html content="This section is now outdated and unsupported. Click [here](dataattributes-1.18.2_home) for the latest release." %}

<img src="https://github.com/CleverNucleus/Data-Attributes/blob/main/img/logo.png?raw=true" alt="Data Attributes" />

<img src="https://img.shields.io/github/license/CleverNucleus/Data-Attributes?style=flat-square&color=367DBB" alt="License" />
<img src="https://img.shields.io/badge/dynamic/json?color=EC1F52&label=Applicable Versions&prefix=%20&query=0.releases&url=https://raw.githubusercontent.com/CleverNucleus/Data-Attributes/main/versions.json&style=flat-square" alt="Applicable Versions" />

## Welcome

This wiki covers the following:

- Using Data Attributes as a dependency for your mod (Gradle).
- Changing attribute's default, minimum and maximum values, as well as their translation key.
- Adding new attributes.
- Modifying the contents of living entities attribute containers.
- Attribute Functions.
- Attribute Properties.
- Relevant Json formatting.
- API structure and usage.

## Integrating Data Attributes

Data Attributes can be added to your project inserting the following to your `build.gradle`:

{% highlight gradle %}

repositories {
    maven {
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

Please note that Data Attributes depends on Fabric API, so your `build.gradle` will have to accommodate that.

{% include links.html %}
