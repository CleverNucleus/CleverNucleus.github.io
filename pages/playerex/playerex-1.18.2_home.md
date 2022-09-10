---
title: "Home"
keywords: home homepage playerex player ex 1.18.2 1.19 1.19.1 1.19.2 quick links
tags: [introduction, playerex, 1.18.2, 1.19, 1.19.1, 1.19.2]
sidebar: playerex_sidebar
permalink: playerex-1.18.2_home.html
summary: "PlayerEx is a Minecraft mod built using the Fabric ecosystem that adds RPG themed attributes to the game and the player."
---

<img src="https://github.com/CleverNucleus/PlayerEx/blob/master/img/logo.png?raw=true" alt="PlayerEx" height="50%" />

<img src="https://img.shields.io/github/license/CleverNucleus/PlayerEx?style=flat-square&color=367DBB" alt="License" />
<img src="https://img.shields.io/badge/dynamic/json?color=EC1F52&label=Applicable Versions&prefix=%20&query=2.releases&url=https://raw.githubusercontent.com/CleverNucleus/PlayerEx/master/versions.json&style=flat-square" alt="Applicable Versions" />

## Welcome

This wiki covers the following:

- Adding PlayerEx to your project.
- The in-game content PlayerEx provides.
- TextPlaceholderAPI and included placeholders.
- FAQ, examples and mini-tutorials
- API contents and usage.

## Integrating PlayerEx

PlayerEx has a [Curseforge](https://www.curseforge.com/minecraft/mc-mods/playerex) and [Modrinth](https://modrinth.com/mod/playerex) page. To add PlayerEx to your project, insert the following into your `build.gradle`:

<ul id="profileTabs" class="nav nav-tabs">
    <li class="active"><a href="#modrinth" data-toggle="tab">Modrinth</a></li>
    <li><a href="#curseforge" data-toggle="tab">Curseforge</a></li>
</ul>
<div class="tab-content">
<div role="tabpanel" class="tab-pane active" id="modrinth">

{% highlight gradle %}

repositories {
    maven {
        name = "Modrinth"
        url = "https://api.modrinth.com/maven"
        content {
            includeGroup "maven.modrinth"
        }
    }
}

{% endhighlight %}

{% highlight gradle %}

dependencies {
    modImplementation "maven.modrinth:playerex:<version>"
}

{% endhighlight %}

</div>
<div role="tabpanel" class="tab-pane" id="curseforge">

{% highlight gradle %}

repositories {
    maven {
        name = "Curseforge"
        url = "https://www.cursemaven.com"
    }
}

{% endhighlight %}

{% highlight gradle %}

dependencies {
    modImplementation "curse.maven:playerex-409221:<fileId>"
}

{% endhighlight %}

The last number after the colon refers to the file id; you should always try to use the latest file.

</div>
</div>

{% include note.html content="PlayerEx has multiple dependencies, so your `build.gradle` must accommodate this." %}

### Dependencies

PlayerEx requires the following depdencies:

| Dependency | Purpose |
| :--------- | :------ |
| [Fabric API](https://github.com/FabricMC/fabric) | Required by other dependencies and for certain events. |
| [Data Attributes](https://github.com/CleverNucleus/Data-Attributes) | Provides the core attribute system and attribute configurability. |
| [Cardinal Components API](https://github.com/OnyxStudios/Cardinal-Components-API) (Base & Entity) | A jar-in-jar dependency that provides player attribute modifiers. |
| [Cloth Config API](https://github.com/shedaniel/cloth-config) | Provides configurability. |
| [Offline Player Cache](https://github.com/CleverNucleus/Offline-Player-Cache) | A jar-in-jar dependency that provides TextPlaceholderAPI with persistent player data. |
| [Text Placeholder API](https://github.com/Patbox/TextPlaceholderAPI) | A jar-in-jar dependency that provides text placeholders, namely for custom multiplayer leaderboards. |
| [exp4j](https://www.objecthunter.net/exp4j/) | A jar-in-jar dependency that provides levelling configurability, namely an expression parser. |

{% highlight gradle %}

repositories {
	mavenCentral()
	maven {
		name = "Ladysnake"
		url = "https://ladysnake.jfrog.io/artifactory/mods"
	}
	maven {
		name = "Shedaniel"
		url = "https://maven.shedaniel.me/"
	}
	maven {
		name = "TerraformersMC"
		url = "https://maven.terraformersmc.com"
	}
	maven {
		name = "Nucleoid"
		url = "https://maven.nucleoid.xyz/"
	}
	maven {
		name = "Modrinth"
		url = "https://api.modrinth.com/maven"
		content {
			includeGroup "maven.modrinth"
		}
	}
}

{% endhighlight %}

{% include links.html %}
