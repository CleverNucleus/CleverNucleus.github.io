---
title: "API"
keywords: playerex player ex 1.18.2 1.19 1.19.1 1.19.2 api
tags: [playerex, api, 1.18.2, 1.19, 1.19.1, 1.19.2]
sidebar: playerex_sidebar
permalink: playerex-1.18.2_api.html
summary: 
---

## Structure and Overview

PlayerEx includes an API package, located at `com.github.clevernucleus.playerex.api` and contains the following:

```
📂api
 ┣📂client
 ┃ ┣📄ClientUtil.java class
 ┃ ┣📄PageLayer.java class
 ┃ ┣📄PageRegistry.java class
 ┃ ┗📄RenderComponent.java class
 ┃
 ┣📂damage
 ┃ ┣📄DamageFunction.java interface
 ┃ ┗📄DamagePredicate.java interface
 ┃
 ┣📂event
 ┃ ┣📄LivingEntityEvents.java class
 ┃ ┗📄PlayerEntityEvents.java class
 ┃
 ┣📄EntityAttributeSupplier.java class
 ┣📄ExAPI.java class
 ┣📄ExConfig.java interface
 ┣📄PacketType.java enum
 ┗📄PlayerData.java interface
```

The PlayerEx API can be split into four distinct sub-systems: the client-side page/tab registry, which exists to allow developers to add more inventory tabs and content to existing inventory pages; the damage-registry, which exists to allow developers to register damage handlers to better manipulate damage events; the `LivingEntityEvents` and `PlayerEntityEvents` classes, which provide developers useful hooks into attributes and specific methods; and finally, the attribute registry - which acts to provide developers with attribute suppliers and access to all the attributes PlayerEx adds.

## API Content

The following subsections detail every method/field in each java class located in the `api` package.

### 1. client.ClientUtil.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`DecimalFormat`</span> | <span id="redCode">**FORMATTING_2**</span> <br>Format for two decimal places. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`DecimalFormat`</span> | <span id="redCode">**FORMATTING_3**</span> <br>Format for three decimal places. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`double`</span> | <span id="redCode">**displayValue**(**Supplier&lt;EntityAttribute&gt;** attribute, **double** value)</span> <br>Takes the input attribute and looks for the `PERCENTAGE_PROPERTY` property; if it is present, multiplies the input value by the property's multiplier. If not, then looks for the `MULTIPLIER_PROPERTY` property; if present, multiplies the input value by the property's multiplier. Else returns the input value. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`String`</span> | <span id="redCode">**formatValue**(**Supplier&lt;EntityAttribute&gt;** attribute, **double** value)</span> <br>If the input value is positive, adds the `+` prefix. If the input attribute has the `PERCENTAGE_PROPERTY` property, appends the `%` suffix. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`void`</span> | <span id="redCode">**appendChildrenToTooltip**(**List&lt;Text&gt;** tooltip, **Supplier&lt;EntityAttribute&gt;** attribute)</span> <br>Looks for the input attribute's functions, and if there are any, formats them and adds them to the input list as tooltips. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`void`</span> | <span id="redCode">**modifyAttributes**(**PacketType** type, **Consumer&lt;BiConsumer&lt;EntityAttributeSupplier, Double&gt;&gt;** ... consumers)</span> <br>Client-side. Sends a packet to the server to modify the attribute modifiers provided by PlayerEx by the input amount, and performs the checks and functions dictated by the PacketType (this logic is run on the server when it receives the packet). |

### 2. client.PageLayer.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`protected`</span><br><span id="redType">`final`</span><br><span id="redType">`HandledScreen<?>`</span> | <span id="redCode">**parent**</span> <br>Reference to the parent screen. |
| <span id="redType">`public`</span><br><span id="redType">`void`</span> | <span id="redCode">**render**(**MatrixStack** matrices, **int** mouseX, **int** mouseY, **float** delta)</span> <br>This is where text and tooltips can be rendered, in that order. |
| <span id="redType">`public`</span><br><span id="redType">`void`</span> | <span id="redCode">**drawBackground**(**MatrixStack** matrices, **float** delta, **int** mouseX, **int** mouseY)</span> <br>This is where textures and widgets can be rendered, in that order. |

