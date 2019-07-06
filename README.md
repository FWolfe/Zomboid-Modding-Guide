# Project Zomboid Modding Guide

**_This document is very much a Work-In-Progress._**

**_The goal is to create a solid 1-stop spot for how to mod TheIndieStone's Project Zomboid, covering all aspects of modding, tools required, tips and tricks, as well as examples and code snippets for the more common tasks._**

**_Much of the current modding information is in outdated tutorials, scattered in forum posts, and locked away in the brains of modders and other community members. Ideally by hosting a universal guide on Github, it will be collected all in one spot and anyone can contribute new information, corrections or updates as PZ's API changes over time._**

## Intro
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
This section contains a brief description of the file and folder structure that makes up a mod. Your mod may include other folders in addition to these, but these ones are the most common.

#### mod.info
This file can be created/edited with any text editor. It contains details like your mod's ID, name, description, preview image etc.
```
name=My First Mod
poster=poster.png
id=MyFirst
description=Basic example mod
url=https://theindiestone.com/forums/
```

#### media/
The actual content of your mod will be placed in subfolders of the `media` folder

#### media/scripts/
This folder contains text files with item, vehicles and recipe definitions. See the section on Scripts for more information.

#### media/models/
Used for 3d models of weapons, vehicles etc.

#### media/textures/
Icons, 3d model textures and the like go in here, in `.png` format. Pay attention to the naming scheme: Item icons should use the `Item_` prefix.

#### media/sounds/
Sound files go in here, in `.wav` or `.ogg` format.

#### media/lua/
If your mod includes any .lua scripts, you will need this folder as well as at least one of the subfolders listed below. Any .lua files need to go in the subfolders, not directly in this one.

#### media/lua/shared/
Used for lua scripts shared by client and server-side logic. These are the first Lua scripts that get loaded.

#### media/lua/client/
Used for client-side scripts. UI elements, context menus timed actions and the like. These get loaded after any 'shared' lua scripts.

#### media/lua/server/
Used for server-side scripts. Item spawning, core farming, weather and other server-side events. These only get loaded when the game is actually started (loading a save, starting a server, etc).

----------------------------------------------------------------------------------

## The Scripts
Items, recipes, vehicles and similar stuff is defined in .txt files in the `media/scripts/` directory. These files are seperated into various `{ block }` types. Within these blocks exist items properties: its name, how much it weighs, what type of item it is, how much time recipes take, what ingredients are required, how much horsepower a vehicle has, etc.

Each of the various block types has different attribute/value types, and syntax can vary slightly from other block types (items vs recipes), but the overall format is the same:

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
Most (not all) vanilla items use the module `Base`. You are free to use this module as well but be aware defining things like items that already exist in the `Base` module you will **overwrite** them.  
If you have lots of items or don't want to conflict with any existing items or other mods its best to use a custom (or multiple) module names. Common practice is to use the same name as the mod ID.  
Sometimes for basic mods or compatibility reasons it may be better to use a existing module name such as `Base`. Creating a extra module namespace for a few simple items is not always the best option: unnecessary  modules are very minor performance hit.

### The imports block
Often for recipes and other blocks you will need to reference items from other modules. Normally when a item exists in a different script module, you have to use the full name such as `Base.Nails` to reference it. By declaring a `imports` block you can skip the module prefix on the item and just use `Nails`
```
imports {
    Base
}
```
*Note: when loading mods scripts, PZ will log a warning to the console if you do not import Base. This warning can be ignored unless you intented to import.*


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

Lua is designed to be a simple and lightweight language, without a lot of bells and whistles. It is easy to learn, its syntax is simple and there are a minimal number of built-in functions and modules.  It does include some concepts that can initially throw people coming from other languages.

**_TODO: Include a brief lua tutorial here, and links to more online._**

### Zomboid's Lua Component
Those familiar with Lua know there can be minor differences between versions (5.1, 5.2, 5.3) primarly in the modules and methods contained. Zomboid's Lua is not 'pure' Lua, it is modified Kahlua, a Lua interpreter writen entirely in Java. It
lacks the performance of pure Lua, but provides a almost seamless integration of the Java and Lua components. Not all Lua modules are implemented such as `io.*` and `os.*`  
Java classes and functions are 'exported' to Lua providing normal access to them, however things like java reflection will not work. These classes and functions are manually specified for export by the Zomboid developers, thus the Lua does not have full access to Java and is at least partially sandboxed.

### The Vanilla Lua
**_TODO: Brief outline of what aspects of the game are controlled by lua, and where these aspects can be found in the files._**

### Zomboid's API
**_TODO: Describe major parts of the api, common globals and the event system._**

### Performance Tips
**_TODO: Outline common performance mistakes, variable scoping and troublesome event callbacks._**

### Overwriting Vanilla Code
**_TODO: File vs Function overwrites and compatibility issues._**

### Decompiling The Java
**_TODO: Brief outline of installing and working with a decompiler._**

### Code Snippets
**_TODO: Commonly used bits._**


----------------------------------------------------------------------------------

## The Maps
**_TODO: Describe the tools and process for creating custom maps._**


----------------------------------------------------------------------------------

## The Models
**_TODO: Describe process for loading importing and loading custom 3d models in game._**
