---
title: "API"
keywords: dataattributes data attributes 1.17.1 api
tags: [data_attributes, api, 1.17.1]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.17.1_api.html
summary: 
---

{% include important.html content="This section is now outdated and unsupported. Click [here](dataattributes-1.18.2_home) for the latest release." %}

## Structure and Overview

Data Attributes includes an API package, located at `com.github.clevernucleus.dataattributes.api` and contains the following:

```
📂api
 ┣📂attribute
 ┃ ┣📄AdditionFunction.java interface
 ┃ ┣📄AttributeBehaviour.java enum
 ┃ ┣📄AdditionFunction.java interface
 ┃ ┣📄IAttribute.java interface
 ┃ ┣📄IAttributeFunction.java interface
 ┃ ┗📄IEntityAttributeModifier.java interface
 ┃
 ┣📂event
 ┃ ┣📂client
 ┃ ┃ ┗📄ClientSyncedEvent.java class
 ┃ ┃
 ┃ ┣📄EntityAttributeEvents.java class
 ┃ ┣📄MathClampEvent.java class
 ┃ ┗📄ServerSyncedEvent.java class
 ┃
 ┗📄API.java class
```

## API Content

The following subsections detail every method/field in each java class located in the `api` package.

### 1. attribute.AdditionFunction.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`double`</span> | <span id="redCode">**add**(**double** current, **double** adding, **double** limit)</span> <br>A functional interface equivalent to a TriFunction, used to add a layer of abstraction to function behaviours. |

### 2. attribute.AttributeBehaviour.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`AttributeBehaviour`</span> | <span id="redCode">**FLAT**</span> <br>Flat/normal addition behaviour. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`AttributeBehaviour`</span> | <span id="redCode">**DIMINISHING**</span> <br>Diminishing addition behaviour. |
| <span id="redType">`public`</span><br><span id="redType">`byte`</span> | <span id="redCode">**id**()</span> <br>Returns the byte id of the behaviour enum. |
| <span id="redType">`public`</span><br><span id="redType">`double`</span> | <span id="redCode">**result**(**double** current, **double** adding, **double** limit)</span> <br>Returns the inputs through the correct function for the specific behaviour type. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`AttributeBehaviour`</span> | <span id="redCode">**fromId**(**byte** id)</span> <br>Takes a byte id and returns the corresponding behaviour type. |

### 3. attribute.IAttribute.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`double`</span> | <span id="redCode">**getDefaultValue**()</span> <br>Returns the attribute's default value. |
| <span id="redType">`double`</span> | <span id="redCode">**getMinValue**()</span> <br>Returns the attribute's minimum value. |
| <span id="redType">`double`</span> | <span id="redCode">**getMaxValue**()</span> <br>Returns the attribute's maximum value. |
| <span id="redType">`double`</span> | <span id="redCode">**clamp**(**double** value)</span> <br>For `EntityAttribute`, returns the input. For `ClampedEntityAttribute`, returns a clamped value between the min and max. |
| <span id="redType">`String`</span> | <span id="redCode">**getTranslationKey**()</span> <br>Returns the attribute's translation key (references a lang json name). |
| <span id="redType">`Collection<String>`</span> | <span id="redCode">**properties**()</span> <br>Returns an immutable collection of the properties' keys attached to this attribute. |
| <span id="redType">`boolean`</span> | <span id="redCode">**hasProperty**(**String** property)</span> <br>Returns true if this attribute has the input property key; false if not, or if the input is null. |
| <span id="redType">`String`</span> | <span id="redCode">**getProperty**(**String** property)</span> <br>Returns this attribute's property value assigned to the input property's key. If it does not exist or is null, returns "". |
| <span id="redType">`Collection<IAttributeFunction>`</span> | <span id="redCode">**functions**()</span> <br>Returns an immutable collection of the attribute functions attached to this attribute. |

