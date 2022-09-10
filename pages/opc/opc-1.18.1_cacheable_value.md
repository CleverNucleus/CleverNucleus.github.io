---
title: "Cacheable Value"
keywords: opc offline player cache 1.18.1 cacheable value
tags: [offline_player_cache, 1.18.1]
sidebar: opc_sidebar
permalink: opc-1.18.1_cacheable_value.html
summary: This section details the content and implementation of the Cacheable Value abstract class from a developer perspective. 
---

{% include important.html content="This section is now outdated and unsupported. Click [here](opc-1.18.2_home) for the latest release." %}

## Welcome

The Cacheable Value is a type of key-wrapper hybrid. It serves to hold a generic type and provide a value of this type through a key-function map.

## Structure

The `CacheableValue` abstract class accepts a generic argument that represents its data type. For example, `CacheableValue<Integer>` holds an integer value. The abstract class has the following methods, where `T` denotes its generic type:

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`T`</span> | <span id="redCode">**get**(**ServerPlayerEntity** player)</span> <br>When the player is online, gets the value from the player. When the player disconnects, this is used to cache the value on the server. |
| <span id="redType">`T`</span> | <span id="redCode">**readFromNbt**(**NbtCompound** tag)</span> <br>Reads the value from nbt. |
| <span id="redType">`void`</span> | <span id="redCode">**writeToNbt**(**NbtCompound** tag, **Object** value)</span> <br>Writes the value to nbt. |

## Example Implementation

For every value or piece of data that a mod wishes to cache, they must create a new `CacheableValue` implementation. The following is an example:

{% highlight java %}
import com.github.clevernucleus.opc.api.CacheableValue;

import net.minecraft.nbt.NbtCompound;
import net.minecraft.server.network.ServerPlayerEntity;
import net.minecraft.util.Identifier;

public class CurrentHealthValue extends CacheableValue<Float> {
    public CurrentHealthValue() {
        super(new Identifier("opc:current_health"));
    }

    @Override
    public Float get(ServerPlayerEntity player) {
        return (float)player.getHealth();
    }

    @Override
    public Float readFromNbt(NbtCompound tag) {
        return tag.getFloat("CurrentHealth");
    }

    @Override
    public void writeToNbt(NbtCompound tag, Object value) {
        tag.putFloat("CurrentHealth", (Float)value);
    }
}
{% endhighlight %}

### Notes

- Your `Identifier` that is present in the constructor should be of the form `modid:key`.
- This implementation will cache players' current health when they disconnect, and provide this value if they are offline, or the *current* current health if they are online.
- You should try to ensure that the appropriate type is used in your generic. In the example above, `Float` is used because the return type of `player#getHealth` is `float`. Using `Integer` here would not be appropriate as you would lose data, and using `Double` here would also not be appropriate because the extra precision is not utilised.

{% include links.html %}