### 3. client.PageRegistry.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`void`</span> | <span id="redCode">**registerPage**(**Identifier** pageId, **Identifier** icon, **Identifier** texture, **Text** title)</span> <br>Registers a page and tab to the PlayerEx screen. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`void`</span> | <span id="redCode">**registerPage**(**Identifier** pageId, **Identifier** icon, **Text** title)</span> <br>Registers a page and tab to the PlayerEx screen, but with the default background. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`void`</span> | <span id="redCode">**registerLayer**(**Identifier** pageId, **PageLayer.Builder** builder)</span> <br>Registers a page layer to a page of the same input pageId. |

### 4. client.RenderComponent.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`RenderComponent`</span> | <span id="redCode">**of**(**Supplier&lt;EntityAttribute&gt;** attribute, **Function&lt;LivingEntity, Text&gt;** function, **Function&lt;LivingEntity, List&lt;Text&gt;&gt;** tooltip, **int** dx, **int** dy)</span> <br>Creates a new `RenderComponent` object. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`RenderComponent`</span> | <span id="redCode">**of**(**Function&lt;LivingEntity, Text&gt;** function, **Function&lt;LivingEntity, List&lt;Text&gt;&gt;** tooltip, **int** dx, **int** dy)</span> <br>Creates a new `RenderComponent` object. |
| <span id="redType">`private`</span><br><span id="redType">`boolean`</span> | <span id="redCode">**isMouseOver**(**float** x, **float** y, **float** width, **float** height, **int** mouseX, **int** mouseY)</span> <br>Checks to see if the mouse is over the input area. |
| <span id="redType">`public`</span><br><span id="redType">`void`</span> | <span id="redCode">**renderText**(**LivingEntity** livingEntity, **MatrixStack** matrices, **TextRenderer** textRenderer, **int** x, **int** y, **float** scaleX, **float** scaleY)</span> <br>Renders the text from the input function. |
| <span id="redType">`public`</span><br><span id="redType">`void`</span> | <span id="redCode">**renderTooltip**(**LivingEntity** livingEntity, **RenderTooltip** consumer, **MatrixStack** matrices, **TextRenderer** textRenderer, **int** x, **int** y, **int** mouseX, **int** mouseY, **float** scaleX, **float** scaleY)</span> <br>Renders tooltip for the given text. |

### 5. damage.DamageFunction.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`float`</span> | <span id="redCode">**apply**(**LivingEntity** livingEntity, **DamageSource** source, **float** damage)</span> <br>Using the input conditions, modifies the incoming damage (either reducing or increasing it) and returns the result. |

### 6. damage.DamagePredicate.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`boolean`</span> | <span id="redCode">**test**(**LivingEntity** livingEntity, **DamageSource** source, **float** damage)</span> <br>Determines, using the input conditions, whether to apply the `DamageFunction` to the incoming damage of the `LivingEntity`. |

### 7. event.LivingEntityEvents.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Healed>`</span> | <span id="redCode">**ON_HEAL**</span> <br>Fired before `LivingEntity#heal`; allows the amount healed to be modified before healing happens. Setting the output to 0 is an unreliable way to negate incoming damage depending on other mods installed. Instead, use `SHOULD_HEAL`. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Heal>`</span> | <span id="redCode">**SHOULD_HEAL**</span> <br>Fired at the start of `LivingEntity#heal`, but before healing is applied. Can return false to cancel all healing, or true to allow it. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Tick>`</span> | <span id="redCode">**EVERY_SECOND**</span> <br>Fired once at the end of `LivingEntity#tick`, every 20 ticks (1 second). |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Damaged>`</span> | <span id="redCode">**ON_DAMAGE**</span> <br>Fired before `LivingEntity#damage`; allows the amount of damage to be modified before it is used in any way. Can be used to perform logic prior to the damage method, and can return the original damage to avoid modifying the value. The original value is the incoming damage, followed by the result of this event by any previous registries. Setting the output to 0 is an unreliable way to negate incoming damage depending on other mods installed. Instead, use `SHOULD_DAMAGE`. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<Damage>`</span> | <span id="redCode">**SHOULD_DAMAGE**</span> <br>Fired after: <br>`LivingEntity#isInvulnerableTo`<br>`World#isClient`<br>`LivingEntity#isDead`<br>`DamageSource#isFire` and `LivingEntity#hasStatusEffect`<br>`LivingEntity#isSleeping`<br>but before all other logic is performed. Can be used to cancel the method and prevent damage from being taken by returning false. Returning true allows the logic to continue. |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Healed** {...}</span> <br>Nested functional interface for `ON_HEAL` . |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Heal** {...}</span> <br>Nested functional interface for `SHOULD_HEAL` . |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Tick** {...}</span> <br>Nested functional interface for `EVERY_SECOND` . |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Damaged** {...}</span> <br>Nested functional interface for `ON_DAMAGE` . |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**Damage** {...}</span> <br>Nested functional interface for `SHOULD_DAMAGE` . |

