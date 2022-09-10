---
title: "API"
keywords: dataattributes data attributes 1.18.2 1.19 1.19.1 1.19.2 api
tags: [data_attributes, api, 1.18.2, 1.19, 1.19.1, 1.19.2]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.18.2_api.html
summary: 
---

## Structure and Overview

Data Attributes includes an API package, located at `com.github.clevernucleus.dataattributes.api` and contains the following:

```
📂api
 ┣📂attribute
 ┃ ┣📄IEntityAttribute.java interface
 ┃ ┣📄IEntityAttributeInstance.java interface
 ┃ ┣📄IItemEntityAttributeModifiers.java interface
 ┃ ┣📄StackingBehaviour.java enum
 ┃ ┗📄StackingFunction.java interface
 ┃
 ┣📂event
 ┃ ┣📄EntityAttributeModifiedEvents.java class
 ┃ ┗📄AttributesReloadedEvent.java class
 ┃
 ┣📂item
 ┃ ┣📄ItemFields.java class
 ┃ ┗📄ItemHelper.java interface
 ┃
 ┣📂util
 ┃ ┣📄Maths.java class
 ┃ ┣📄RandDistribution.java class
 ┃ ┗📄VoidConsumer.java interface
 ┃
 ┗📄DataAttributesAPI.java class
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

### 3. attribute.IItemEntityAttributeModifiers.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`default`</span><br><span id="redType">`Multimap<EntityAttribute, EntityAttributeModifier>`</span> | <span id="redCode">**getAttributeModifiers**(**ItemStack** stack, **EquipmentSlot** slot)</span> <br>This method provides a mutable attribute modifier multimap so that items can have dynamically changing modifiers based on nbt. By default returns an empty `HashMultimap`. |

### 4. attribute.StackingBehaviour.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`StackingBehaviour`</span> | <span id="redCode">**FLAT**</span> <br>Flat/normal stacking behaviour. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`StackingBehaviour`</span> | <span id="redCode">**DIMINISHING**</span> <br>Flat/normal stacking behaviour. |
| <span id="redType">`public`</span><br><span id="redType">`byte`</span> | <span id="redCode">**id**()</span> <br>Returns the byte id of the behaviour enum. |
| <span id="redType">`public`</span><br><span id="redType">`double`</span> | <span id="redCode">**max**(**double** current, **double** input)</span> <br>Clamps the input and returns the greater of the current or absolute clamped input. |
| <span id="redType">`public`</span><br><span id="redType">`double`</span> | <span id="redCode">**stack**(**double** current, **double** input)</span> <br>Clamps the input and returns the current + the absolute clamped input. |
| <span id="redType">`public`</span><br><span id="redType">`double`</span> | <span id="redCode">**result**(**double** k, **double** k2, **double** v, **double** v2, **double** increment)</span> <br>Returns the inputs through the correct function for the specific behaviour type. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`StackingBehaviour`</span> | <span id="redCode">**of**(**byte** id)</span> <br>Takes a byte id and retusn the corresponding behaviour type. |

### 5. attribute.StackingFunction.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`double`</span> | <span id="redCode">**result**(**double** k, **double** k2, **double** v, **double** v2, **double** increment)</span> <br>Returns the inputs through the correct function for the specific behaviour type. |

### 6. event.EntityAttributeModifiedEvents.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Reloaded>`</span> | <span id="redCode">**EVENT**</span> <br>Fires when the value of an attribute instance has been modified, either by adding/removing a modifier or changing the value of a modifier. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Clamp>`</span> | <span id="redCode">**CLAMPED**</span> <br>Fires after the attribute instance value is calculated, but before it is output. This offers one last chance to alter the value in some way (for example round a decimal to an integer). |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Modified** {...}</span> <br>Nested functional interface for `MODIFIED` . |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Clamp** {...}</span> <br>Nested functional interface for `CLAMPED` . |

### 7. event.AttributesReloadedEvent.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Reloaded>`</span> | <span id="redCode">**EVENT**</span> <br>Event that alows for logic reliant on datapack attributes to be ordered after they are loaded on both the server and client. Fired on Server upon joining world and on datapack reload through `/reload`. Fired on Client when selecting datapacks and after server has synced with client. |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Reloaded** {...}</span> <br>Nested functional interface for `EVENT` . |

### 8. item.ItemFields.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`UUID`</span> | <span id="redCode">**attackDamageModifierID**()</span> <br>Returns `Item#ATTACK_DAMAGE_MODIFIER_ID`. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`UUID`</span> | <span id="redCode">**attackSpeedModifierID**()</span> <br>Returns `Item#ATTACK_SPEED_MODIFIER_ID`. |

### 9. item.ItemHelper.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`default`</span><br><span id="redType">`void`</span> | <span id="redCode">**onStackCreated**(**ItemStack** itemStack, **int** count)</span> <br>Fired on the constructor for itemstacks. All items are automatically an instance of `ItemHelper`, so checks should be made when using this to avoid running unnecessary logic on all itemstack creation events. Example usage includes attaching nbt data when an itemstack is first created. |
| <span id="redType">`default`</span><br><span id="redType">`float`</span> | <span id="redCode">**getAttackDamage**(**ItemStack** itemStack)</span> <br>Returns `0.0` by default. ItemStack dependent version of `SwordItem#getAttackDamage` and `MiningToolItem#getAttackDamage`; for the `SwordItem` and `MiningToolItem`, returns the non-itemstack dependent methods. |
| <span id="redType">`default`</span><br><span id="redType">`float`</span> | <span id="redCode">**getProtection**(**ItemStack** itemStack)</span> <br>Returns `0.0` by default. ItemStack dependent version of `ArmorItem#getProtection`; for `ArmorItem`, returns the non-itemstack dependent method. |
| <span id="redType">`default`</span><br><span id="redType">`float`</span> | <span id="redCode">**getToughness**(**ItemStack** itemStack)</span> <br>Returns `0.0` by default. ItemStack dependent version of `ArmorItem#getToughness`; for `ArmorItem`, returns the non-itemstack dependent method. |
| <span id="redType">`SoundEvent`</span> | <span id="redCode">**getEquipSound**(**ItemStack** itemStack)</span> <br>Returns `null` by default. ItemStack dependent version of `Item#getEquipSound`. |

### 10. util.Maths.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`Map<K, V>`</span> | <span id="redCode">**enumLookupMap**(**V[]** values, **Function&lt;V, K&gt;** mapping)</span> <br>Creates a static lookup map for the input `enum#values` implementation. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`float`</span> | <span id="redCode">**parse**(**String** string)</span> <br>Parses a string to a float. If it cannot be parsed, returns `0F`. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`double`</span> | <span id="redCode">**stairs**(**double** x, **double** stretch, **double** steepness, **double** xOffset, **double** yOffset)</span> <br>A staircase function based on `Math#sin`. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`boolean`</span> | <span id="redCode">**isWithinLimits**(**double** value, **double** min, **double** max)</span> <br>Returns `true` if the value is less than max and greater than or equal to min. |

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
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`T`</span> | <span id="redCode">**ifPresent**(**LivingEntity** entity, **Supplier&lt;EntityAttribute&gt;** attribute, **T** fallback, **Function&lt;Float, T&gt;** function)</span> <br>Allows for Optional-like use of attributes that may or may not exist all the time. This is the correct way of getting and using values from attributes loaded by datapacks. If the entity is not null and the input attribute exists, and the entity has the input attribute, passes and returns the input function. Otherwise, returns the fallback input. |

{% include links.html %}
