---
title: "In-game Content"
keywords: playerex player ex 1.18.2 1.19 1.19.1 1.19.2 in game content in-game attributes config commands
tags: [getting_started, playerex, 1.18.2, 1.19, 1.19.1, 1.19.2]
sidebar: playerex_sidebar
permalink: playerex-1.18.2_content.html
summary: "PlayerEx adds new attributes to the game, that are by default attached to the player. Additionally, PlayerEx features a new GUI screen and levelling system."
---

## Attributes

PlayerEx adds fully functional attributes to the game. By default these attributes are only present on the player, but attributes with the **Applicable Entity** type `LivingEntity` can be attached to any living entity with datapacks - see [Data Attributes](dataattributes-1.18.2_datapacks).

| Attribute<br>Registry Key | Applicable Entity | Stacking Behaviour | Description |
| :-------- | :---------------: | :----------------: | :---------- |
| Evasion<br><span id="redType">`playerex:evasion`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`DIMINISHING`</span> | The chance to dodge projectiles. |
| Lifesteal<br><span id="redType">`playerex:lifesteal`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`DIMINISHING`</span> | A percentage of melee and ranged damage dealth is healed. |
| Health Regeneration<br><span id="redType">`playerex:health_regeneration`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`DIMINISHING`</span> | Health passively healed every second. |
| Heal Amplification<br><span id="redType">`playerex:heal_amplification`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`DIMINISHING`</span> | All healing is amplified by this amount. |
| Melee Crit Damage<br><span id="redType">`playerex:melee_crit_damage`</span> | <span id="redType">`PlayerEntity`</span> | <span id="redType">`DIMINISHING`</span> | Attack damage is multiplied by this amount on a melee critical hit. |
| Melee Crit Chance<br><span id="redType">`playerex:melee_crit_chance`</span> | <span id="redType">`PlayerEntity`</span> | <span id="redType">`DIMINISHING`</span> | The chance for a melee attack to be a critical hit. |
| Ranged Crit Damage<br><span id="redType">`playerex:ranged_crit_damage`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`DIMINISHING`</span> | Projectile damage is multiplied by this amount on a rangd critical hit. |
| Ranged Crit Chance<br><span id="redType">`playerex:ranged_crit_chance`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`DIMINISHING`</span> | The chance for a projectile fired to result in a critical hit. |
| Ranged Bonus Damage<br><span id="redType">`playerex:ranged_damage`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`FLAT`</span> | Damage added to projectile base damage. |
| Fire Resistance<br><span id="redType">`playerex:fire_resistance`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`DIMINISHING`</span> | Reduces fire damage by this amount (100% is immunity). |
| Freeze Resistance<br><span id="redType">`playerex:freeze_resistance`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`DIMINISHING`</span> | Reduces freeze damage by this amount (100% is immunity). |
| Lightning Resistance<br><span id="redType">`playerex:lightning_resistance`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`DIMINISHING`</span> | Reduces lightning damage by this amount (100% is immunity). |
| Poison Resistance<br><span id="redType">`playerex:poison_resistance`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`DIMINISHING`</span> | Reduces poison damage by this amount (100% is immunity). |
| Wither Resistance<br><span id="redType">`playerex:wither_resistance`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`DIMINISHING`</span> | Reduces wither damage by this amount (100% is immunity). |
| Breaking Speed<br><span id="redType">`playerex:breaking_speed`</span> | <span id="redType">`PlayerEntity`</span> | <span id="redType">`FLAT`</span> | Defines the player's base block breaking speed. |
| Level<br><span id="redType">`playerex:level`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`FLAT`</span> | An RPG like level value attribute; does nothing. |
| Constitution<br><span id="redType">`playerex:constitution`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`FLAT`</span> | An RPG like stat value used to increase other attributes: <br>+1 Max Health <br>+0.1 Knockback Resistance <br>+0.1 Poison Resistance |
| Strength<br><span id="redType">`playerex:strength`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`FLAT`</span> | An RPG like stat value used to increase other attributes: <br>+0.25 Melee Attack Damage <br>+0.5 Armor <br>+0.01 Health Regen./s |
| Dexterity<br><span id="redType">`playerex:dexterity`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`FLAT`</span> | An RPG like stat value used to increase other attributes: <br>+0.1 Attack Speed <br>+0.25 Ranged Damage <br>+5% Melee Crit Damage <br>+0.1 Lightning Resistance |
| Intelligence<br><span id="redType">`playerex:intelligence`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`FLAT`</span> | An RPG like stat value used to increase other attributes: <br>+2% Heal Amplification <br>+5% Ranged Crit Damage <br>+0.1 Wither Resistance |
| Luckiness<br><span id="redType">`playerex:luckiness`</span> | <span id="redType">`LivingEntity`</span> | <span id="redType">`FLAT`</span> | An RPG like stat value used to increase other attributes: <br>+0.1 Luck <br>+2% Evasion <br>+2% Melee Crit Chance <br>+2% Ranged Crit Chance |

## Modified Vanilla Attributes

