---
title: "API"
keywords: opc offline player cache 1.18.2 1.19 1.19.1 1.19.2 api
tags: [offline_player_cache, api, 1.18.2, 1.19, 1.19.1, 1.19.2]
sidebar: opc_sidebar
permalink: opc-1.18.2_api.html
summary: 
---

## Structure and Overview

Offline Player Cache offers a small API package, located at `com.github.clevernucleus.opc.api` and contains the following:

```
📂api
 ┣📄CacheableValue.java interface
 ┗📄OfflinePlayerCache.java interface
```

The contents of `CacheableValue.java` are available [here](opc-1.18.2_cacheable_value#structure). The contents of `OfflinePlayerCache.java` are shown below:

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`String`</span> | <span id="redCode">**MODID**</span><br>The mod id. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`CacheableValue<V>`</span> | <span id="redCode">**register**(**CacheableValue&lt;V&gt;** key)</span> <br>Registers a cacheable value to the server: these are keys that instruct the server to cache some data from the players when they disconnect. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`T`</span> | <span id="redCode">**getOfflinePlayerCache**(**MinecraftServer** server, **T** fallback, **Function&lt;OfflinePlayerCache, T&gt;** function)</span> <br>Gains access to the offline player cache object. This should only be used on the logical server. |
| <span id="redType">`V`</span> | <span id="redCode">**get**(**UUID** uuid, **CacheableValue&lt;V&gt;** key)</span> <br>If the player is offline and exists in the cache, retrieves the last cached value. If the player is online, retrieves the player's current value. |
| <span id="redType">`V`</span> | <span id="redCode">**get**(**String** name, **CacheableValue&lt;V&gt;** key)</span> <br>If the player is offline and exists in the cache, retrieves the last cached value. If the player is online, retrieves the player's current value. |
| <span id="redType">`Collection<UUID>`</span> | <span id="redCode">**playerIds**()</span> <br>Returns all offline/cached AND online players' UUIDs. |
| <span id="redType">`Collection<String>`</span> | <span id="redCode">**playerNames**()</span> <br>Returns all offline/cached AND online players' names. |
| <span id="redType">`boolean`</span> | <span id="redCode">**isPlayerCached**(**UUID** uuid)</span> <br>Tests if the player with the input UUID exists in the cache. |
| <span id="redType">`boolean`</span> | <span id="redCode">**isPlayerCached**(**String** name)</span> <br>Tests if the player with the input name exists in the cache. |

## Using Offline Player Cache

This section will use the cacheable value implementation shown [here](opc-1.18.2_cacheable_value#example-implementation) to further provide an example of how OPC could be used in a project:

### 1. Registering a Cacheable Value

{% highlight java %}

import com.github.clevernucleus.opc.api.OfflinePlayerCache;

import net.fabricmc.api.ModInitializer;

public class ExampleMod implements ModInitializer {

    // Register our CurrentHealthValue from before.
    public static final CacheableValue<Float> CURRENT_HEALTH_VALUE = OfflinePlayerCache
    .register(new CurrentHealthValue());

    @Override
    public void onInitialize() {}
}

{% endhighlight %}

That is all that is needed. Players will disconnect and their current health will be cached on the server, ready to be accessed at any time. An potential usage is with commands - in the next example, we create a new command: `/example health <name>`.

### 2. Creating a command with OPC

{% highlight java %}

public static void register(CommandDispatcher<ServerCommandSource> dispatcher) {
    LiteralCommandNode<ServerCommandSource> exampleNode = CommandManager
    .literal("example").build();
    dispatcher.getRoot().addChild(exampleNode);
    
    LiteralCommandNode<ServerCommandSource> currentHealthNode = CommandManager
    .literal("health").build();
    exampleNode.addChild(currentHealthNode);

    ArgumentCommandNode<ServerCommandSource, String> nameNode = CommandManager
    .argument("name", StringArgumentType.string()).executes(ctx -> {
        MinecraftServer server = ctx.getSource().getServer();

        return OfflinePlayerCache.getOfflinePlayerCache(server, -1, opc -> {
            String player = StringArgumentType.getString(ctx, "name");
            float health = opc.get(player, ExampleMod.CURRENT_HEALTH_VALUE);
            LiteralText msg = new LiteralText(player + "'s current health is: " + health);

            ctx.getSource.sendFeedback(msg, false);

            return 1;
        });
    }).build();
    currentHealthNode.addChild(nameNode);
}

{% endhighlight %}

Using examples [1.](#1-registering-a-cacheable-value) and [2.](#2-creating-a-command-with-opc), we have added the command `/example health <name>`. The output of which is `<name>'s health is: <health>`. You can now effectively get the current health of all online and offline players on your server. This is obviously a very simplistic example: usually you would also add a command node to take a UUID argument in addition to the player's name; and you would have a suggestion provider to give you all player names/uuids. 

{% include links.html %}
