---
title: "Datapacks"
keywords: dataattributes data attributes 1.18.2 1.19 1.19.1 1.19.2 datapacks
tags: [data_attributes, 1.18.2, 1.19, 1.19.1, 1.19.2]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.18.2_datapacks.html
summary: This section provides an overview of the datapack structure used by Data Attributes for Minecraft 1.18.2.
---

## Structure

Data Attributes works using Minecraft's data resource system. The file hierarchy for this is `data/namespace/attributes`. Where `namespace` is either the default `minecraft` or any given modid. By default, nothing exists in this directory, as all json data is optional. However, a fully populated directiory contains the files `entity_types.json`, `functions.json`, `properties.json` and `/overrides/` (which is a folder). This takes the form shown below:

```
📂data
 ┗📂<namespace>
   ┗📂attributes
     ┣📂overrides
     ┃ ┣📄attribute_name.json
     ┃ ┗...
     ┃
     ┣📄entity_types.json
     ┣📄functions.json
     ┗📄properties.json
```

Before creating a datapack with Data Attributes, or using Data Attributes as a mod dependency, please familiarise yourself with [Overrides](dataattributes-1.18.2_overrides), [Entity Types](dataattributes-1.18.2_entity_types), [Attribute Functions](dataattributes-1.18.2_attribute_functions) and [Attribute Properties](dataattributes-1.18.2_attribute_properties).

{% include links.html %}
