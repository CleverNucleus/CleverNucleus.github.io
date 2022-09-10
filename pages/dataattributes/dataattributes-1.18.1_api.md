---
title: "API"
keywords: dataattributes data attributes 1.18.1 api
tags: [data_attributes, api, 1.18.1]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.18.1_api.html
summary: 
---

{% include important.html content="This section is now outdated and unsupported. Click [here](dataattributes-1.18.2_home) for the latest release." %}

## Structure and Overview

Data Attributes includes an API package, located at `com.github.clevernucleus.dataattributes.api` and contains the following:

```
📂api
 ┣📂attribute
 ┃ ┣📄IEntityAttribute.java interface
 ┃ ┣📄IEntityAttributeInstance.java interface
 ┃ ┣📄IItemEntityAttributeModifiers.java interface
 ┃ ┗📄StackingBehaviour.java enum
 ┃
 ┣📂event
 ┃ ┣📂client
 ┃ ┃ ┗📄ClientSyncedEvent.java class
 ┃ ┃
 ┃ ┣📄EntityAttributeModifiedEvents.java class
 ┃ ┣📄ItemStackCreatedEvent.java class
 ┃ ┗📄ServerSyncedEvent.java class
 ┃
 ┣📂util
 ┃ ┣📄ItemFields.java class
 ┃ ┣📄Maths.java class
 ┃ ┣📄RandDistribution.java class
 ┃ ┗📄VoidConsumer.java interface
 ┃
 ┗📄API.java class
```

## API Content

The following subsections detail every method/field in each java class located in the `api` package.

### 1. attribute.IEntityAttribute.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`double`</span> | <span id="redCode">**minValue**()</span> <br>Returns the attribute's minimum value. |
| <span id="redType">`double`</span> | <span id="redCode">**maxValue**()</span> <br>Returns the attribute's maximum value. |
| <span id="redType">`StackingBehaviour`</span> | <span id="redCode">**stackingBehaviour**()</span> <br>Returns the attribute's stacking behaviour enum type. |
| <span id="redType">`Map<IEntityAttribute, Double>`</span> | <span id="redCode">**children**()</span> <br>Returns an immutable map of the function-children attached to this attribute. |
| <span id="redType">`Collection<String>`</span> | <span id="redCode">**properties**()</span> <br>Returns an immutable collection of the properties' keys attached to this attribute. |
| <span id="redType">`boolean`</span> | <span id="redCode">**hasProperty**(**String** property)</span> <br>Returns `true` if this attribute has the input property key, `false` if not or if the input is null. |
| <span id="redType">`String`</span> | <span id="redCode">**getProperty**(**String** property)</span> <br>Returns the attribute's property value mapped to the input key. If it does not exist or is null, returns `""`. |

### 2. attribute.IEntityAttributeInstance.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`void`</span> | <span id="redCode">**updateModifier**(**UUID** uuid, **double** value)</span> <br>Changes the value of the input modifier (if it exists) and updates the instance and all children. |

### 3. attribute.IItemAttributeModifiers.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`default`</span><br><span id="redType">`Multimap<EntityAttribute, EntityAttributeModifier>`</span> | <span id="redCode">**getAttributeModifiers**(**ItemStack** stack, **EquipmentSlot** slot)</span> <br>This method provides a mutable attribute modifier multimap so that items can have dynamically changing modifiers based on nbt. By default returns an empty `HashMultimap`. |

### 4. attribute.StackingBehaviour.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`StackingBehaviour`</span> | <span id="redCode">**FLAT**</span> <br>Flat/normal stacking behaviour. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`StackingBehaviour`</span> | <span id="redCode">**DIMINISHING**</span> <br>Flat/normal stacking behaviour. |
| <span id="redType">`public`</span><br><span id="redType">`byte`</span> | <span id="redCode">**id**()</span> <br>Returns the byte id of the behaviour enum. |
| <span id="redType">`public`</span><br><span id="redType">`double`</span> | <span id="redCode">**result**(**double** current, **double** input)</span> <br>Returns the inputs through the correct function for the specific behaviour type. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`StackingBehaviour`</span> | <span id="redCode">**of**(**byte** id)</span> <br>Takes a byte id and retusn the corresponding behaviour type. |