### 8. event.PlayerEntityEvents.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<AttackCritDamage>`</span> | <span id="redCode">**ON_CRIT**</span> <br>Fired after the player lands a critical hit. The result is the damage. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`Event<AttackCritBoolean>`</span> | <span id="redCode">**SHOULD_CRIT**</span> <br>Fired when determining if the player's attack is critical. Return true if it is critical, return false if it is not. |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**AttackCritDamage** {...}</span> <br>Nested functional interface for `ON_DAMAGE` . |
| <span id="redType">`public`</span><br><span id="redType">`interface`</span> | <span id="redCode">**AttackCritBoolean** {...}</span> <br>Nested functional interface for `SHOULD_DAMAGE` . |

### 9. EntityAttributeSupplier.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`private`</span><br><span id="redType">`final`</span><br><span id="redType">`Identifier`</span> | <span id="redCode">**identifier**</span> <br>The attribute's identifier (data does not change, objects do). |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**of**(**Identifier** registryKey)</span> <br>Creates a new `EntityAttributeSupplier` object. |
| <span id="redType">`public`</span><br><span id="redType">`Identifier`</span> | <span id="redCode">**getId**()</span> <br>Returns the supplier's attribute registry key. |
| <span id="redType">`public`</span><br><span id="redType">`EntityAttribute`</span> | <span id="redCode">**get**()</span> <br>Returns the mapped `EntityAttribute` object in the games attribute registry if it exists for this supplier's registry key; otherwise, returns null. |

