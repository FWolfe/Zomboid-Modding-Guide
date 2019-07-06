# Project Zomboid Modding Guide

**_This document is very much a Work-In-Progress._**

**_The goal is to create a solid 1-stop spot for how to mod TheIndieStone's Project Zomboid, covering all aspects of modding, tools required, tips and tricks, as well as examples and code snippits for the more common tasks._**

**_Much of the current modding information is in outdated tutorials, scattered in forum posts, and locked away in the brains of modders and other community members. Ideally by hosting a universal guide on Github, it will be collected all in one spot and anyone can contribute new information, corrections or updates as PZ's API changes over time._**

## Intro
**This section should provide a basic description of the various areas of modding, and basic hints as to difficulty and tools required**

"Modding" is a vague term covering multiple areas. Many of these areas sometimes overlap but should be considered separate as they require different skill sets and knowledge.

### Adding basic items and recipes
This can be done with little (or none) coding knowledge and minimal tools. Item and recipe definitions are inside standard .txt documents, although these txt files must conform to a specific syntax and style.

### Code changes, advanced items and recipes
Being able to add on to or edit Zomboid's code is a major bonus, and where the real power of mods comes in. PZ is coded in a mixture of Java (the main engine) and Lua (the moddable components).
Knowledge of the Lua scripting language is HIGHLY advised. You'll save yourself a lot of grief and effort learning Lua before diving into this area. Fortunately Lua is a simple language by design, and relatively easy to learn. The internet is scattered with tutorials.

### 3d Models

### Textures

### Map Making


----------------------------------------------------------------------------------

## Tools (software)
**_This section should describe the various software tools required for various areas, and provide url links for the more common software used_**

For the most part, you are free to use what ever tools you want. The exception is custom map creation, where you are currently limited to the tools released by TIS.

### Text Editors:
Used in all areas of modding. Notepad++ is often the preferred editor for windows OS's (it also runs amazingly well on linux with wine)

### 3d Modelling:
Used for the creation of custom items, vehicles and weapons. Blender is the common choice as its free, and import/export scripts for PZ's model format exist. Any 3d app can be used to make your models, with blender doing the final conversion.

### Image Editor:
If your doing custom icons, textures, maps or 3d models you'll need one of these. Paint.NET, Photoshop and GIMP are the most used, but whatever supports the .png format will work.

### Custom Mapping:


----------------------------------------------------------------------------------

## Mod structure
**_Describe the various files and folders that make up a mod, and their purpose_**

#### mod.info (required file)
This file can be created/editted with any text editor. It contains details like your mod's ID, name, description, preview image etc.
```
name=My First Mod
poster=poster.png
id=MyFirst
description=Basic example mod
url=https://theindiestone.com/forums/
```

#### media/ (required folder)
The actual content of your mod will be placed in subfolders of the `media` folder

#### media/scripts (optional folder)
This folder contains text files with item, vehicles and recipe definitions. See the section on Scripts for more information.

#### media/models (optional folder)
Used for 3d models of weapons, vehicles etc.

#### media/textures (optional folder)

#### media/sounds (optional folder)

#### media/lua (optional folder) required for lua scripts

#### media/lua/client

#### media/lua/shared

#### media/lua/server


----------------------------------------------------------------------------------

## The Scripts
**_Describe the various types of blocks in the scripts/*.txt files, such as module, item, recipe and the key=value types._**

### The module block

### The item block

### The recipe block


----------------------------------------------------------------------------------

## The Code

### Introduction
**_Basic lua intro and tutorial (not pz specific) with links to more advanced lua tutorials online._**

Project Zomboid's code is a 2-language system, using both Java and Lua. The main engine and API features are primarly Java,
with large chunks of the logic in Lua.  Most modding is done to the Lua component, while the Java component is generally considered non-moddable.

_Note: The java can be modified, but requires knowlage of java, decompiling and recompiling the .class files. Mods with Java edits also can not be installed using normal methods, these files need to go directly to zomboid's install directory. Modifing the Java is beyond the scope of this document._

Unlike the Java, Lua can be editted with the text editor of your choice and requires no other tools. _Those familiar with basic Lua can skip to the next section._


### Zomboid's Lua Component
**_Describe pz lua/kahlua component, how it interacts with the java, and how lua is used in pz in general._**

### The Vanilla Lua
**_Brief outline of what aspects of the game are controlled by lua, and where these aspects can be found in the files._**

### Zomboid's API
**_Describe major parts of the api, common globals and the event system._**

### Decompiling The Java
**_Brief outline of installing and working with a decompiler._**

### Performance Tips

### Code Snippits


----------------------------------------------------------------------------------

## The Maps
**_Describe the tools and process for creating custom maps._**


----------------------------------------------------------------------------------

## The Models
**_Describe process for loading importing and loading custom 3d models in game._**