### 5. event.client.ClientSyncedEvent.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Synced>`</span> | <span id="redCode">**EVENT**</span> <br>Provides a hook to the client, AFTER it has received server-side packets and synced all Data Attribute's relevant data, but BEFORE the player exists (i.e. during login). |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Synced** {...}</span> <br>Nested functional interface for `EVENT` . |

### 6. event.EntityAttributeModifiedEvents.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Modified>`</span> | <span id="redCode">**MODIFIED**</span> <br>Fires when the value of an attribute instance has been modified, either by adding/removing a modifier or changing the value of a modifier. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Clamp>`</span> | <span id="redCode">**CLAMPED**</span> <br>Fires after the attribute instance value is calculated, but before it is output. This offers one last chance to alter the value in some way (for example round a decimal to an integer). |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Modified** {...}</span> <br>Nested functional interface for `MODIFIED` . |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Clamp** {...}</span> <br>Nested functional interface for `CLAMPED` . |

### 7. event.ItemStackCreatedEvent.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Created>`</span> | <span id="redCode">**EVENT**</span> <br>Offers a hook into ItemStack constructor, allowing nbt data to be attached to stacks on creation. |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Created** {...}</span> <br>Nested functional interface for `EVENT` . |

### 8. event.ServerSyncedEvent.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Synced>`</span> | <span id="redCode">**EVENT**</span> <br>Provides a hook to the server, AFTER it has sent packets to the client and synced all Data Attribute's relevant data, but BEFORE the player exists (i.e. during login). |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Synced** {...}</span> <br>Nested functional interface for `EVENT` . |

### 9. util.ItemFields.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`UUID`</span> | <span id="redCode">**attackDamageModifierID**()</span> <br>Returns `Item#ATTACK_DAMAGE_MODIFIER_ID`. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`UUID`</span> | <span id="redCode">**attackSpeedModifierID**()</span> <br>Returns `Item#ATTACK_SPEED_MODIFIER_ID`. |

### 10. util.Maths.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`Map<K, V>`</span> | <span id="redCode">**enumLookupMap**(**V[]** values, **Function&lt;V, K&gt;** mapping)</span> <br>Creates a static lookup map for the input `enum#values` implementation. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`float`</span> | <span id="redCode">**parse**(**String** string)</span> <br>Parses a string to a float. If it cannot be parsed, returns `0F`. |

### 11. util.RandDistribution.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span> | <span id="redCode">**RandDistribution**(**T** fallback)</span> <br>Constructor. |
| <span id="redType">`public`</span><br><span id="redType">`void`</span> | <span id="redCode">**add**(**T** input, **float** weight)</span> <br>Adds to the internal inventory of the distributor. |
| <span id="redType">`public`</span><br><span id="redType">`T`</span> | <span id="redCode">**getDistributedRandom**()</span> <br>Returns a randomly distributed and weighted result. |

### 12. util.VoidConsumer.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`void`</span> | <span id="redCode">**accept**()</span> <br>A functional interface equivalent to a Consumer, but with no parameters. |

### 13. DataAttributesAPI.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`String`</span> | <span id="redCode">**MODID**</span> <br>The mod id. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`Supplier<EntityAttribute>`</span> | <span id="redCode">**getAttribute**(**Identifier** registryKey)</span> <br>Returns a `Supplier` getting the registered attribute assigned to the input key. Uses a supplier because attributes added through json are null until datapacks are loaded/synced to the client, so static initialisation would not work. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`boolean`</span> | <span id="redCode">**checkHasAttribute**(**LivingEntity** entity, **ItemStack** stack, **EquipmentSlot** slot)</span> <br>Checks to see if the entity has the attributes required of hte itemStack's attribute modifiers; returns `true` if so, `false` if not. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`T`</span> | <span id="redCode">**ifPresent**(**LivingEntity** entity, **Supplier&lt;EntityAttribute&gt;** attribute, **T** fallback, **Function&lt;Float, T&gt;** function)</span> <br>Allows for Optional-like use of attributes that may or may not exist all the time. This is the correct way of getting and using values from attributes loaded by datapacks. If the entity is not null and the input attribute exists, and the entity has the input attribute, passes and returns the input function. Otherwise, returns the fallback input. |

{% include links.html %}