### 10. ExAPI.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`String`</span> | <span id="redCode">**MODID**</span> <br>The PlayerEx mod id. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`String`</span> | <span id="redCode">**INTEGER_PROPERTY**</span> <br>Round value to integer property that is attached to some attributes. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`String`</span> | <span id="redCode">**PERCENTAGE_PROPERTY**</span> <br>Multiply value by property and append % symbol. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`String`</span> | <span id="redCode">**MULTIPLY_PROPERTY**</span> <br>Multiply value by property. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`UUID`</span> | <span id="redCode">**PLAYEREX_MODIFIER_ID**</span> <br>The UUID PlayerEx attribute modifiers use. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`CacheableValue<Integer>`</span> | <span id="redCode">**LEVEL_VALUE**</span> <br>The `CacheableValue` key for PlayerEx's player data.. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`ComponentKey<PlayerData>`</span> | <span id="redCode">**PLAYER_DATA**</span> <br>The Caridnal Components Key for PlayerEx player data. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**LEVEL**</span> <br>The level attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**CONSTITUTION**</span> <br>The constitution attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**STRENGTH**</span> <br>The strength attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**DEXTERITY**</span> <br>The dexterity attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**INTELLIGENCE**</span> <br>The intelligence attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**LUCKINESS**</span> <br>The luckiness attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**EVASION**</span> <br>The evasion attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**LIFESTEAL**</span> <br>The lifesteal attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**HEALTH_REGENERATION**</span> <br>The health regeneration attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**HEAL_AMPLIFICATION**</span> <br>The heal amplification attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**MELEE_CRIT_DAMAGE**</span> <br>The melee crit damage attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**MELEE_CRIT_CHANCE**</span> <br>The melee crit chance attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**RANGED_CRIT_DAMAGE**</span> <br>The ranged crit damage attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**RANGED_CRIT_CHANCE**</span> <br>The ranged crit chance attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**RANGED_DAMAGE**</span> <br>The ranged damage attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**FIRE_RESISTANCE**</span> <br>The fire resistance attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**FREEZE_RESISTANCE**</span> <br>The freeze resistance attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**LIGHTNING_RESISTANCE**</span> <br>The lightning resistance attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**POISON_RESISTANCE**</span> <br>The poison resistance attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**WITHER_RESISTANCE**</span> <br>The wither resistance attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**BREAKING_SPEED**</span> <br>The breaking speed attribute. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**REACH_DISTANCE**</span> <br>The reach distance attribute. This is not implemented by PlayerEx, but by JamieWhiteShirt's reach-entity-attributes. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`EntityAttributeSupplier`</span> | <span id="redCode">**ATTACK_RANGE**</span> <br>The attack range attribute. This is not implemented by PlayerEx, but by JamieWhiteShirt's reach-entity-attributes. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`ExConfig`</span> | <span id="redCode">**getConfig**()</span> <br>Access to PlayerEx's config. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`void`</span> | <span id="redCode">**registerDamageModification**(**DamagePredicate** predicate, **DamageFunction** function)</span> <br>Registers a damage modification condition that is applied to living entities under specific circumstances. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`void`</span> | <span id="redCode">**registerRefundCondition**(**BiFunction&lt;PlayerData, PlayerEntity&gt;** refundCondition)</span> <br>Registers a refund condition. Refund conditions tell the game what can be refunded and what the maximum number of refund points are for a given circumstance. |

### 11. ExConfig.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`boolean`</span> | <span id="redCode">**resetOnDeath**()</span> |
| <span id="redType">`boolean`</span> | <span id="redCode">**isAttributesGuiDisabled**()</span> |
| <span id="redType">`int`</span> | <span id="redCode">**skillPointsPerLevelUp**()</span> |
| <span id="redType">`int`</span> | <span id="redCode">**requiredXp**(**PlayerEntity** player)</span> |
| <span id="redType">`float`</span> | <span id="redCode">**levelUpVolume**()</span> |
| <span id="redType">`float`</span> | <span id="redCode">**skillUpVolume**()</span> |
| <span id="redType">`float`</span> | <span id="redCode">**textScaleX**()</span> |
| <span id="redType">`float`</span> | <span id="redCode">**textScaleY**()</span> |

### 12. PacketType.java

| Modifiers and Type | Method/Field and Description |
| ------------------ | ---------------------------- |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`PacketType`</span> | <span id="redCode">**DEFAULT**</span><br>Does nothing extra - just allows the server to modify PlayerEx's attribute modifiers. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`PacketType`</span> | <span id="redCode">**LEVEL**</span><br>Only allows modification if the player's experience levels are greater than or equal to the required xp to level up. Also removes those experience levels from the player and adds n skill points; where n is defined by config option skill points per level up. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`PacketType`</span> | <span id="redCode">**SKILL**</span><br>Only allows modification if the player's skill points are greater than or equal to 1. Also subtracts one skill point from the player. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`final`</span><br><span id="redType">`PacketType`</span> | <span id="redCode">**REFUND**</span><br>Only allows modification if the player's refund points are greater than or equal to 1. Also subtracts one refund point and adds one skill point. |
| <span id="redType">`public`</span><br><span id="redType">`static`</span><br><span id="redType">`PacketType`</span> | <span id="redCode">**fromId**(**byte** id)</span><br>Gets the correct `PacketType` from the input (or else `DEFAULT`). |
| <span id="redType">`public`</span><br><span id="redType">`byte`</span> | <span id="redCode">**id**()</span><br>Returns the `PacketType`'s id. |
| <span id="redType">`public`</span><br><span id="redType">`boolean`</span> | <span id="redCode">**test**(**MinecraftServer** server, **ServerPlayerEntity** player, **PlayerData** data)</span><br>Runs the test function. |

{% include links.html %}
