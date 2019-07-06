# Project Zomboid Modding Guide

**_This document is very much a Work-In-Progress._**

**_The goal is to create a solid 1-stop spot for how to mod TheIndieStone's Project Zomboid, covering all aspects of modding, tools required, tips and tricks, as well as examples and code snippets for the more common tasks._**

**_Much of the current modding information is in outdated tutorials, scattered in forum posts, and locked away in the brains of modders and other community members. Ideally by hosting a universal guide on Github, it will be collected all in one spot and anyone can contribute new information, corrections or updates as PZ's API changes over time._**

## Intro
**This section should provide a basic description of the various areas of modding, and basic hints as to difficulty and tools required**

"Modding" is a vague term covering multiple areas. Many of these areas sometimes overlap but should be considered separate as they require different skill sets and knowledge.

### Adding basic items and recipes
This can be done with little (or none) coding knowledge and minimal tools. Item and recipe definitions are inside standard .txt documents, although these txt files must conform to a specific syntax and style.  

### Code changes, advanced items and recipes
Being able to add on to or edit Zomboid's code is a major bonus, and where the real power of mods comes in. PZ is coded in a mixture of Java (the main engine) and Lua (the moddable components).  
Knowledge of the Lua scripting language is HIGHLY advised. You'll save yourself a lot of grief and effort learning Lua before diving into this area. Fortunately Lua is a simple language by design, and relatively easy to learn.  
The internet is scattered with tutorials.

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

#### media/
*required folder*  
The actual content of your mod will be placed in subfolders of the `media` folder

#### media/scripts/
*optional folder*  
This folder contains text files with item, vehicles and recipe definitions. See the section on Scripts for more information.

#### media/models/
*optional folder*  
Used for 3d models of weapons, vehicles etc.

#### media/textures/
*optional folder*  

#### media/sounds/
*optional folder*  

#### media/lua/
*optional folder but required for lua scripts*  
If your mod includes any .lua scripts, you will need this folder as well as at least one of the subfolders listed below. Any .lua files need to go in the subfolders, not directly in this one.

#### media/lua/client/
*optional folder*  
Used for client-side scripts. UI elements, context menus timed actions and the like.

#### media/lua/shared/
*optional folder*  

#### media/lua/server/
*optional folder*  


----------------------------------------------------------------------------------

## The Scripts
Items, recipes, vehicles and similar stuff is defined in .txt files in the `media/scripts/` directory. These files are seperated into various `{ block }` types. Within these blocks exist items properties: its name, how much it weighs, what type of item it is, how much time recipes take, what ingredients are required, how much horsepower a vehicle has, etc.

Each of the various block types has different attribute/value types, and syntax can vary slightly from other block types (items vs recipes), but the overall format is a same:

A basic example:
```
module MyMod {
    imports {
        Base
    }
    item MyItem
    {
        Type         = Normal,
        DisplayName  = My First Item,
        Icon         = MyIcon,
        Weight       = 0.1,
    }
}
```

### The module block
In the example above, you'll notice the file starts off with `module MyFirst`. All items, recipes and other blocks need to be contained within the module block.  
The name used is used as a prefix for a item's full name: The item in the example is `MyMod.MyItem`.
Most (not all) vanilla items use the module `Base`. You are free to use this module as well but be aware defining things like items that have already exist in the `Base` module you will **overwrite** them.  
If you have lots of items or don't want to conflict with any existing items or other mods its best to use a custom (or multiple) module names. Sometimes for basic mods or compatibility reasons it may be more better to use a existing module name such as `Base`. Creating a extra module namespace for a few simple items is not always the best option: unnecessary  modules are very minor performance hit.

### The imports block
Often for recipes (and some other blocks) you will need to reference items from other modules. Normally when a item exists in a different script module, you have to use the full name such as `Base.Nails` to reference it. By declaring a `imports` block you can skip the module prefix on the item and just use `Nails`
```
imports {
    Base
}
```
*Note: when loading mods scripts, PZ will log a warning to the console if you do not import Base. This warning can be ignored if not-importing was intentional.*


### The item block

### The recipe block

### The evolvedrecipe block

### The fixing block

### The sound block

### The vehicle block

----------------------------------------------------------------------------------

## The Code

### Introduction
Project Zomboid's code is a 2-language system, using both Java and Lua. The main engine and API features are primarily Java,
with large chunks of the logic in Lua.  Most modding is done to the Lua component, while the Java component is generally considered non-moddable.

_Note: The java can be modified, but requires knowledge of java, decompiling and recompiling the .class files. Mods with Java edits also can not be installed using normal methods, these files need to go directly to Zomboid's install directory. Modifying the Java is beyond the scope of this document._

Unlike the Java, Lua can be edited with the text editor of your choice and requires no other tools. _Those familiar with basic Lua can skip ahead._

Lua is designed to be a simple and lightweight language, without a lot of bells and whistles. It is easy to learn, its syntax is simple and there are a minimal number of built-in functions and modules.  It does include some concepts that can throw off people coming from other languages (ordered lists/arrays, unordered lists/maps/dicts and objects in Lua are all the same thing: a table)

**_Include a brief lua tutorial here, and links to more online._**

### Zomboid's Lua Component
Those familiar with Lua know there can be minor differences between versions (5.1, 5.2, 5.3) primarly in the modules and methods contained. Zomboid's Lua is not 'pure' Lua, it is modified Kahlua, a Lua interpreter writen entirely in Java. It
lacks the performance of pure lua, but provides a almost seamless integration of the Java and Lua components. Not all Lua modules are implemented such as `io.*` and `os.*`  
Java classes and functions are 'exported' to Lua providing normal access to them, however things like java reflection will not work.



### The Vanilla Lua
**_Brief outline of what aspects of the game are controlled by lua, and where these aspects can be found in the files._**

### Zomboid's API
**_Describe major parts of the api, common globals and the event system._**

### Decompiling The Java
**_Brief outline of installing and working with a decompiler._**

### Performance Tips

### Code Snippets


----------------------------------------------------------------------------------

## The Maps
**_Describe the tools and process for creating custom maps._**


----------------------------------------------------------------------------------

## The Models
**_Describe process for loading importing and loading custom 3d models in game._**
