# Project Zomboid Modding Guide

**_This document is very much a Work-In-Progress._**

**_The goal is to create a solid one-stop-spot for how to mod TheIndieStone's [Project Zomboid](https://projectzomboid.com), covering all aspects of modding, tools required, tips and tricks, as well as examples and code snippets for the more common tasks._**

**_Much of the current modding information is in outdated tutorials, scattered in forum posts, and locked away in the brains of modders and other community members. Ideally by hosting a universal guide on Github, it will be collected all in one spot and anyone can contribute new information, corrections or updates as PZ's API changes over time._**

## Intro
"Modding" is a vague term covering multiple areas. Many of these areas sometimes overlap but should be considered separate as they require different skill sets and knowledge.

### Adding basic items and recipes
This can be done with little (or none) coding knowledge and minimal tools. Item and recipe definitions are inside standard .txt documents, although these txt files must conform to a specific syntax and style.  

### Code changes, advanced items and recipes
Being able to add on to or edit Zomboid's code is a major bonus, and where the real power of mods comes in. PZ is coded in a mixture of Java (the main engine) and Lua (the moddable components).  
Knowledge of the [Lua programming language](https://www.lua.org/) is HIGHLY advised. You'll save yourself a lot of grief and effort learning Lua before diving into this area. Fortunately Lua is a simple language by design, and relatively easy to learn.  
The internet is scattered with tutorials.

### 3d Models
For build 40 and below, 3d art for Project Zomboid primarily consists of weapon models and vehicles. At this time all 3d mods fall into one of these 2 types.  Models are in a custom .txt format.
The processes of exporting from your modelling app and importing in PZ is fairly simple, though initial positioning, scaling and rotation can take a bit of tweaking. Minor coding is required to tell Zomboid to load the custom models.

Build 41 is expected to bring additional 3d models such as clothing and static items used in timed actions, and a different model format.

### Textures


### Map Making


----------------------------------------------------------------------------------

## Tools (software)
**_This section should describe the various software tools required for various areas, and provide url links for the more common software used_**

For the most part, you are free to use what ever tools you want. The exception is custom map creation, where you are currently limited to the tools released by TIS.

### Text Editors:
Used in all areas of modding. [Notepad++](https://notepad-plus-plus.org) is often the preferred editor for windows OS's (it also runs amazingly well on linux with wine)

### 3d Modelling:
Used for the creation of custom items, vehicles and weapons. [Blender](https://blender.org) is the common choice as its free, and import/export scripts for PZ's model format exist. Any 3d app can be used to make your models, with blender doing the final conversion.

### Image Editor:
If your doing custom icons, textures, maps or 3d models you'll need one of these. Paint.NET, Photoshop and [GIMP](https://gimp.org) are the most used, but whatever supports the .png format will work.

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
Items, recipes, vehicles and similar stuff is defined in .txt files in the `media/scripts/` directory. These files are separated into various `{ block }` types. Within these blocks exist items properties: its name, how much it weighs, what type of item it is, how much time recipes take, what ingredients are required, how much horsepower a vehicle has, etc.

Each of the various block types has different attribute/value types, and syntax can vary slightly from other block types (items vs recipes), but the overall format is the same:

A basic example:
```
/** This line is a comment. anything inside these brackets is ignored **/
module MyMod {
    imports {
        Base
    }
    /** Comments can be multiline like this
        and can appear anywhere in the file, even in the
        middle of a { block }
    **/
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
*Note: when loading mods scripts, PZ will log a warning to the console if you do not import Base. This warning can be ignored unless you intended to import.*


### The item block
**_TODO: explain the item block, the various types of items and valid property differences between types._**

### The recipe block
Recipes are one form of crafting in Zomboid. They take input 'ingredients' (food, tools, items, etc) and produce a output item.   
Pay attention to the syntax differences between `item` blocks and `recipe` blocks. Items use the familiar `property = value,` syntax while recipes use `property:value,` and `=` are used strictly for item counts.

In its most basic form, a recipe has the following structure:
```
recipe Open Box of Nails
{
    NailsBox,

    Result:Nails=20,
    Sound:PutItemInBag,
    Time:5.0,
}
```
The first line `NailsBox,` is our input item, followed by a blank line. Input items are destroyed on recipe creation unless `keep` has been specified (more on that later).  
`Result:Nails=20,` is obviously the output item, `=20` is the number of this item to give the player. **Note** this number is multiplied by the resulting item's `count =` property in its `item` block definition. Nails have `count = 5`, so our end result is `20 * 5 = 100 Nails`  
`Sound` is the audio clip to play and `Time` how long this action takes.

Naturally the reverse of this recipe is:
```
recipe Place Nails in Box
{
    Nails=100,

    Result:NailsBox,
    Sound:PutItemInBag,
    Time:5.0,
}
```
A slightly more advanced recipe that uses multiple ingredients which can be different and calls lua functions:
```
recipe Slice Fillet
{
    keep KitchenKnife/HuntingKnife,
    Bass/Catfish/Perch/Crappie/Panfish/Pike/Trout,

    Result:FishFillet=2,
    Sound:SliceMeat,
    Time:50.0,
    OnTest:CutFish_TestIsValid,
    OnCreate:CutFish_OnCreate,
    Category:Cooking,
    OnGiveXP:Give10CookingXP,
}
```
Here you can see the ingredients are a knife and a fish. `keep` is declared before the knife to ensure it isn't destroyed after, and `/` is used to separate valid items (ie: KitchenKnife **OR** HuntingKnife).  
`OnTest` and `OnCreate` are *global* Lua functions called, the first for testing if the recipe is valid, and the second when the recipe has finished.  
`Category` controls which tab this recipe appears on in the player's Build Window. You can easily define custom tabs simply by using custom values here.  
`OnGiveXP` is another *global* Lua function. There are many predefined for giving various xp values to different skills, or you can create your own function.

### The evolvedrecipe block

### The fixing block

### The sound block
PZ build 40 introduced new audio code to deal with issues in multiplayer. All sounds now need to be defined in a `sound` block to have them heard by players farther then 20 tiles. Defining a sound here also includes it in PZ's options advanced audio tab.  

If you don't have a sound listed here, the first time it plays it will get added to the audio tab, its MP radius will get set to 20 and a message will appear in the console: `WARNING: no GameSound called "MySound", adding a new one`

The vanilla shotgun blast is defined in the `item` block as:
```
    SwingSound	=	FirearmShotgun,
    SoundRadius	=	200,
```
And in the `sound` block as:
```
sound FirearmShotgun
{
    category = Item,
    clip
    {
        event = Weapons/Firearm/shotgun2,
        distanceMax = 200, /* SoundRadius in items.txt */
    }
}
```
The `SwingSound` must match the sound name, while the `SoundRadius` in the item block is *only the zombie agro distance*, `distanceMax` is the range other players will hear it.  
`event` only applies to sound clips that are in the vanilla soundbanks, for custom sounds we need to use `file`
```
sound Remington870
{
    category = Item,
    clip
    {
        file = media/sound/Remington870.ogg,
        distanceMax = 200,
    }
}
```


### The vehicle block
**_TODO: Outline syntax differences, template files, and important properties._**

----------------------------------------------------------------------------------

## The Code

### Introduction
Project Zomboid's code is a 2-language system, using both Java and Lua. The main engine and API features are primarily Java,
with large chunks of the logic in Lua.  Most modding is done to the Lua component, while the Java component is generally considered non-moddable.

_Note: The java can be modified, but requires knowledge of java, decompiling and recompiling the .class files. Mods with Java edits also can not be installed using normal methods, these files need to go directly to Zomboid's install directory. Modifying the Java is beyond the scope of this document._

Unlike the Java, Lua can be edited with the text editor of your choice and requires no other tools. _Those familiar with basic Lua can skip ahead._

Lua is designed to be a simple and lightweight language, without a lot of bells and whistles. It is easy to learn, its syntax is simple and there are a minimal number of built-in functions and modules.  It does include some concepts that can initially throw people coming from other languages.

### New To Programming
**_TODO: Include a brief lua tutorial here, and links to more online._**

### New To Lua
**_TODO: Highlight the differences and concepts in Lua compared to more common languages._**

### Zomboid's Lua Component
Those familiar with Lua know there can be minor differences between versions (5.1, 5.2, 5.3) primarily in the modules and methods contained. Zomboid's Lua is not 'pure' Lua, it is modified Kahlua, a Lua interpreter writen entirely in Java. It
lacks the performance of pure Lua, but provides a almost seamless integration of the Java and Lua components. Not all Lua modules are implemented such as `io.*` and `os.*`  

Java classes and functions are 'exported' to Lua providing normal access to them, however things like java reflection will not work. These classes and functions are manually specified for export by the Zomboid developers, thus the Lua does not have full access to Java and is at least partially sandboxed.

One thing to bear in mind is functions called from Lua will often return Java objects. Java Arrays and Lists in particular can catch people off guard, since they are quite different from a lua table list. For example iterating over a basic table list:
```lua
local tbl = {'a', 'b', 'c'}
for index, value in ipairs(tbl) do
    print(index, value)
end
-- or we could do this:
for index=1, #tbl do -- use # to get the size of the table
    print(index, tbl[index])
end

-- prints:
-- 1    a
-- 2    b
-- 3    c
```
Now say we have a player object want a list of the known recipes:
```lua
local known = player:getKnownRecipes() -- returns a Java List, not a Lua table.
```
Unlike a normal table list, our the first entry in the Java List is index 0 (not 1), and `ipairs` will no longer work, nor will `#` to get the size, or `[]` to get the value at a specified index. For this we need to use the methods in Java's List class `:size()` and `:get()`
```lua
for index=0, known:size() -1 do
    print(index, known:get(index))
end
```
The upside of this is Java Lists and Arrays have shortcut methods not available in standard Lua tables, like using `:contains("some string")` to test if a string is in the List. Doing this with a Lua table requires iterating over the table manually checking each index.

### The Vanilla Lua
**_TODO: Brief outline of what aspects of the game are controlled by lua, and where these aspects can be found in the files._**

### Zomboid's API
**_TODO: Describe major parts of the api, common globals and the event system._**

### Decompiling The Java
**_TODO: Brief outline of installing and working with a decompiler._**

### Overwriting Vanilla Code
A big advantage with large parts of the game logic in Lua means vanilla code can **replaced**, not just added on to. Meaning core parts of the game logic can be modified. Overwriting vanilla code comes with risks though:

**_The more you replace the more likely it is to break across Zomboid versions, and more likely to be incompatible with other mods_**

A common mistake is blindly copying a vanilla file into a mod with a few minor edits. If that file changes next Zomboid update, you need to check and redo your work. If some other mod also has a modified version of that file, then one mod will overwrite the other. Following a few basic rules will drastically help compatibility and reduce potential breakage:

**Always overwrite as little as possible to achieve your goal. Don't replace a whole file if you can do it by overwriting a single function.**  

Using custom tooltips for items as a example (many mods include custom tooltips), the file that handles these tooltips is `client/ISUI/ISToolTipInv.lua` and the method that draws the tooltip is `ISToolTipInv:render()`.  
So we could just copy that whole file and edit our render function, but that's likely to cause compatibility issues and entirely pointless anyways. `ISToolTipInv` is a *global* lua table, we can simply go:
```lua
function ISToolTipInv:render()
    -- ... some custom code ...
end
```
This can be done from any file, without having to replace the original.

**Store the original function in a local variable. If your overwrite doesn't need to run, call the original.**

To solve the problem that many different types of mods want to overwrite the `:render()` method at the same time (ie: weapon and food mods), always store the functions you overwrite in a local variable, so you can call it if you dont need to run your custom code. A food related mod doesn't need to show custom tooltips for weapons and vice versa.  
Run a check early in your overwrite to see if its valid or if you can use the original callback.
```lua
local original_render = ISToolTipInv.render
function ISToolTipInv:render()
    if not CONDITION then
        original_render(self)
    end
    -- ... some custom code ...
end
```
By calling the original 2 mods overwriting the same function have higher chances of being compatible, and it will be more resilient to minor changes in the original vanilla code.

Obviously This not only applies to tooltips, but all overwrites.

### Performance Tips
**_TODO: Outline common performance mistakes, variable scoping and troublesome event callbacks._**

### Code Quality Tips
**_TODO: Outline consistent syntax, indentation levels, mixing tabs/spaces and documenting._**

### Code Snippets
**_TODO: Commonly used bits._**


----------------------------------------------------------------------------------

## Translations
**_TODO: Describe the process of creating and supporting mod translations._**


----------------------------------------------------------------------------------

## The Maps
**_TODO: Describe the tools and process for creating custom maps._**


----------------------------------------------------------------------------------

## The Models
**_TODO: Describe process for loading importing and loading custom 3d models in game._**
