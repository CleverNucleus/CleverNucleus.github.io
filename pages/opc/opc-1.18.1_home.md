---
title: "Home"
keywords: home homepage opc offline player cache 1.18.1 commands
tags: [introduction, offline_player_cache, 1.18.1]
sidebar: opc_sidebar
permalink: opc-1.18.1_home.html
summary: Offline Player Cache (OPC) is a Minecraft API mod built using the Fabric framework to allow caching player data on the server while the player is offline. This means that mods can use this API to access offline player's data. This was primarily developed as a way to allow global and persistent leaderboards for Minecraft servers that included offline players.
---

{% include important.html content="This section is now outdated and unsupported. Click [here](opc-1.18.2_home) for the latest release." %}

<img src="https://github.com/CleverNucleus/Offline-Player-Cache/blob/main/img/logo.png?raw=true" alt="Offline Player Cache" />

<img src="https://img.shields.io/github/license/CleverNucleus/Offline-Player-Cache?style=flat-square&color=367DBB" alt="License" />
<img src="https://img.shields.io/badge/dynamic/json?color=EC1F52&label=Applicable Versions&prefix=%20&query=0.releases&url=https://raw.githubusercontent.com/CleverNucleus/Offline-Player-Cache/main/versions.json&style=flat-square" alt="Applicable Versions" />

## Welcome

This wiki covers the following: 

- Specific Use cases for Offline Player Cache.
- Accessing/Removing data with Commands.
- Adding Offline Player Cache to your project.
- Creating and using Cacheable Values.
- API structure and usage.

## Specific Use Cases

OPC is designed to be used in conjunction with other mods. By itself it adds no content. [PlayerEx](playerex-1.18.2_home) uses it to cache players' levels when they disconnect from a server, so that server leaderboards that display player levels are persistent and not dependent on who is online at the time of viewing. Similar to a '<a href="https://readyplayerone.fandom.com/wiki/Scoreboard" target="_blank">*Ready Player One*</a>' type situation - it would be odd if Parzival disappeard from the scoreboard every time he logged off. When the player logs back in, the cached value is removed and instead the real-time value is used.

In the aforementioned case, it is only an integer that is cached/used. However, anything can be cached, so long as it can be read from/written to nbt data. Furthermore, this data is not synced across to the client, which means developers do not need to be as mindful of nbt size. 

It should be noted that the functionality provided by this mod relies on players being using legitimate accounts and not cracked versions, as their uuid/name must be valid for their data to be cached. 

## Commands

OPC registers three commands to access and/or remove cached data:

- `/opc get uuid|name <uuid|name> <key>` Returns the input uuid/name player's value; if they are online, returns their current value, if they are offline returns their cached value. If the value is an `instanceof` `Number` and run from a command block, the redstone output is the absolute modulus of 16.
- `/opc remove uuid|name <uuid|name> <key>` If the input uuid/name player is offline, removes that player's cached value determined by the input key; if the player is online, does nothing.
- `/opc remove uuid|name <uuid|name>` If the input uuid/name player is offline, removes all of their cached data; if the player is online, does nothing.

## Adding OPC to your Project

This mod can be accessed from its <a href="https://github.com/CleverNucleus/Offline-Player-Cache" target="_blank">Github</a> repository, either by cloning and using a local maven repository via the `publishToMavenLocal` task, or by using <a href="https://jitpack.io/" target="_blank">JitPack</a>. The latter is used below to show how OPC can be added to your project. In your `build.gradle` include the following:

{% highlight gradle %}
repositories {
    maven {
        name = "Jitpack"
        url = "https://jitpack.io"
    }
}
{% endhighlight %}

{% highlight gradle %}
dependencies {
    modImplementation "com.github.CleverNucleus:Offline-Player-Cache:<version>"
    include "com.github.CleverNucleus:Offline-Player-Cache:<version>"
}
{% endhighlight %}

OPC is designed to be included in your project jar (jar-in-jar), so it is recommended to use `include`.

{% include links.html %}
