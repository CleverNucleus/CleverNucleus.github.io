---
title: "Stacking Behaviour"
keywords: dataattributes data attributes 1.18.1 stacking behaviour
tags: [data_attributes, 1.18.1]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.18.1_stacking_behaviour.html
summary: "This section covers Stacking Behaviour, including: what it means; what it does; and how it's different from previous releases."
---

{% include important.html content="This section is now outdated and unsupported. Click [here](dataattributes-1.18.2_home) for the latest release." %}

## Overview

Stacking Behaviour is a reworked implementation of the 1.17.x [Adding Behaviour](dataattributes-1.17.1_adding_behaviour.html) that was utilised when defining [Attribute Functions](dataattributes-1.17.1_attribute_functions.html), using the `behaviour` tag. Instead of defining this property as an attribute function however, it now serves as an attribute-specific instruction on addition from different sources. That is to say, how do different sources of an attribute stack up?

### 1. FLAT

The `FLAT` stacking behaviour describes normal addition and functions as:

$$a+b=r$$

Where $$a$$ and $$b$$ are the attribute values and $$r$$ is the result. For example, you have an Iron Helmet that provides +2 armor and an Iron Chestplate that provides +6. When added together you get 8 armor:

$$2+6=8$$

### 2. DIMINISHING

The `DIMINISHING` stacking behaviour describes a non-linear addition function, and only works for attributes with a minimum value of between -1 and 0, and a maximum value of 1 (e.g. percentage based attributes would tend to use this). The *exact* maths behind this is slightly complex, but it can be broadly summarised as the following:

$$1-((1-x_1)\times(1-x_2)\times\cdots(1-x_n))=r$$

Where $$x_1$$, $$x_2$$ and $$x_n$$ are source 1, source 2 and source n, respectively. For example, you have an Iron Helmet that provides +20% Evasion and an Iron Chestplate that provides +30% Evasion. When added together you would expect 50% Evasion with normal `FLAT` addition, but with `DIMINISHING` you get 44% Evasion:

$$1-((1-0.2)\times(1-0.3))=0.44$$

## Subtraction

How subtraction works is much the same, but values are instead treated as absolute and the result is subtracted from the positive result (above). It should be noted that while this is greate for attributes with a range of 0% - 100%, or -100% to 100%, you have to change the precision of your attribute's value to deatl with percentages above 100%. For example, if you wanted a critical strike damage attribute that went from 0% - 500%, you would use attribute values of 0 - 0.5, and then multiply the results accordingly.

{% include links.html %}
