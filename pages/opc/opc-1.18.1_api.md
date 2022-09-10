---
title: "API"
keywords: opc offline player cache 1.18.1 api
tags: [offline_player_cache, api, 1.18.1]
sidebar: opc_sidebar
permalink: opc-1.18.1_api.html
summary: 
---

{% include important.html content="This section is now outdated and unsupported. Click [here](opc-1.18.2_home) for the latest release." %}

## Structure and Overview

Offline Player Cache offers a small API package, located at `com.github.clevernucleus.opc.api` and contains the following:

```
📂api
 ┣📄CacheableValue.java abstract class
 ┗📄PlayerCacheAPI.java class
```

The contents of `CacheableValue.java` are available [here](opc-1.18.2_cacheable_value#structure). The contents of `PlayerCacheAPI.java` are shown below:

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`String`</span> | <span id="redCode">**MODID**</span><br>The mod id. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`CacheableValue<T>`</span> | <span id="redCode">**registerCacheableValue**(**CacheableValue&lt;T&gt;** key)</span> <br>Gains access to the offline player cache object. This should only be used on the logical server. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`T`</span> | <span id="redCode">**get**(**MinecraftServer** server, **UUID** uuid, **CacheableValue&lt;T&gt;** key)</span> <br>If the player is offline and exists in the cache, retrieves the last cached value. If the player is online, retrieves the player's current value. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`T`</span> | <span id="redCode">**get**(**MinecraftServer** server, **String** name, **CacheableValue&lt;T&gt;** key)</span> <br>If the player is offline and exists in the cache, retrieves the last cached value. If the player is online, retrieves the player's current value. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`void`</span> | <span id="redCode">**uncacheValue**(**String** name, **CacheableValue&lt;T&gt;** key)</span> <br>Removes the input value cached to the input player, if it exists. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`void`</span> | <span id="redCode">**uncachePlayer**(**UUID** uuid)</span> <br>Removes the player who's uuid this belongs to from the cache, if they exist. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`void`</span> | <span id="redCode">**uncachePlayer**(**String** name)</span> <br>Removes the player who's name this belongs to from the cache, if they exist. |
| <span id="redType">`Collection<String>`</span> | <span id="redCode">**getPlayerNames**(**MinecraftServer** server)</span> <br>Returns all offline/cached AND online players' names. |
| <span id="redType">`Collection<UUID>`</span> | <span id="redCode">**getPlayerIds**(**MinecraftServer** server)</span> <br>Returns all offline/cached AND online players' UUIDs. |

## Using Offline Player Cache

This section will use the cacheable value implementation shown [here](opc-1.18.2_cacheable_value#example-implementation) to further provide an example of how OPC could be used in a project:

### 1. Registering a Cacheable Value

{% highlight java %}

import com.github.clevernucleus.opc.api.PlayerCacheAPI;

import net.fabricmc.api.ModInitializer;

public class ExampleMod implements ModInitializer {

    // Register our CurrentHealthValue from before.
    public static final CacheableValue<Float> CURRENT_HEALTH_VALUE = PlayerCacheAPI
    .registerCacheableValue(new CurrentHealthValue());

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
        String player = StringArgumentType.getString(ctx, "name");
        float health = PlayerCacheAPI.get(server, player, ExampleMod.CURRENT_HEALTH_VALUE);
        LiteralText msg = new LiteralText(player + "'s current health is: " + health);

        ctx.getSource.sendFeedback(msg, false);

        return 1;
    }).build();
    currentHealthNode.addChild(nameNode);
}

{% endhighlight %}

Using examples [1.](#1-registering-a-cacheable-value) and [2.](#2-creating-a-command-with-opc), we have added the command `/example health <name>`. The output of which is `<name>'s health is: <health>`. You can now effectively get the current health of all online and offline players on your server. This is obviously a very simplistic example: usually you would also add a command node to take a UUID argument in addition to the player's name; and you would have a suggestion provider to give you all player names/uuids. 

{% include links.html %}