### 4. attribute.AttributeFunction.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`Identifier`</span> | <span id="redCode">**attribute**()</span> <br>Returns the affected attribute's registry key. |
| <span id="redType">`AttributeBehaviour`</span> | <span id="redCode">**behaviour**()</span> <br>Returns the affected attribute's behaviour (if it uses flat or diminishing addition). |
| <span id="redType">`double`</span> | <span id="redCode">**multiplier**()</span> <br>Returns a multiplier applied to the resulting change in value (e.g. for every one point changed in the parent attribute). |

### 5. attribute.IEntityAttributeModifier.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`UUID`</span> | <span id="redCode">**getId**()</span> <br>Returns the modifier's uuid. |
| <span id="redType">`String`</span> | <span id="redCode">**getName**()</span> <br>Returns the modifier's name. |
| <span id="redType">`Operation`</span> | <span id="redCode">**getOperation**()</span> <br>Returns the modifier's operation. |
| <span id="redType">`double`</span> | <span id="redCode">**getValue**()</span> <br>Returns the modifier's (mutable) value. |
| <span id="redType">`void`</span> | <span id="redCode">**setValue**(**double** value)</span> <br>Sets the value of the modifier. |

### 6. event.client.ClientSyncedEvent.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Synced>`</span> | <span id="redCode">**EVENT**</span> <br>Provides a hook to the client, AFTER it has received server-side packets and synced all Data Attribute's relevant data, but BEFORE the player exists (i.e. during login). |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Synced** {...}</span> <br>Nested functional interface for `EVENT` . |

### 7. event.EntityAttributeEvents.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<AddedPre>`</span> | <span id="redCode">**MODIFIER_ADDED_PRE**</span> <br>Fired before an `EntityAttributeModifier` is added to an `EntityAttributeInstance`. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<AddedPost>`</span> | <span id="redCode">**MODIFIER_ADDED_POST**</span> <br>Fired after an `EntityAttributeModifier` is added to an `EntityAttributeInstance`. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<RemovedPre>`</span> | <span id="redCode">**MODIFIER_REMOVED_PRE**</span> <br>Fired before an `EntityAttributeModifier` is removed from an `EntityAttributeInstance`. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<RemovedPost>`</span> | <span id="redCode">**MODIFIER_REMOVED_POST**</span> <br>Fired after an `EntityAttributeModifier` is removed from an `EntityAttributeInstance`. |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**AddedPre** {...}</span> <br>Nested functional interface for `MODIFIER_ADDED_PRE` . |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**AddedPost** {...}</span> <br>Nested functional interface for `MODIFIER_ADDED_POST` . |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**RemovedPre** {...}</span> <br>Nested functional interface for `MODIFIER_REMOVED_PRE` . |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**RemovedPost** {...}</span> <br>Nested functional interface for `MODIFIER_REMOVED_POST` . |

### 8. event.MathClamp.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Clamp>`</span> | <span id="redCode">**EVENT**</span> <br>Fired on `EntityAttribute#clamp` and on `ClampedEntityAttribute#clamp` but before `MathHelper#clamp` is called. |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Clamp** {...}</span> <br>Nested functional interface for `EVENT` . |

### 9. event.ServerSyncedEvent.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Synced>`</span> | <span id="redCode">**EVENT**</span> <br>Provides a hook to the server, AFTER it has sent packets to the client and synced all Data Attribute's relevant data, but BEFORE the player exists (i.e. during login). |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Synced** {...}</span> <br>Nested functional interface for `EVENT` . |

### 10. API.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`String`</span> | <span id="redCode">**MODID**</span> <br>The modid for Data Attributes. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`Supplier<EntityAttribute>`</span> | <span id="redCode">**getAttribute**(**Identifier** attributeKey)</span> <br>Returns a `Supplier` getting the registered attribute assigned to the input key. Uses a supplier because attributes added through json are null until datapacks are loaded/synced to the client, so static initialisation would not work. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`double`</span> | <span id="redCode">**add**(**double** current, **double** adding, **double** limit)</span> <br>Adds or subtracts the input values, with diminishing returns tending towards the limit. |



{% include links.html %}