| Attribute<br>Registry Key | Modification |
| Armor<br><span id="redType">`minecraft:generic.armor`</span> | Increased max value from 30 to <span id="redType">`Integer.MAX_VALUE`</span>. |
| Armor Toughness<br><span id="redType">`minecraft:generic.armor_toughness`</span> | Increased max value from 20 to <span id="redType">`Integer.MAX_VALUE`</span>. |
| Knockback Resistance<br><span id="redType">`minecraft:generic.knockback_resistance`</span> | Changed stacking behaviour from <span id="redType">`FLAT`</span> (default) to <span id="redType">`DIMINISHING`</span>. |
| Max Health<br><span id="redType">`minecraft:generic.max_health`</span> | Changed max value from 1024 to <span id="redType">`Integer.MAX_VALUE`</span>. |

## GUI

PlayerEx adds tabs to the survival inventory that let the player navigate between their inventory, attributes screen and combat attributes screen. The latter of which allow the player to view their attributes and spend skill points to level up and increase certain attributes. These have an RPG theme.

<div style="text-align: center">

{% include image.html file="inventory_tabs.png" alt="inventory tabs" caption="Added inventory tabs to the survival inventory." max-width="600" %}

</div>

{% include note.html content="The attributes screen can also be accessed using a hotkey: by default this is R." %}

## Config

Most of PlayerEx's configurability is provided by Data Attributes in the form of datapacks, but there are some items that have a dedicated config - this is split into server and client items. The former requires restarting/reloading the world.

| Item | Side | Function |
| :--- | :--: | :------- |
| Reset On Death | <span id="redType">`SERVER`</span> | On death, the player's attributes, level and skill points revert to their defaults. |
| Disable Attributes GUI | <span id="redType">`SERVER`</span> | Hides inventory tabs, disables opening of/access to the attributes screen. |
| Show Level Nameplates | <span id="redType">`SERVER`</span> | For every <span id="redType">`LivingEntity`</span> with the Level attribute, a nameplate displays above that entity rendering their level (for players on multiplayer this is just above the head but below the nametag, such that it does not interfere with health bar mods). |
| Skill Points per Level | <span id="redType">`SERVER`</span> | How many Skill Points the player gets for each level up. |
| Level Up Formula | <span id="redType">`SERVER`</span> | Dictates how many experience levels are required for the player to level up. |
| Restorative Force Ticks | <span id="redType">`SERVER`</span> | The number of ticks between every restorative event. Note that 20 ticks is 1 second. |
| Restorative Force | <span id="redType">`SERVER`</span> | The counter-balancing multiplier that acts to restore the XP Negation Factor to 1.0. |
| XP Negation Factor | <span id="redType">`SERVER`</span> | The chance for xp orbs to drop in a given chunk. Setting this value to 100 causes vanilla behaviour. |
| Level Up Volume | <span id="redType">`CLIENT`</span> | How loud the level up sound effect is (can be set to 0 to mute). |
| Skill Up Volume | <span id="redType">`CLIENT`</span> | How loud the spend skill point sound effect is (can be set to 0 to mute). |
| Horizontal Text Scale | <span id="redType">`CLIENT`</span> | How squished the text on the attributes/combat screen is in the horizontal direction (good for languages with long form translations). |
| Vertical Text Scale | <span id="redType">`CLIENT`</span> | How squished the text on the attributes/combat screen is in the vertical direction (good for languages with long form translations). |
| Tooltip Attributes | <span id="redType">`CLIENT`</span> | How weapon tooltips should display their Attack Damage/Speed attributes. By default, increasing your attack damage/speed is not reflected in the tooltip of these attributes - PlayerEx fixes this, but offers the option to choose between different solutions: <br>**1.** Does not fix the issue and leaves tooltips alone, i.e. if you had 5 attack damage, the tooltip of a sword would not show this. This option is available in the event that you are playing with another mod that changes tooltips, and would otherwise be incompatible.). <br>**2.** Displays attack damage/speed the same way attributes are displayed on tooltips, and only shows modifier values. <br>**3.** Displays the attack damage/speed in the vanilla way, but fixes the values so that they reflect the player's values. |

## Commands

| Command | Function |
| :------ | :------- |
| <span id="redType">`/playerex levelup <player> [amount]`</span> | Levels the input player up by the input amount. |
| <span id="redType">`/playerex refund <player> [amount]`</span> | Gives the input player the input number of refund points. |
| <span id="redType">`/playerex reset <player>`</span> | Resets all attributes and skill points on the input player to their defaults. |
| <span id="redType">`/playerex skillAttribute <player> <attribute> <requires skill points>`</span> | Skills the input attribute, provided the player has a Skill Point available if the final boolean argument is true, or regardless otherwise. |
| <span id="redType">`/playerex refundAttribute <player> <attribute> <requires refund points>`</span> | Refunds the input attribute, provided the player has a Refund Point available if the final boolean argument is true, or regardless otherwise. |

{% include note.html content="Also see commands available with [Offline Player Cache](opc-1.18.2_home#commands), which is included with PlayerEx." %}

{% include links.html %}
