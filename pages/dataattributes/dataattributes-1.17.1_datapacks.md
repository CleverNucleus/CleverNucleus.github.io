---
title: "Datapacks"
keywords: dataattributes data attributes 1.17.1 datapacks
tags: [data_attributes, 1.17.1]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.17.1_datapacks.html
summary: This section provides an overview of the datapack structure used by Data Attributes for Minecraft 1.17.1.
---

{% include important.html content="This section is now outdated and unsupported. Click [here](dataattributes-1.18.2_home) for the latest release." %}

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

Before creating a datapack with Data Attributes, or using Data Attributes as a mod dependency, please familiarise yourself with [Overrides](dataattributes-1.17.1_overrides), [Entity Types](dataattributes-1.17.1_entity_types), [Attribute Functions](dataattributes-1.17.1_attribute_functions) and [Attribute Properties](dataattributes-1.17.1_attribute_properties).

{% include links.html %}
