---
title: "Text Placeholder API"
keywords: playerex player ex 1.18.2 1.19 1.19.1 1.19.2 text placeholder api
tags: [playerex, 1.18.2, 1.19, 1.19.1, 1.19.2]
sidebar: playerex_sidebar
permalink: playerex-1.18.2_textplaceholderapi.html
summary: "Since version 3.0.2, PlayerEx supports Text Placeholder API and includes three text placeholders."
---

## Provided Placeholders

| Placeholder | Online Status | Result |
| :---------- | :------------ | :----- |
| <span id="redType">`%playerex:level%`</span> | Online Players Only | This displays the player's current level attribute value. |
| <span id="redType">`%playerex:name_top/x%`</span> | Offline and Online Players Only | This displays the player's name whose level is the highest in the current world; where <span id="redType">`x`</span> represents the relative position of the player in terms of level. For example: for a player with the highest level, <span id="redType">`x`</span> would be 1; for the player with the second highest level, <span id="redType">`x`</span> would be 2, and so on. The name displayed is provided by <span id="redType">`GameProfile#getName`</span>. |
| <span id="redType">`%playerex:level_top/x%`</span> | Offline and Online Players Only | This displays the player's level whose level is the highest in the current world; where <span id="redType">`x`</span> represents the relative position of the player in terms of level. For example: for a player with the highest level, <span id="redType">`x`</span> would be 1; for the player with the second highest level, <span id="redType">`x`</span> would be 2, and so on. The value displayed is provided by <span id="redType">`LivingEntity#getAttributeValue`</span>. |

## Notes

When a player leaves a world (disconnects), PlayerEx caches that player's `UUID`, `Name` and `Level`. These values are used by the server when providing placeholders for offline players, whereas for online players the current/life value is provided instead of the cached one. Upon joining a world (reconnecting) that player's cache is removed and their current/live values are used once again.

This offline caching functionality is provided by [Offline Player Cache](opc-1.18.2_home) - a lighweight library/API mod. PlayerEx ships with it included (jar-in-jar). There may be circumstances where a player has quit a world/server but their cache is still there. To remove their cache, or even just their cached `Name` or `Level`, please see [here](opc-1.18.2_home#commands).

{% include note.html content="Also see [Text Placeholder API](https://github.com/Patbox/TextPlaceholderAPI)" %}

{% include links.html %}
