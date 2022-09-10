---
title: "Adding Behaviour"
keywords: dataattributes data attributes adding behaviour 1.17.1 flat diminishing
tags: [data_attributes, 1.17.1]
sidebar: dataattributes_sidebar
permalink: dataattributes-1.17.1_adding_behaviour.html
summary: Data Attributes adds Adding Behaviours to attributes. This section briefly outlines their use.
---

{% include important.html content="This section is now outdated and unsupported. Click [here](dataattributes-1.18.2_home) for the latest release." %}

## Overview

An adding behaviour simply describes how two sources of an attribute will add together. Implemented with Data Attributes are `FLAT` and `DIMINISHING`, as follows:

### 1. FLAT

The `FLAT` adding behaviour describes normal addition and functions as:

$$a+b=r$$

Where $$a$$ and $$b$$ are the attribute values and $$r$$ is the result. For example, you have an Iron Helmet that provides +2 armor and an Iron Chestplate that provides +6. When added together you get 8 armor:

$$2+6=8$$

### 2. DIMINISHING

The `DIMINISHING` adding behaviour describes a non-linear addition function as:

$$c\frac{a+b}{a+c}=r$$

and for subtraction

$$a+b-\frac{a\cdot b}{c}=r$$

Where $$a$$, $$b$$, $$c$$ and $$r$$ are the current value, adding/subtracting value, maximum attribute value and result, respectively. Note how unlike `FLAT` addition, symmetry between arguments is no longer present i.e. $$a$$ and $$b$$ can no longer be interchanged and they no longer just represent two values, but rather the current and modifying value. Hence, the order of $$a$$ and $$b$$ are important. Also of note is the introduction of a third argument, $$c$$, the maximum attribute value. This property allows the result to be scaled such that the maximum value can never truly be reached, hence the term diminishing.

Using the same items as before (+2 armor Iron Helmet and +6 armor Iron Chestplate), lets see what the total armor becomes under a `DIMINISHING` behaviour:

$$30\times\frac{2+6}{30+6}=6.67$$

The result is greater than the largest argument (in this case the adding value), but smaller than the maximum value. It is also less than adding using `FLAT` behaviour. This behaviour can be useful for percentile attributes (evasion, for example) that only stay within the bounds 0 - 1.

Some assumptions that we have made for this example:

- The player *currently* has the +2 armor Iron Helmet equipped, and is in the process of equipping the +6 armor Iron Chestplate; in other words, 2 is the *current value* and 6 is the *adding value*.
- The maximum value for armor is 30 (the vanilla default).

## Note
All vanilla attributes have the `FLAT` adding behaviour by default. For the subtraction equation, we ensure that reversibility is maintained with the addition equation. That is to say that if $$a$$ is the current value and $$b$$ is the subtracting value, the equation takes the following form when considering the aforementioned example values:

$$6.67+(-6)-\frac{6.67\cdot (-6)}{30}=2$$

{% include links.html %}
